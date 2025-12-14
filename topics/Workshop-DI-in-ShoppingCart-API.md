# [Workshop] DI in ShoppingCart API

Imagine an e-commerce system.

1. The **CartService** stores items added by the user.
2. The **DiscountService** provides dynamic discounts.
3. The **AppConfigService** provides global configuration (like tax rate, delivery fee).

## Create a New ASP.NET Core Web API Project

First, create a new ASP.NET Core Web API Project, name it ShoppingCartAPI.

### Define Models

Create a new folder, Models, at the project root directory. Then, add the following classes inside the Models folder.

**`CartItem.cs`**

Represents an item added to the cart, including _ProductId_, _Name_, _Price_, and _Quantity_. Create a class file named **CartItem.cs** within the **Models** folder.

```c#
namespace ShoppingCartAPI.Models
{
    public class CartItem
    {
        public int ProductId { get; set; }
        public string ProductName { get; set; } = string.Empty;
        public decimal Price { get; set; }
        public int Quantity { get; set; }
    }
}
```

**`CartSummary.cs`**

Represents the final computed values of a cart: Subtotal, Discount, Tax, Delivery Fee, and Final Total. Create a class file named **CartSummary.cs** within the **Models** folder.

```C#
namespace ShoppingCartAPI.Models
{
    public class CartSummary
    {
        public decimal SubTotal { get; set; }
        public decimal Discount { get; set; }
        public decimal Tax { get; set; }
        public decimal DeliveryFee { get; set; }
        public decimal Total { get; set; }
    }
}
```
### Define Services

Create a new folder `Services` and `Interfaces` at the project root directory. Then, add the following classes and interfaces inside the Services and interfaces folder respectively.

#### Singleton Service → `AppConfigService`

Create an interface named `IAppConfigService.cs` within the Services/Interface folder.

```C#
namespace ShoppingCartAPI.Services.Interfaces
{
    public interface IAppConfigService
    {
        decimal GetTaxRate();
        decimal GetDeliveryFee(decimal orderAmount);
    }
}

```
##### AppConfigService

This class provides application-wide configuration data, such as the **GST tax rate** and **delivery fee**, based on the cart amount. These values don’t change per request or user. Create a class file named **AppConfigService.cs** within the **Services** folder.

```C#
using ShoppingCartAPI.Services.Interfaces;

namespace ShoppingCartAPI.Services
{
    public class AppConfigService : IAppConfigService
    {
        private readonly decimal _taxRate = 0.18m; // GST rate = 18%

        public AppConfigService(ILogger<AppConfigService> logger)
        {
            // Log only once since Singleton → single instance shared across app
            logger.LogInformation("AppConfigService (Singleton) instance created.");
        }

        public decimal GetDeliveryFee(decimal orderAmount)
        {

            if (orderAmount < 500)
                return 50;  // Flat ₹50 for small orders
            else if (orderAmount >= 500 && orderAmount <= 2000)
                return 30;  // Reduced fee for mid-sized orders
            else
                return 0;   // Free delivery for big orders
        }

        public decimal GetTaxRate()
        {
            return _taxRate;
        }
    }
}

```

**Why Singleton?**
* It holds static, read-mostly configuration.
* It’s shared across all users and requests.
* Saves memory and avoids unnecessary object creation.

#### Scoped Service → `CartService`

Stores the **shopping cart** for a single user request. Create an interface named `ICartService.cs` within the **Services/Interfaces** folder.

```C#
using ShoppingCartAPI.Models;

namespace ShoppingCartAPI.Services.Interfaces
{
    public interface ICartService
    {
        void AddItem(CartItem item);
        List<CartItem> GetItems();
        void ClearCart();
    }
}

```

##### CartService

This service manages the user’s shopping cart for the current HTTP request. It fetches the cart from IMemoryCache, adds/updates items, and clears the cart as needed. Create a class file named **CartService.cs** within the **Services** folder.

```C#
using Microsoft.Extensions.Caching.Memory;
using ShoppingCartAPI.Models;
using ShoppingCartAPI.Services.Interfaces;

namespace ShoppingCartAPI.Services
{
    public class CartService : ICartService
    {
        private readonly IMemoryCache _cache;
        private readonly string _userId;
        private readonly ILogger<CartService> _logger;

        public CartService(IMemoryCache cache, IHttpContextAccessor httpContextAccessor, ILogger<CartService> logger)
        {
            _cache = cache;
            _userId = httpContextAccessor.HttpContext?.Request.Headers["UserId"].FirstOrDefault() ?? "guest";
            _logger = logger;
            _logger.LogInformation("CartService (Scoped) instance created for user {UserId}", _userId);
        }

        public void AddItem(CartItem item)
        {
            _logger.LogInformation("Adding item for user {UserId}: {ProductName}", _userId, item.ProductName);
            
            var cart = _cache.GetOrCreate(_userId, entry =>
            {
                _logger.LogInformation("Creating new cart for user {UserId}", _userId);
                entry.SlidingExpiration = TimeSpan.FromMinutes(30);
                return new List<CartItem>();
            })!;

            var existing = cart.FirstOrDefault(x => x.ProductId == item.ProductId);
            if (existing != null)
            {
                existing.Quantity += item.Quantity;
                _logger.LogInformation("Updated quantity for ProductId {ProductId}, new quantity: {Quantity}", item.ProductId, existing.Quantity);
            }
            else
            {
                cart.Add(item);
                _logger.LogInformation("Added new item. Cart now has {Count} items", cart.Count);
            }

            _cache.Set(_userId, cart);
            _logger.LogInformation("Cart saved to cache for user {UserId}, total items: {Count}", _userId, cart.Count);
        }

        public List<CartItem> GetItems()
        {
            _logger.LogInformation("Getting items for user {UserId}", _userId);
            
            if (_cache.TryGetValue(_userId, out List<CartItem>? cart))
            {
                _logger.LogInformation("Cart found for user {UserId}, contains {Count} items", _userId, cart?.Count ?? 0);
                return cart ?? new List<CartItem>();
            }

            _logger.LogWarning("No cart found in cache for user {UserId}", _userId);
            return new List<CartItem>();
        }

        public void ClearCart()
        {
            _logger.LogInformation("Clearing cart for user {UserId}", _userId);
            _cache.Remove(_userId);
        }
    }
}

```

**Why Scoped?**
* A new instance is created per request.
* It ensures request isolation, but the user cart persists across requests via IMemoryCache (singleton).
* Allows logging and handling per-user state safely within a request.

#### Transient Service → `DiscountService`

Provides a **tier-based discount logic** (5%, 10%, 15%) depending on the subtotal amount. Create an interface named `IDiscountService.cs` within the **Services/Interfaces** folder.

```C#
namespace ShoppingCartAPI.Services.Interfaces
{
    public interface IDiscountService
    {
        decimal CalculateDiscount(decimal amount);
    }
}
```

##### DiscountService

Calculates discounts based on order amount (5%, 10%, 15%). Each injection creates a new instance so that you can see multiple calculations in the same request. Create a class file named **DiscountService.cs** within the **Services** folder.

```C#
using ShoppingCartAPI.Services.Interfaces;

namespace ShoppingCartAPI.Services
{
    public class DiscountService : IDiscountService
    {
        public DiscountService(ILogger<DiscountService> logger)
        {
            // Log whenever a new DiscountService instance is created
            // (helps visualize Transient lifetime → multiple per request possible)
            logger.LogInformation("DiscountService (Transient) instance created.");
        }
        public decimal CalculateDiscount(decimal amount)
        {
            // Tier-based discount calculation
            decimal discountPercent = 0;
            if (amount >= 5000 && amount <= 20000)
                discountPercent = 5;   // 5% discount for mid-level orders
            else if (amount > 20000 && amount <= 50000)
                discountPercent = 10;  // 10% discount for higher orders
            else if (amount > 50000)
                discountPercent = 15;  // 15% discount for premium orders
            // Calculate discount amount
            decimal discount = amount * discountPercent / 100;
            return discount;
        }
    }
}
```

**Why Transient?**
* It is stateless and lightweight.
* We want a new instance every time (even within the same request), especially to show how different injections behave independently.
* Suitable for one-off calculations.

#### Scoped Service → `ICartSummaryService`

Create an interface named **ICartSummaryService.cs** within the **Services/Interfaces** folder.

```C#
using ShoppingCartAPI.Models;

namespace ShoppingCartAPI.Services.Interfaces
{
    public interface ICartSummaryService
    {
        CartSummary GenerateSummary();
    }
}

```

##### CartSummaryService

Generates the **final cart summary** (Subtotal, Discount, Tax, Delivery Fee, and Total). Depends on:

* CartService (Scoped → per-request cart data),
* DiscountService (Transient → fresh calculations),
* AppConfigService (Singleton → global tax/delivery rules).

Create a class file named `CartSummaryService.cs` within the Services folder.

```C#
using ShoppingCartAPI.Models;
using ShoppingCartAPI.Services.Interfaces;

namespace ShoppingCartAPI.Services
{
    public class CartSummaryService : ICartSummaryService
    {
        // Dependencies injected via constructor
        private readonly ICartService _cartService;       // To fetch items already added to the cart
        private readonly IDiscountService _discount1;     // First transient discount service
        private readonly IDiscountService _discount2;     // Second transient discount service (for demo showing new instance)
        private readonly IAppConfigService _config;       // For global configuration (tax rate, delivery fee)
        // Constructor injection - services are provided by DI container
        public CartSummaryService(
            ICartService cartService,
            IDiscountService discount1,
            IDiscountService discount2,
            IAppConfigService config)
        {
            _cartService = cartService;
            _discount1 = discount1;
            _discount2 = discount2;
            _config = config;
        }
        // Main method to calculate and return cart summary
        public CartSummary GenerateSummary()
        {
            // Fetch all cart items from the cart service
            var items = _cartService.GetItems();
            // Calculate subtotal = sum of price * quantity for all items
            decimal subTotal = items.Sum(i => i.Price * i.Quantity);
            // Apply discounts using two transient instances
            // (each transient service will likely give different results,
            // to demonstrate lifetime differences)
            decimal discount1 = _discount1.CalculateDiscount(subTotal);
            decimal discount2 = _discount2.CalculateDiscount(subTotal);
            // Calculate tax using the Singleton AppConfigService
            decimal tax = subTotal * _config.GetTaxRate();
            // Calculate delivery fee using dynamic business rules
            decimal delivery = _config.GetDeliveryFee(subTotal);
            // Build and return the summary object
            return new CartSummary
            {
                SubTotal = subTotal,                              // Original total before tax/discounts
                Discount = (discount1 + discount2) / 2,           // Average of both discounts (demo purpose)
                Tax = tax,                                        // Calculated tax
                DeliveryFee = delivery,                           // Delivery fee based on subtotal
                Total = subTotal - ((discount1 + discount2) / 2)  // Net total = subtotal - discount + tax + delivery
                        + tax
                        + delivery
            };
        }
    }
}

```

**Why Scoped**?
* It depends on the scoped CartService, transient DiscountService, and singleton AppConfigService.
* Scoped ensures a **fresh summary per request**, yet internally respects the correct lifetimes of its dependencies.

### Register Services in Program.cs

Now, we need to register the services with a proper lifetime. So, please update the `Program.cs` class file as follows:

```C#
using ShoppingCartAPI.Services;
using ShoppingCartAPI.Services.Interfaces;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.

builder.Services.AddControllers();
// Learn more about configuring OpenAPI at https://aka.ms/aspnet/openapi
builder.Services.AddOpenApi();

// Service Register lifetimes
// Register AppConfigService as Singleton
// One instance is created and shared across the entire application lifetime
//    - Good for global config like tax rate, delivery fee
//    - Created once, reused everywhere
builder.Services.AddSingleton<IAppConfigService, AppConfigService>();
// Register CartService as Scoped
// One instance per HTTP request
//    - Each request gets its own CartService instance
//    - Items stored in CartService are isolated to that request
//    - Demonstrates per-request lifetime
builder.Services.AddScoped<ICartService, CartService>();
// Register DiscountService as Transient
// A new instance is created every time it is requested
//    - Even within the same request, multiple injections create different objects
//    - Great for lightweight, stateless operations like discount calculations
builder.Services.AddTransient<IDiscountService, DiscountService>();
// Register CartSummaryService as Scoped
// New instance per HTTP request
//    - Depends on CartService, DiscountService, AppConfigService
//    - Calculates cart totals (subtotal, discount, tax, delivery, final total)
//    - Scoped makes sense because summary is tied to the current cart/request
builder.Services.AddScoped<ICartSummaryService, CartSummaryService>();
// Register HttpContextAccessor
// Provides access to HttpContext (e.g., reading headers like "UserId")
//    - Needed for CartService to know which user's cart to manage
builder.Services.AddHttpContextAccessor();
// Register In-Memory Cache
// Provides IMemoryCache (Singleton under the hood)
//    - Used by CartService to persist cart data across requests for a given user
//    - Supports expiration policies (e.g., cart expires after 30 minutes of inactivity)
builder.Services.AddMemoryCache();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
}

app.UseHttpsRedirection();

app.UseAuthorization();

app.MapControllers();

app.Run();

```

### Creating Controllers

#### CartController.cs

Handles the HTTP endpoints:

* POST /add to add an item to the cart
* GET /items to view cart contents
* GET /summary to calculate totals
* DELETE /clear to empty the cart

It demonstrates how services with different lifetimes work together to build a real-time user experience. So, create an Empty API Controller named CartController within the Controllers folder.

```C#
using Microsoft.AspNetCore.Mvc;
using ShoppingCartAPI.Models;
using ShoppingCartAPI.Services.Interfaces;

namespace ShoppingCartAPI.Controllers
{
    [Route("api/[controller]")]
    public class CartController : Controller
    {
        // Injected dependency for cart operations (Scoped service)
        private readonly ICartService _cartService;
        // Constructor Injection
        // ASP.NET Core automatically resolves ICartService from the DI container
        public CartController(ICartService cartService)
        {
            _cartService = cartService;
        }

        // POST: api/cart/add
        // Adds an item to the user's cart
        [HttpPost("add")]
        public IActionResult AddItem([FromBody] CartItem item)
        {
            // Call cart service to add item to the in-memory cache
            _cartService.AddItem(item);
            // Return success response with a message
            return Ok(new { Message = $"{item.ProductName} added to cart." });
        }
        // GET: api/cart/items
        // Returns all items currently in the user's cart
        [HttpGet("items")]
        public IActionResult GetItems()
        {
            // Fetch items from cart service
            return Ok(_cartService.GetItems());
        }
        // GET: api/cart/summary
        // Returns the calculated cart summary (subtotal, discounts, tax, delivery fee, total)
        [HttpGet("summary")]
        public IActionResult GetSummary([FromServices] ICartSummaryService summaryService)
        {
            // Here, CartSummaryService is injected per-request (Scoped)
            // It internally uses:
            //   - ICartService (Scoped, manages cart data)
            //   - IDiscountService (Transient, two different instances for discount calculations)
            //   - IAppConfigService (Singleton, global tax & delivery fee rules)
            var summary = summaryService.GenerateSummary();
            // Return summary object as JSON response
            return Ok(summary);
        }
        // DELETE: api/cart/clear
        // Clears all items from the user's cart
        [HttpDelete("clear")]
        public IActionResult ClearCart()
        {
            // Clear user's cart from cache
            _cartService.ClearCart();
            // Return success response
            return Ok(new { Message = "Cart cleared." });
        }
    }
}

```

### Testing ShoppingCartAPI in Postman

##### Request 1: Add Laptop

POST /api/cart/add

```json
{
  "productId": 1,
  "productName": "Laptop",
  "price": 60000,
  "quantity": 1
}
```

Here:

* A **Scoped CartService** instance is created for this request.
* The Laptop item is added to the cart (stored in **IMemoryCache**, keyed by UserId).
* **Log Output**:
  * CartService (Scoped) instance created for user user123
  * If this is the very first request, you’ll also see: AppConfigService (Singleton) instance created.

##### Request 2: Add Mobile

POST /api/cart/add

```JSON
{
  "productId": 2,
  "productName": "Mobile",
  "price": 10000,
  "quantity": 2
}
```

##### View Items

GET /api/cart/items

Here,

* Creates another **Scoped CartService** instance.
* Retrieves cart items from **IMemoryCache**.
* You’ll see **both Laptop + Mobile**, because cache persists data beyond request lifetime.
* **Log Output**:
  * CartService (Scoped) instance created for user user123.

##### Get Cart Summary

GET /api/cart/summary

Here,

* Creates a new Scoped CartService instance.
* Fetches all cart items (Laptop + Mobile).
* Two Transient DiscountService instances are created:
  * _discount1
  * _discount2
* Each calculates a discount based on the subtotal (tiered: 5%, 10%, 15%).
* Singleton AppConfigService is reused (no new log).
  * It now calculates the delivery fee dynamically:
    * Subtotal < 500 → ₹50
    * Subtotal 500–2000 → ₹30
    * Subtotal > 2000 → Free delivery (₹0)

**Log Output:**

* CartService (Scoped) instance created for user user123
* DiscountService (Transient) instance created.
* DiscountService (Transient) instance created.

#### Best Practices

* Choose **Singleton** for shared, thread-safe, read-mostly resources you want to reuse.
* Choose **Scoped** for anything that logically lives for a single HTTP request.
* Choose **Transient** for small, stateless helpers you don’t mind recreating often.

Understanding these lifetimes is crucial for building efficient and bug-free ASP.NET Core Web APIs. Improper use can cause:

* Memory leaks (e.g., injecting scoped into a singleton),
* Unwanted shared states, or
* Performance issues.
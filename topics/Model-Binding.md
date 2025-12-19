# Model Binding

Model Binding in ASP.NET Core Web API is the process that automatically maps incoming HTTP request data, from routes, query strings, headers, forms, or request body, to controller action parameters or model objects.

**Instead of manually reading and parsing values from HttpContext.Request, the framework intelligently handles this for you, converting raw input into strongly-typed** .NET objects. This makes your code cleaner, safer, and easier to maintain by removing repetitive parsing logic and ensuring consistent validation across all API endpoints

This feature abstracts the complexities of extracting and converting raw request data from HttpRequest into strongly-typed .NET objects. ASP.NET Core uses model binding to map data from multiple sources:

* **Query Strings**: P**arameters appended** to the URL.
* **Route Data**: Parameters defined in the **URL path**.
* **Request Body**: Payload data, often in **JSON** or **XML** format (commonly for **POST**, **PUT**, or **PATCH** requests).
* **Headers**: Custom **data sent within HTTP headers**.

> Key Idea: Model binding **converts raw request data (text, JSON, form, etc.) into .NET objects, making APIs cleaner, type-safe, and easy to maintain**. So, you don’t need to parse request data manually; the framework does it for you.

### What are the Model Binding Techniques Used in ASP.NET Core Web API?

ASP.NET Core provides several techniques for model binding depending on the **source of the data**.
You can explicitly tell the framework where to look for data using specific attributes.

#### `[FromRoute]` – Binding from Route Parameters

The `[FromRoute]` is used when data is part of the URL Route itself. The model binder reads the value from the route template defined in your controller’s endpoint. It’s commonly used for identifying a resource by its unique **ID**, such as `/api/products/5`.

**Syntax:**

```C#
[HttpGet(“products/{id}”)]
public IActionResult GetProduct([FromRoute] int id)
```
**Use Cases:**
* When your API endpoint includes dynamic route segments.
* To extract resource identifiers like userId, orderId, or productId.
* When designing RESTful URLs, such as /api/users/{id} or /api/orders/{orderId}.

#### `[FromQuery]` – Binding from Query String

The `[FromQuery]` tells ASP.NET Core to **bind data from the Query String Parameters appended to the URL.** It’s best suited for **optional parameters**, **filters**, **pagination**, or **sorting options** where you want to keep the URL readable and flexible.

**Syntax:**

```C#
[HttpGet(“products”)]
public IActionResult SearchProducts([FromQuery] string category, [FromQuery] int page = 1)
```

**Use Cases:**
* Implementing **filtering**, **sorting**, or **pagination** (`/api/products?category=Electronics&page=2`).
* Passing optional parameters without changing the route structure.
* **Searching**, **filtering**, or **listing data dynamically**.

#### `[FromBody]` – Binding from Request Body

The `[FromBody]` attribute **is used when data is sent in the HTTP Request Body**, usually as **JSON** or **XML**. ASP.NET Core uses input formatters **(like JSON or XML) to deserialize the body into a strongly-typed object**. Only one parameter per action can use [FromBody] because the request body can be read only once.

**Syntax:**

```C#
[HttpPost(“add”)]
public IActionResult AddProduct([FromBody] ProductDTO product)
```

**Use Cases:**
* When sending complex objects or large data in POST/PUT requests.
* For creating or updating records (e.g., new user, new product).
* When consuming APIs with JSON request bodies

#### [FromHeader] – Binding from Request Headers
`[FromHeader]` binds values from specific HTTP request headers, such as **Authorization**, **Accept-Language**, or **custom headers** used for security or tracking.

**Syntax:**

```C#
[HttpGet(“userinfo”)]
public IActionResult GetUser([FromHeader(Name = “X-User-Id”)] string userId)
```

**Use Cases:**
* Reading **authentication tokens**, **version information**, or **API keys from headers**.
* Custom tracking IDs or correlation IDs.
* Language or localization preferences via headers.

#### `[FromServices]` – Binding from Dependency Injection Container
The `[FromServices]` attribute tells ASP.NET Core to resolve a parameter from the **Dependency Injection (DI) Container instead of the request**. It’s not about client-supplied data; it’s about injecting services (e.g., **repositories**, **loggers**, **configuration**).

That means `[FromServices]` allows us to inject Services Registered in the DI Container directly into an action method. It’s ideal when we need a service just for that specific action without adding it to the constructor.

**Syntax:**

```C#
[HttpGet(“config”)]
public IActionResult GetConfig([FromServices] IConfiguration config)
{
      var env = config[“ASPNETCORE_ENVIRONMENT”];
      return Ok(env);
}
```

**Use Cases:**
* Accessing a lightweight or specialized service in one controller method.
* Avoiding constructor clutter for infrequently used services.
* Logging, Caching, or health-check services.

### Manual Data Reading in ASP.NET Core Web API (Without Model Binding)

We will create a simple in-memory Product Management API that performs operations like reading product details, adding new products, and checking system health, all by manually extracting and validating request data. Clients can:

1. Retrieve product details by route.
2. Search products by query parameters.
3. Fetch all active products using a header-based authentication key.
4. Add a new product via a JSON request body.
5. Apply a discount to a product (combination of route, query, and header).

Everything is handled manually, without model binding or **[ApiController]**.

##### Create the Project
First, create a new ASP.NET Core Web API Project, name it ModelBindingDemo.

##### Create an In-Memory Product Store
First, create 2 folders named Models and Services in the Project root directory. Inside the Models folder, create a class file named Product.cs.

```C#
namespace ModelBindingDemo.Models
{
    public class Product
    {
        public int Id { get; set; }
        public string? Name { get; set; }
        public decimal Price { get; set; }
        public string? Category { get; set; }
        public bool IsActive { get; set; }
        public int Stock { get; set; }
        public double Rating { get; set; }
        public DateTime CreatedDate { get; set; }
    }
}
```

##### Creating Product Service Interface
Create a class file named IProductService.cs within the Services folder.

```C#
using ModelBindingDemo.Models;
namespace ModelBindingDemo.Services
{
    public interface IProductService
    {
        IEnumerable<Product> GetAll();
        Product? GetById(int id);
        IEnumerable<Product> Search(string? category, decimal? minPrice, decimal? maxPrice);
        void Add(Product product);
        bool UpdatePrice(int id, decimal discountPercent);
    }
}
```
##### Creating Product Service Implementation
Create a class file named ProductService.cs within the Services folder, and then copy and paste the following code.

````C#
using ModelBindingDemo.Models;

namespace ModelBindingDemo.Services
{
    public class ProductService : IProductService
    {
        private readonly List<Product> _products = new()
        {
            new Product { Id = 1, Name = "Laptop", Price = 65000, Category = "Electronics", IsActive = true, Stock = 20, Rating = 4.5, CreatedDate = DateTime.Now.AddDays(-10) },
            new Product { Id = 2, Name = "Headphones", Price = 2500, Category = "Audio", IsActive = true, Stock = 50, Rating = 4.2, CreatedDate = DateTime.Now.AddDays(-5) },
            new Product { Id = 3, Name = "Smartwatch", Price = 12000, Category = "Wearables", IsActive = false, Stock = 10, Rating = 3.9, CreatedDate = DateTime.Now.AddDays(-15) },
            new Product { Id = 4, Name = "Keyboard", Price = 1500, Category = "Accessories", IsActive = true, Stock = 35, Rating = 4.1, CreatedDate = DateTime.Now.AddDays(-2) }
        };

        public IEnumerable<Product> GetAll()
        {
            return _products;
        }

        public Product? GetById(int id)
        {
            return _products.FirstOrDefault(p => p.Id == id);
        }

        public IEnumerable<Product> Search(string? category, decimal? minPrice, decimal? maxPrice)
        {
            var query = _products.AsQueryable();
            if (!string.IsNullOrWhiteSpace(category))
                query = query.Where(p => p.Category!.Equals(category, StringComparison.OrdinalIgnoreCase));
            if (minPrice.HasValue)
                query = query.Where(p => p.Price >= minPrice.Value);
            if (maxPrice.HasValue)
                query = query.Where(p => p.Price <= maxPrice.Value);

            return query.ToList();
        }

        public void Add(Product product)
        {
            product.Id = _products.Max(p => p.Id) + 1;
            product.CreatedDate = DateTime.Now;
            _products.Add(product);
        }

        public bool UpdatePrice(int id, decimal discountPercent)
        {
            var product = _products.FirstOrDefault(p => p.Id == id);
            if (product == null) return false;

            var discountAmount = product.Price * discountPercent / 100;
            product.Price -= discountAmount;
            return true;
        }
    }
}
````
**Register Service in Program.cs:**
Please add the following statement inside the Program class file.

```C#
builder.Services.AddSingleton<IProductService, ProductService>();
```

##### Create the Controller – Manual Data Extraction
Create an API Empty Controller named **ProductsController** within the Controllers folder.

````C#
using Microsoft.AspNetCore.Mvc;
using ModelBindingDemo.Models;
using ModelBindingDemo.Services;
using System.Text.Json;

namespace ModelBindingDemo.Controllers
{
    [Route("api/[controller]")]
    public class ProductsController : ControllerBase
    {
        private readonly ILogger<ProductsController> _logger;
        private readonly IProductService _productService; 

        // Constructor injection for logger and service
        public ProductsController(IProductService productService, ILogger<ProductsController> logger)
        {
            _productService = productService;
            _logger = logger;
        }

        // --------------------------------------------------------------
        // Read from Route Data
        // --------------------------------------------------------------
        // GET /api/products/details/2
        [HttpGet("details/{id}")]
        public IActionResult GetProductById()
        {
            // Step 1: Read "id" from route values
            var idValue = HttpContext.Request.RouteValues["id"]?.ToString();

            // Step 2: Validate and parse it into an integer
            if (!int.TryParse(idValue, out int productId))
                return BadRequest("Invalid Product ID format.");

            // Step 3: Use injected service to fetch product
            var product = _productService.GetById(productId);

            // Step 4: Check existence
            if (product == null)
                return NotFound($"Product with ID {productId} not found.");

            _logger.LogInformation($"Product {productId} fetched at {DateTime.UtcNow}");
            return Ok(product);
        }

        // --------------------------------------------------------------
        // Read from Query String
        // --------------------------------------------------------------
        // GET /api/products/search?category=Electronics&minPrice=2000&maxPrice=70000
        [HttpGet("search")]
        public IActionResult SearchProducts()
        {
            // Step 1: Extract query parameters
            string? category = HttpContext.Request.Query["category"];
            decimal.TryParse(HttpContext.Request.Query["minPrice"], out decimal minPrice);
            decimal.TryParse(HttpContext.Request.Query["maxPrice"], out decimal maxPrice);

            // Step 2: Call service with filters
            var result = _productService.Search(
                category,
                minPrice > 0 ? minPrice : null,
                maxPrice > 0 ? maxPrice : null
            );

            // Step 3: Log the query and return result
            _logger.LogInformation($"Search executed for Category={category}, MinPrice={minPrice}, MaxPrice={maxPrice}");
            return Ok(result);
        }

        // --------------------------------------------------------------
        // Read from Header
        // --------------------------------------------------------------
        // GET /api/products/all
        // Header: X-Api-Key: secret123
        [HttpGet("all")]
        public IActionResult GetAllProducts()
        {
            // Step 1: Extract custom header
            var apiKey = HttpContext.Request.Headers["X-Api-Key"].ToString();

            // Step 2: Validate header presence
            if (string.IsNullOrWhiteSpace(apiKey))
                return Unauthorized("Missing API key.");

            // Step 3: Check header value for access control
            if (apiKey != "secret123")
                return Unauthorized("Invalid API key.");

            // Step 4: Get all active products from service
            var products = _productService.GetAll().Where(p => p.IsActive);

            _logger.LogInformation("Active products fetched using API key authentication.");
            return Ok(products);
        }

        // --------------------------------------------------------------
        // Read from Request Body (Manual JSON Parsing)
        // --------------------------------------------------------------
        // POST /api/products/add
        // Body: { "name":"Tablet","price":25000,"category":"Electronics","isActive":true,"stock":15,"rating":4.6 }
        [HttpPost("add")]
        public async Task<IActionResult> AddProduct()
        {
            // Step 1: Read raw JSON body from request
            string body;
            using (var reader = new StreamReader(HttpContext.Request.Body))
                body = await reader.ReadToEndAsync();

            // Step 2: Validate empty request
            if (string.IsNullOrWhiteSpace(body))
                return BadRequest("Empty request body.");

            // Step 3: Try to deserialize JSON into Product object
            Product? product;
            try
            {
                product = JsonSerializer.Deserialize<Product>(body, new JsonSerializerOptions
                {
                    PropertyNameCaseInsensitive = true // Allows flexible casing
                });
            }
            catch
            {
                return BadRequest("Invalid JSON format.");
            }

            // Step 4: Manual validation
            if (product == null || string.IsNullOrWhiteSpace(product.Name) || product.Price <= 0)
                return BadRequest("Invalid product data. Name and Price are required.");

            // Step 5: Add to in-memory collection using service
            _productService.Add(product);

            _logger.LogInformation($"New product '{product.Name}' added successfully at {DateTime.UtcNow}.");
            return Ok(product);
        }

        // --------------------------------------------------------------
        // Mix of Multiple Parameters (Route + Query + Header)
        // --------------------------------------------------------------
        // PUT /api/products/discount/2?discountPercent=10
        // Header: X-Api-Key: secret123
        [HttpPut("discount/{id}")]
        public IActionResult ApplyDiscount()
        {
            // Step 1: Read from Route
            var routeId = HttpContext.Request.RouteValues["id"]?.ToString();
            if (!int.TryParse(routeId, out int productId))
                return BadRequest("Invalid product ID.");

            // Step 2: Read from Query String
            var discountQuery = HttpContext.Request.Query["discountPercent"].ToString();
            if (!decimal.TryParse(discountQuery, out decimal discountPercent) || discountPercent <= 0)
                return BadRequest("Invalid discount percent.");

            // Step 3: Read from Header (simulate authentication)
            var apiKey = HttpContext.Request.Headers["X-Api-Key"].ToString();
            if (string.IsNullOrWhiteSpace(apiKey))
                return Unauthorized("Missing API key.");
            if (apiKey != "secret123")
                return Unauthorized("Invalid API key.");

            // Step 4: Manually fetch service (for demo purposes)
            var productService = HttpContext.RequestServices.GetService<IProductService>();
            if (productService == null)
                return StatusCode(500, "Product service unavailable.");

            // Step 5: Apply discount logic
            bool updated = productService.UpdatePrice(productId, discountPercent);
            if (!updated)
                return NotFound($"No product found with ID {productId}.");

            _logger.LogInformation($"Discount of {discountPercent}% applied to Product ID {productId} at {DateTime.UtcNow}.");
            return Ok($"Product #{productId} updated successfully with {discountPercent}% discount.");
        }
    }
}
````

#### Drawbacks or Challenges without Model Binding

While the above example works, it exposes the following drawbacks or challenges of doing everything manually:

* Excessive Duplicate Code: Every action method contains repetitive code for reading route values, query strings, headers, and request bodies. For instance, `HttpContext.Request.RouteValues`, `HttpContext.Request.Query`, and `HttpContext.Request.Body` appears across multiple methods.
* Manual Type Conversion and Parsing Errors: Since all incoming data is read as strings, we must manually convert them into appropriate .NET types (int.TryParse, decimal.TryParse, etc.). Each conversion introduces potential for runtime errors and edge cases (e.g., invalid numbers or missing query parameters)
* Lack of Centralized Validation: For every request, the developer must manually check for nulls, empty values, invalid formats, and business constraints using multiple if statements.
* Error Handling is Tedious and Inconsistent: In the manual approach, every possible failure (bad route value, missing header, malformed JSON, invalid discount, etc.) must be caught and handled individually with explicit BadRequest(), Unauthorized(), or NotFound() responses. This creates inconsistencies; one endpoint may return a plain string message, while another may return a differently formatted JSON error.
* No Automatic Validation or 400 Response: Without Model Binding and [ApiController], the framework doesn’t automatically validate incoming models or return standardized 400 responses for invalid inputs.

### Using Model Binding & `[ApiController]` in ASP.NET Core Web API

In ASP.NET Core Web API, Model Binding automatically maps incoming HTTP request data (from route, query string, headers, form, or body) to method parameters and model objects. When combined with the [ApiController] attribute, the framework:

* Automatically Validates Incoming Models and populates ModelState.
* Returns 400 Bad Request responses automatically if validation fails.
* Provides Cleaner and More Maintainable controller code.
* Reduces duplicate code by eliminating repetitive parsing, type conversion, and validation code.

#### Updated Product Entity – With Data Annotations
Please modify the Product Entity as follows.

```C#
using System.ComponentModel.DataAnnotations;
namespace ModelBindingDemo.Models
{
    public class Product
    {
        public int Id { get; set; }

        [Required(ErrorMessage = "Product name is required.")]
        [StringLength(100, MinimumLength = 3, ErrorMessage = "Product name must be between 3 and 100 characters.")]
        public string Name { get; set; } = null!;

        [Required(ErrorMessage = "Price is required.")]
        [Range(1, 100000, ErrorMessage = "Price must be between 1 and 100000.")]
        public decimal Price { get; set; }

        [Required(ErrorMessage = "Category is required.")]
        [StringLength(50, ErrorMessage = "Category name cannot exceed 50 characters.")]
        public string Category { get; set; } = null!;

        public bool IsActive { get; set; } = true;

        [Range(0, 10000, ErrorMessage = "Stock must be between 0 and 10000.")]
        public int Stock { get; set; }

        [Range(0.0, 5.0, ErrorMessage = "Rating must be between 0.0 and 5.0.")]
        public double Rating { get; set; }
        public DateTime CreatedDate { get; set; } = DateTime.UtcNow;
    }
}
```

#### ProductsController — Using Model Binding & `[ApiController]`
Please modify the ProductsController as follows.

```C#
using Microsoft.AspNetCore.Mvc;
using ModelBindingDemo.Models;
using ModelBindingDemo.Services;

namespace ModelBindingDemo.Controllers
{
    // Enables automatic model binding, validation, and 400 error responses
    [ApiController]
    [Route("api/[controller]")]
    public class ProductsController : ControllerBase
    {
        private readonly ILogger<ProductsController> _logger;
        private readonly IProductService _productService;

        public ProductsController(IProductService productService, ILogger<ProductsController> logger)
        {
            _productService = productService;
            _logger = logger;
        }

        // --------------------------------------------------------------
        // Route Data Example
        // --------------------------------------------------------------
        // GET /api/products/details/2
        [HttpGet("details/{id}")]
        public IActionResult GetProductById([FromRoute] int id)
        {
            // Model Binding automatically maps {id} → int id
            var product = _productService.GetById(id);

            if (product == null)
                return NotFound($"Product with ID {id} not found.");

            _logger.LogInformation($"Product {id} fetched successfully at {DateTime.UtcNow}.");
            return Ok(product);
        }

        // --------------------------------------------------------------
        // Query String Example
        // --------------------------------------------------------------
        // GET /api/products/search?category=Electronics&minPrice=2000&maxPrice=70000
        [HttpGet("search")]
        public IActionResult SearchProducts(
            [FromQuery] string? category,
            [FromQuery] decimal? minPrice,
            [FromQuery] decimal? maxPrice)
        {
            // Model Binding automatically maps query string parameters
            var result = _productService.Search(category, minPrice, maxPrice);

            _logger.LogInformation($"Search executed for Category={category}, MinPrice={minPrice}, MaxPrice={maxPrice}.");
            return Ok(result);
        }

        // --------------------------------------------------------------
        // Header Example
        // --------------------------------------------------------------
        // GET /api/products/all
        // Header: X-Api-Key: secret123
        [HttpGet("all")]
        public IActionResult GetAllProducts([FromHeader(Name = "X-Api-Key")] string apiKey)
        {
            // [FromHeader] automatically binds header values
            if (string.IsNullOrWhiteSpace(apiKey))
                return Unauthorized("Missing API key.");

            if (apiKey != "secret123")
                return Unauthorized("Invalid API key.");

            var products = _productService.GetAll().Where(p => p.IsActive);

            _logger.LogInformation("Active products fetched successfully using API key authentication.");
            return Ok(products);
        }

        // --------------------------------------------------------------
        // Request Body Example (with Data Annotations)
        // --------------------------------------------------------------
        // POST /api/products/add
        // Body: { "name":"Tablet","price":25000,"category":"Electronics","isActive":true,"stock":15,"rating":4.6 }
        [HttpPost("add")]
        public IActionResult AddProduct([FromBody] Product product)
        {
            // No need to check ModelState.IsValid — [ApiController] does it automatically
            // If validation fails, ASP.NET Core returns 400 Bad Request with detailed errors

            _productService.Add(product);

            _logger.LogInformation($"New product '{product.Name}' added successfully at {DateTime.UtcNow}.");

            // Returns 201 Created with location header
            return CreatedAtAction(nameof(GetProductById), new { id = product.Id }, product);
        }

        // --------------------------------------------------------------
        // Mix of Multiple Parameters (Route + Query + Header + Service)
        // --------------------------------------------------------------
        // PUT /api/products/discount/2?discountPercent=10
        // Header: X-Api-Key: secret123
        [HttpPut("discount/{id}")]
        public IActionResult ApplyDiscount(
            [FromRoute] int id,
            [FromQuery] decimal discountPercent,
            [FromHeader(Name = "X-Api-Key")] string apiKey,
            [FromServices] IProductService productService)
        {
            if (string.IsNullOrWhiteSpace(apiKey))
                return Unauthorized("Missing API key.");

            if (apiKey != "secret123")
                return Unauthorized("Invalid API key.");

            if (discountPercent <= 0)
                return BadRequest("Discount percent must be greater than zero.");

            // Using service from [FromServices] instead of controller field
            bool updated = productService.UpdatePrice(id, discountPercent);

            if (!updated)
                return NotFound($"No product found with ID {id}.");

            _logger.LogInformation($"Discount of {discountPercent}% applied to Product ID {id} at {DateTime.UtcNow}.");
            return Ok($"Product #{id} updated successfully with {discountPercent}% discount.");
        }
    }
}
```

#### Advantages of using Model Binding in ASP.NET Core Web API

* Clean, Minimal Controller Code: No more HttpContext.Request.RouteValues, Request.Query, or StreamReader. Model Binding automatically matches data from the request to action parameters.
* Automatic Type Conversion: String values from the request are automatically converted to their proper types (int, decimal, bool, etc.), no need for TryParse
* Built-In Validation:   With [ApiController], ASP.NET Core automatically:
  * Checks ModelState after binding.
  * Returns 400 Bad Request with detailed validation errors if the model is invalid.
  * You can add **[Required]**, **[Range]**, or **[StringLength]** attributes on your model for even stronger validation.
* Consistent Error Responses: Validation errors are automatically returned in a standardized JSON structure using the ProblemDetails format, making the API responses consistent and client-friendly.
* Declarative Data Source Mapping: Attributes like `[FromRoute]`, `[FromQuery]`, `[FromHeader]`, and `[FromBody]` clearly indicate where each value is expected to come from, improving readability and maintainability.
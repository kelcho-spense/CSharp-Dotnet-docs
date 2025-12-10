# Services in ASP.NET Core Web API

In ASP.NET Core Web API, controllers are responsible for handling HTTP requests and returning responses. While it is technically possible to write all business logic and data access logic inside controllers, this approach quickly becomes messy and complicated to maintain as the project grows.

To solve this, **Services** are introduced. Services act as dedicated classes that contain the application’s **business logic and data manipulation code**, keeping controllers clean and focused only on coordinating HTTP requests and responses. This separation of concerns improves readability, maintainability, testability, and scalability of the application.

## Problems with Writing Logic Inside Controllers

If we directly embed business logic and data access operations inside controllers, the following problems arise:

* **Code Duplication**: Reusing logic across multiple controllers results in repeated code.
* **Reduced Testability**: Testing controllers becomes more challenging because business rules are intertwined with request-handling logic.
* **Tightly Coupled Code**: Changes in business rules force changes in multiple controllers, reducing flexibility.
* **Violation of the Single Responsibility Principle (SRP)**: Controllers handle both HTTP request/response management and business rules, making them overloaded.

### How Services Solve These Problems

By moving business logic into dedicated service classes, controllers can remain lightweight and focused only on HTTP concerns.

* Encapsulation of Logic: Business rules and data access logic live inside services.
* Reusability: Services can be shared across multiple controllers.
* Testability: Services can be unit-tested independently without worrying about HTTP request handling.
* Maintainability: Easier to manage, extend, or replace logic without affecting controllers.

### Where Should We Write Validation Logic?

Validation in ASP.NET Core Web API can be handled in two ways:

* At the Controller Level: ASP.NET Core automatically validates model state if [ApiController] is used. Invalid requests return 400 Bad Request automatically.
* At the Service Level: Business-specific validations (e.g., “price cannot exceed 10,000” or “product name must be unique”) should be placed in services.

> **Best Practice**:
> * Use **Data Annotations in Models/DTOs for general input validation** (length, required fields, ranges).
> * **Use Services for business logic validations** (unique constraints, cross-entity rules).

Creating a Product Service

First, create a folder named ‘**Services**’ at the project root directory, where we will create all service interfaces and their corresponding concrete implementations.

![image_267.png](image_267.png)

#### Define an Interface (Contracts)
Create an interface named `IProductService.cs` within the **Services** folder, and copy-paste the following code.

![image_268.png](image_268.png)

```c#
using small_ecommerce_api.DTOs;

namespace small_ecommerce_api.Services
{
    public interface IProductService
    {
        IEnumerable<ProductDTO> GetAllProducts();
        ProductDTO? GetProductById(int id);
        ProductDTO CreateProduct(ProductCreateDTO createDto);
        bool UpdateProduct(int id, ProductUpdateDTO updateDto);
        bool DeleteProduct(int id);
    }
}
```

**Code Explanations:**
* Interfaces define a **Contract**. They specify **what methods exist** without defining **how they work**.
* This ensures **Loose Coupling**. The controller doesn’t care about the actual implementation, only that the service provides these methods.
* It also enables **testability** (we can mock this interface in unit tests).

#### Implement the Service

Create a class file named `ProductService.cs` within the Services folder. The following class implements the `IProductService.cs` interface and provides implementations for all interface methods.

```C#
using small_ecommerce_api.DTOs;
using small_ecommerce_api.Models;

namespace small_ecommerce_api.Services
{
    public class ProductService : IProductService
    {
        private static List<Category> _categories = new()
        {
            new Category { Id = 1, Name = "Electronics" },
            new Category { Id = 2, Name = "Furniture" },
        };
        private static List<Product> _products = new()
        {
            new Product { Id = 1, Name = "Laptop", Price = 1000.00m, CategoryId = 1 },
            new Product { Id = 2, Name = "Desktop", Price = 2000.00m, CategoryId = 1 },
            new Product { Id = 3, Name = "Chair", Price = 150.00m, CategoryId = 2 },
        };
        public IEnumerable<ProductDTO> GetAllProducts()
        {
            return _products.Select(p => new ProductDTO
            {
                Id = p.Id,
                Name = p.Name,
                Price = p.Price,
                CategoryName = _categories.FirstOrDefault(c => c.Id == p.CategoryId)?.Name ?? "Unknown"
            });
        }
        public ProductDTO? GetProductById(int id)
        {
            var product = _products.FirstOrDefault(p => p.Id == id);
            if (product == null) return null;
            return new ProductDTO
            {
                Id = product.Id,
                Name = product.Name,
                Price = product.Price,
                CategoryName = _categories.FirstOrDefault(c => c.Id == product.CategoryId)?.Name ?? "Unknown"
            };
        }
        public ProductDTO CreateProduct(ProductCreateDTO createDto)
        {
            var newProduct = new Product
            {
                Id = _products.Max(p => p.Id) + 1,
                Name = createDto.Name,
                Price = createDto.Price,
                CategoryId = createDto.CategoryId
            };
            _products.Add(newProduct);
            return new ProductDTO
            {
                Id = newProduct.Id,
                Name = newProduct.Name,
                Price = newProduct.Price,
                CategoryName = _categories.FirstOrDefault(c => c.Id == newProduct.CategoryId)?.Name ?? "Unknown"
            };
        }
        public bool UpdateProduct(int id, ProductUpdateDTO updateDto)
        {
            var product = _products.FirstOrDefault(p => p.Id == id);
            if (product == null) return false;
            product.Name = updateDto.Name;
            product.Price = updateDto.Price;
            product.CategoryId = updateDto.CategoryId;
            return true;
        }
        public bool DeleteProduct(int id)
        {
            var product = _products.FirstOrDefault(p => p.Id == id);
            if (product == null) return false;
            _products.Remove(product);
            return true;
        }
    }
}

```

**Code Explanations:**
* This class provides the **Actual Implementation** of the interface.
* It encapsulates the **Business Logic** (e.g., creating, updating, and deleting products).
* It isolates **Data Handling** (right now, hardcoded lists; later, database logic).
* If the logic changes (say you move from in-memory list → EF Core database), only the service changes, not the controller.

#### Register the Service in the `Program.cs`

```C#
using small_ecommerce_api.Services;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddControllers();

// Learn more about configuring OpenAPI at https://aka.ms/aspnet/openapi
builder.Services.AddOpenApi();

// register product service
// AddScoped means a New Instance of ProductService will be created for each HTTP request.
builder.Services.AddScoped<IProductService, ProductService>();

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

**Code Explanations:**
* ASP.NET Core uses **Dependency Injection (DI)** by default.
* Here, we register IProductService with its implementation, ProductService.
* **AddScoped means a New Instance of ProductService will be created for each HTTP request**.
* This ensures services are **injected** automatically into controllers wherever required.

> Real-time Analogy: This is like telling the Restaurant Manager: “Whenever a customer orders something from the menu (IProductService), send it to this specific kitchen (ProductService).”

#### Use the Service in the Controller

Finally, update the Products Controller as follows.

```C#
using Microsoft.AspNetCore.Mvc;
using small_ecommerce_api.DTOs;
using small_ecommerce_api.Models;
using small_ecommerce_api.Services;

namespace small_ecommerce_api.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class ProductsController : Controller
    {
        private readonly IProductService _productService;

        public ProductsController(IProductService productService)
        {
            _productService = productService;
        }

        // GET: api/products
        [HttpGet]
        public ActionResult<IEnumerable<ProductDTO>> GetProducts()
        {
            var Products = _productService.GetAllProducts();

            if (Products == null) {
                return NotFound(new { Message = "No products found." });
            }

            return Ok(Products);
        }

        // GET: api/products/{id}
        [HttpGet("{id}")]
        public ActionResult<ProductDTO> GetProduct(int id)
        {
            var product = _productService.GetProductById(id);
            if (product == null)
            {
                return NotFound(new { Message = $"Product with ID {id} not found." });
            }
         
            return Ok(product);
        }

        // POST: api/products
        [HttpPost]
        public ActionResult<ProductDTO> PostProduct([FromBody] ProductCreateDTO createDto)
        {
            var newProduct = _productService.CreateProduct(createDto);
          
            return CreatedAtAction(nameof(GetProduct), new { id = newProduct.Id }, newProduct);
        }

        // PUT: api/products/{id}
        [HttpPut("{id}")]
        public IActionResult UpdateProduct(int id, [FromBody] ProductUpdateDTO updateDto)
        {
            if (id != updateDto.Id)
            {
                return BadRequest(new { Message = "ID mismatch between route and body." });
            }

            var existingProduct = _productService.GetProductById(id);

            if (existingProduct == null)
            {
                return NotFound(new { Message = $"Product with ID {id} not found." });
            }

            return NoContent();
        }

        // DELETE: api/products/{id}
        [HttpDelete("{id}")]
        public IActionResult DeleteProduct(int id)
        {
            var product = _productService.GetProductById(id);

            if (product == null)
            {
                return NotFound(new { Message = $"Product with ID {id} not found." });
            }

            return NoContent();
        }


    }
}

```

**Code Explanations:**
* The controller **depends** on IProductService, not directly on ProductService.
* ASP.NET Core’s DI system automatically provides an instance of ProductService when the controller is created.
* This keeps the controller **clean**:
  * No product management logic inside.
  * Only coordinates between HTTP requests and the service methods.

> Real-time Analogy: The `waiter` (**controller**) doesn’t cook the food. He takes the order and passes it to the `chef` (**service**), then brings the result back to the customer.
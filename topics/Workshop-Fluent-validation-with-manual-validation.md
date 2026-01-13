# [Workshop] Fluent validation with manual validation

Let’s Understand How to Implement Fluent API Validation in ASP.NET Core Web API. We will create a simple application to validate a Product model using the EF Core Database First approach.

## Create a New ASP.NET Core Web API Project and Install Required NuGet Packages
First, create a new ASP.NET Core Web API Project named FluentAPIValidation and install the following Packages required for Fluent API validation and Entity Framework core. You can install the packages using the Package Manager Console by executing the following commands:

```Bash
Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore.Tools
Install-Package FluentValidation
Install-Package Microsoft.EntityFrameworkCore.Design
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

### Create the Product Model
First, create a folder named Models in the project root directory where we will create our Models.

#### Product Model
Create a class file named `Product.cs`

```C#
using System.ComponentModel.DataAnnotations.Schema;

namespace Fluent_API_Validation.Model
{
    public class Product
    {
        public int ProductId { get; set; }

        // Stock Keeping Unit following a specific pattern (e.g., 8 uppercase letters/digits)
        public string SKU { get; set; }

        public string Name { get; set; }

        [Column(TypeName = "decimal(8,2)")]
        public decimal Price { get; set; }

        public int Stock { get; set; }

        public int CategoryId { get; set; }

        public string? Description { get; set; }

        // Discount percentage (0-100)
        [Column(TypeName = "decimal(18,2)")]
        public decimal Discount { get; set; }

        // Manufacturing date (should not be in the future)
        public DateTime ManufacturingDate { get; set; }

        // Expiry date (must be after the manufacturing date)
        public DateTime ExpiryDate { get; set; }

        // Many-to-many relationship with Tag
        public ICollection<Tag> Tags { get; set; } = new List<Tag>();
    }
}

```
#### Tag Model
Each tag is stored as a separate entity. Products and Tags are connected via many‑to‑many relationships. So, create a class file named Tag.cs within the Models folder and copy and paste the following code.

```C#
namespace Fluent_API_Validation.Model
{
    public class Tag
    {
        public int TagId { get; set; }
        public string Name { get; set; }
        // Many-to-many relationship with Product
        public ICollection<Product> Products { get; set; } = new List<Product>();
    }
}

```

#### DbContext Class:
First, create a folder named Data in the project root directory. Then, add a class file named `ECommerceDbContext.cs`

```C#
using Fluent_API_Validation.Model;
using Microsoft.EntityFrameworkCore;

namespace Fluent_API_Validation.Data
{
    public class ECommerceDbContext : DbContext
    {
        public ECommerceDbContext(DbContextOptions<ECommerceDbContext> options) : base(options) { }
        public DbSet<Product> Products { get; set; }
        public DbSet<Tag> Tags { get; set; }
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // Configure many-to-many relationship between Product and Tag using an implicit join table "ProductTag"
            modelBuilder.Entity<Product>()
                .HasMany(p => p.Tags)
                .WithMany(t => t.Products)
                //Tells EFCore, create a table named ProductTag with TagId and ProductId as columns,
                //but don’t bother me with an explicit class.
                .UsingEntity<Dictionary<string, object>>(
                    "ProductTag",
                    pt => pt.HasOne<Tag>().WithMany().HasForeignKey("TagId"),
                    pt => pt.HasOne<Product>().WithMany().HasForeignKey("ProductId"));
            // Seed Tags (one record per line)
            modelBuilder.Entity<Tag>().HasData(
                new Tag { TagId = 1, Name = "electronics" },
                new Tag { TagId = 2, Name = "gaming" },
                new Tag { TagId = 3, Name = "office" },
                new Tag { TagId = 4, Name = "accessories" },
                new Tag { TagId = 5, Name = "home" }
            );
            // Seed Products (one record per line)
            modelBuilder.Entity<Product>().HasData(
                new Product { ProductId = 1, SKU = "GAM12345", Name = "Gaming Laptop", Price = 1500.00m, Stock = 10, CategoryId = 1, Description = "High performance gaming laptop.", Discount = 10, ManufacturingDate = new DateTime(2023, 1, 1), ExpiryDate = new DateTime(2024, 1, 1) },
                new Product { ProductId = 2, SKU = "OFF12345", Name = "Office Desktop", Price = 800.00m, Stock = 20, CategoryId = 1, Description = "Efficient desktop for office work.", Discount = 5, ManufacturingDate = new DateTime(2023, 1, 1), ExpiryDate = new DateTime(2024, 1, 1) },
                new Product { ProductId = 3, SKU = "SMA12345", Name = "Smartphone", Price = 700.00m, Stock = 50, CategoryId = 2, Description = "Latest model smartphone.", Discount = 0, ManufacturingDate = new DateTime(2023, 1, 1), ExpiryDate = new DateTime(2024, 1, 1) },
                new Product { ProductId = 4, SKU = "WIR12345", Name = "Wireless Mouse", Price = 50.00m, Stock = 100, CategoryId = 3, Description = "Ergonomic wireless mouse.", Discount = 15, ManufacturingDate = new DateTime(2023, 1, 1), ExpiryDate = new DateTime(2024, 1, 1) },
                new Product { ProductId = 5, SKU = "MEC12345", Name = "Mechanical Keyboard", Price = 120.00m, Stock = 75, CategoryId = 3, Description = "RGB mechanical keyboard.", Discount = 20, ManufacturingDate = new DateTime(2023, 1, 1), ExpiryDate = new DateTime(2024, 1, 1) },
                new Product { ProductId = 6, SKU = "4KMON12", Name = "4K Monitor", Price = 400.00m, Stock = 30, CategoryId = 4, Description = "Ultra HD 4K monitor.", Discount = 5, ManufacturingDate = new DateTime(2023, 1, 1), ExpiryDate = new DateTime(2024, 1, 1) },
                new Product { ProductId = 7, SKU = "GAMCHAIR", Name = "Gaming Chair", Price = 300.00m, Stock = 15, CategoryId = 4, Description = "Ergonomic gaming chair.", Discount = 10, ManufacturingDate = new DateTime(2023, 1, 1), ExpiryDate = new DateTime(2024, 1, 1) },
                new Product { ProductId = 8, SKU = "BLU12345", Name = "Bluetooth Speaker", Price = 150.00m, Stock = 40, CategoryId = 5, Description = "Portable Bluetooth speaker.", Discount = 0, ManufacturingDate = new DateTime(2023, 1, 1), ExpiryDate = new DateTime(2024, 1, 1) },
                new Product { ProductId = 9, SKU = "SMAW1234", Name = "Smartwatch", Price = 250.00m, Stock = 25, CategoryId = 2, Description = "Feature-packed smartwatch.", Discount = 5, ManufacturingDate = new DateTime(2023, 1, 1), ExpiryDate = new DateTime(2024, 1, 1) },
                new Product { ProductId = 10, SKU = "HOMECAM1", Name = "Home Security Camera", Price = 100.00m, Stock = 60, CategoryId = 5, Description = "HD home security camera.", Discount = 10, ManufacturingDate = new DateTime(2023, 1, 1), ExpiryDate = new DateTime(2024, 1, 1) }
            );
            // Seed join table for ProductTag (each record provided in the same line)
            modelBuilder.Entity("ProductTag").HasData(
                new { ProductId = 1, TagId = 1 },
                new { ProductId = 1, TagId = 2 },
                new { ProductId = 2, TagId = 1 },
                new { ProductId = 2, TagId = 3 },
                new { ProductId = 3, TagId = 1 },
                new { ProductId = 4, TagId = 4 },
                new { ProductId = 5, TagId = 4 },
                new { ProductId = 5, TagId = 3 },
                new { ProductId = 6, TagId = 1 },
                new { ProductId = 6, TagId = 3 },
                new { ProductId = 7, TagId = 2 },
                new { ProductId = 7, TagId = 3 },
                new { ProductId = 8, TagId = 1 },
                new { ProductId = 8, TagId = 4 },
                new { ProductId = 9, TagId = 1 },
                new { ProductId = 9, TagId = 4 },
                new { ProductId = 10, TagId = 1 },
                new { ProductId = 10, TagId = 5 }
            );
        }
    }

}
```

#### Why Use Dictionary<string, object> while Configuring the Joining table?
We use Dictionary<string, object> here to define a shadow entity for the join table without creating an explicit CLR class for it. By specifying Dictionary<string, object>, we indicate that the join table (named “ProductTag”) doesn’t have its own dedicated entity class. It’s just a table with the foreign keys (ProductId and TagId).

* **No Explicit Entity Needed**: It allows us to configure the join table without writing a separate ProductTag class.
* **Shadow Properties**: The keys (e.g., “ProductId”, “TagId”) become shadow properties that EF manages internally.

#### Creating DTOs
Create a folder named DTOs in the Project root directory. A DTO (Data Transfer Object) transfers data between the client and server. It often contains a subset of the entity’s properties or additional properties for validation that should not be persisted directly. In our example, we will define:

* **ProductCreateDTO** for creation,
* **ProductUpdateDTO** for updating, and
* **ProductResponseDTO** for sending data back to clients.
* **ProductBaseDTO** for common properties and methods

#### ProductBaseDTO

```C#
namespace Fluent_API_Validation.DTOs
{
    public class ProductBaseDTO
    {
        public string SKU { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
        public int Stock { get; set; }
        public int CategoryId { get; set; }
        public string? Description { get; set; }
        public decimal Discount { get; set; }
        public DateTime ManufacturingDate { get; set; }
        public DateTime ExpiryDate { get; set; }
        public List<string>? Tags { get; set; }
    }
}
```
#### ProductCreateDTO

```C#
namespace Fluent_API_Validation.DTOs
{
    // ProductCreateDTO inherits all common properties from ProductBaseDTO
    public class ProductCreateDTO : ProductBaseDTO
    {
    }
}
```
#### ProductUpdateDTO

```C#
namespace Fluent_API_Validation.DTOs
{
    // ProductUpdateDTO inherits all common properties from ProductBaseDTO
    // and adds the ProductId for identifying the product to update
    public class ProductUpdateDTO : ProductBaseDTO
    {
        public int ProductId { get; set; }
    }
}
```

#### ProductResponseDTO

```C#
namespace Fluent_API_Validation.DTOs
{
    public class ProductResponseDTO
    {
        public int ProductId { get; set; }
        public string SKU { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
        public int Stock { get; set; }
        public int CategoryId { get; set; }
        public string? Description { get; set; }
        public decimal Discount { get; set; }
        public DateTime ManufacturingDate { get; set; }
        public DateTime ExpiryDate { get; set; }
        public List<string>? Tags { get; set; }
    }
}
```

### Add Fluent Validation for the Product Model:
First, create a new folder named `Validators` in the project root directory. This folder contains classes inherited from `AbstractValidator<T>` to define validation rules separate from the model classes.
#### ProductBaseDTOValidator

```C#
using Fluent_API_Validation.Data;
using Fluent_API_Validation.DTOs;
using FluentValidation;
using Microsoft.EntityFrameworkCore;

namespace Fluent_API_Validation.Validators
{
    /// <summary>
    /// Base validator for ProductBaseDTO containing all common validation rules.
    /// Supports async validation for database checks.
    /// </summary>
    public class ProductBaseDTOValidator<T> : AbstractValidator<T> where T : ProductBaseDTO
    {
        protected readonly ECommerceDbContext _context;

        public ProductBaseDTOValidator(ECommerceDbContext context)
        {
            _context = context;

            // SKU validation: must not be empty, match regex, and be unique
            RuleFor(p => p.SKU)
                .NotEmpty().WithMessage("SKU is required.")
                .Matches("^[A-Z0-9]{8}$").WithMessage("SKU must be 8 characters long and contain only uppercase letters and digits.")
                .MustAsync(BeUniqueSKUAsync).WithMessage("SKU must be unique.");

            // Name validation: must not be empty, proper length, and be unique
            RuleFor(p => p.Name)
                .NotEmpty().WithMessage("Product name is required.")
                .Length(3, 50).WithMessage("Product name must be between 3 and 50 characters.")
                .MustAsync(BeUniqueNameAsync).WithMessage("Product name must be unique.");

            // Price validation: must be greater than 0 and conform to precision and scale
            RuleFor(p => p.Price)
                .GreaterThan(0).WithMessage("Price must be greater than 0.")
                .PrecisionScale(8, 2, true).WithMessage("Price must have at most 8 digits in total and 2 decimals.");

            // Stock validation: must be zero or positive
            RuleFor(p => p.Stock)
                .GreaterThanOrEqualTo(0).WithMessage("Stock cannot be negative.");

            // CategoryId validation: must be a positive number
            RuleFor(p => p.CategoryId)
                .GreaterThan(0).WithMessage("Category ID is required.");

            // Description validation: if provided, must not exceed 500 characters
            RuleFor(p => p.Description)
                .MaximumLength(500).WithMessage("Description cannot exceed 500 characters.")
                .When(p => !string.IsNullOrEmpty(p.Description));

            // Discount validation: must be between 0 and 100
            RuleFor(p => p.Discount)
                .InclusiveBetween(0, 100).WithMessage("Discount must be between 0 and 100.");

            // Manufacturing date validation: must not be a future date
            RuleFor(p => p.ManufacturingDate)
                .LessThanOrEqualTo(DateTime.Now).WithMessage("Manufacturing date cannot be in the future.");

            // Expiry date validation: must be later than the manufacturing date
            RuleFor(p => p.ExpiryDate)
                .GreaterThan(p => p.ManufacturingDate).WithMessage("Expiry date must be after the manufacturing date.");

            // Tags validation: each tag must not be empty and not exceed 20 characters
            RuleForEach(p => p.Tags).ChildRules(tag =>
            {
                tag.RuleFor(t => t)
                    .NotEmpty().WithMessage("Tag cannot be empty.")
                    .MaximumLength(20).WithMessage("Tag cannot exceed 20 characters.");
            });
        }

        // Checks asynchronously that the SKU is unique in the database.
        // Override in derived classes to exclude current product during updates.
        protected virtual async Task<bool> BeUniqueSKUAsync(T dto, string sku, CancellationToken cancellationToken)
        {
            return !await _context.Products
                .AsNoTracking()
                .AnyAsync(p => p.SKU == sku, cancellationToken);
        }

        // Checks asynchronously that the product name is unique in the database.
        // Override in derived classes to exclude current product during updates.
        protected virtual async Task<bool> BeUniqueNameAsync(T dto, string name, CancellationToken cancellationToken)
        {
            return !await _context.Products
                .AsNoTracking()
                .AnyAsync(p => p.Name == name, cancellationToken);
        }
    }
}

```

#### ProductCreateDTOValidator
The following class is inherited from the `AbstractValidator<T>` class where T is the model being validated and, in our example, it is the `ProductCreateDTO`.

```C#
using Fluent_API_Validation.Data;
using Fluent_API_Validation.DTOs;
using FluentValidation;

namespace Fluent_API_Validation.Validators
{
    // Validator for ProductCreateDTO, inherits all common rules from ProductBaseDTOValidator.
    public class ProductCreateDTOValidator : ProductBaseDTOValidator<ProductCreateDTO>
    {
        public ProductCreateDTOValidator(ECommerceDbContext context) : base(context)
        {
        }
    }
}

```

#### ProductUpdateDTOValidator

````C#
using Fluent_API_Validation.Data;
using Fluent_API_Validation.DTOs;
using FluentValidation;
using Microsoft.EntityFrameworkCore;

namespace Fluent_API_Validation.Validators
{
    
    // Validator for ProductUpdateDTO, inherits all common rules from ProductBaseDTOValidator.
    // Includes async validation to check if ProductId exists in database.
    public class ProductUpdateDTOValidator : ProductBaseDTOValidator<ProductUpdateDTO>
    {
        public ProductUpdateDTOValidator(ECommerceDbContext context) : base(context)
        {
            // ProductId validation: must be greater than 0 and exist in database
            RuleFor(p => p.ProductId)
                .GreaterThan(0).WithMessage("ProductId must be greater than 0.")
                .MustAsync(ProductExistsAsync).WithMessage("Product does not exist.");
        }

        
        // Checks asynchronously that the ProductId exists in the database.       
        private async Task<bool> ProductExistsAsync(int productId, CancellationToken cancellationToken)
        {
            return await _context.Products
                .AsNoTracking()
                .AnyAsync(p => p.ProductId == productId, cancellationToken);
        }

      
        // Override to exclude the current product when checking SKU uniqueness during updates.
        protected override async Task<bool> BeUniqueSKUAsync(ProductUpdateDTO dto, string sku, CancellationToken cancellationToken)
        {
            return !await _context.Products
                .AsNoTracking()
                .AnyAsync(p => p.SKU == sku && p.ProductId != dto.ProductId, cancellationToken);
        }

    
        // Override to exclude the current product when checking Name uniqueness during updates.
        protected override async Task<bool> BeUniqueNameAsync(ProductUpdateDTO dto, string name, CancellationToken cancellationToken)
        {
            return !await _context.Products
                .AsNoTracking()
                .AnyAsync(p => p.Name == name && p.ProductId != dto.ProductId, cancellationToken);
        }
    }
}
````

### Create the Products Controller
The ProductsController handles HTTP requests related to product operations (such as creating, retrieving, and updating products). It is the intermediary between client requests and the underlying business logic and data access layer. The controller demonstrates:

* **Create** and **Update** endpoints that use DTOs and FluentValidation.
* A **GET endpoint** that filters products based on a comma‑separated list of tags.
* Proper mapping between DTOs and domain models with tag processing.

```C#
using Fluent_API_Validation.Data;
using Fluent_API_Validation.DTOs;
using Fluent_API_Validation.Model;
using FluentValidation;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;

namespace Fluent_API_Validation.Controllers
{
    // The ProductsController handles all product-related API endpoints.
    [Route("api/[controller]")]
    [ApiController]
    public class ProductsController : ControllerBase
    {
        // The database context used to interact with the underlying database.
        private readonly ECommerceDbContext _context;
        private readonly IValidator<ProductCreateDTO> _createValidator;
        private readonly IValidator<ProductUpdateDTO> _updateValidator;
        // Constructor injection of the database context and validators.
        public ProductsController(
            ECommerceDbContext context,
            IValidator<ProductCreateDTO> createValidator,
            IValidator<ProductUpdateDTO> updateValidator)
        {
            _context = context;
            _createValidator = createValidator;
            _updateValidator = updateValidator;
        }

        // GET: api/products?tags=tag1,tag2
        // Retrieves all products, optionally filtered by any matching tags.
        [HttpGet]
        public async Task<ActionResult<IEnumerable<ProductResponseDTO>>> GetProducts([FromQuery] string? tags)
        {
            // Build the query and include the related Tags collection for each product.
            IQueryable<Product> query = _context.Products
                            .AsNoTracking()
                            .Include(p => p.Tags);
            if (!string.IsNullOrEmpty(tags))
            {
                // Split the comma-separated tags string into a list.
                // Trim spaces and convert each tag to lower case for case-insensitive comparison.
                var tagList = tags.Split(',', StringSplitOptions.RemoveEmptyEntries)
                                  .Select(t => t.Trim().ToLower())
                                  .ToList();
                // Filter products that contain ANY of the specified tags.
                // If a product has at least one matching tag, it is included in the result.
                query = query.Where(p => p.Tags.Any(t => tagList.Contains(t.Name.ToLower())));
            }
            // Execute the query and retrieve the list of products from the database.
            var products = await query.ToListAsync();
            // Map each Product entity to a ProductResponseDTO.
            var result = products.Select(p => new ProductResponseDTO
            {
                ProductId = p.ProductId,
                SKU = p.SKU,
                Name = p.Name,
                Price = p.Price,
                Stock = p.Stock,
                CategoryId = p.CategoryId,
                Description = p.Description,
                Discount = p.Discount,
                ManufacturingDate = p.ManufacturingDate,
                ExpiryDate = p.ExpiryDate,
                // Map the Tags collection to a list of tag names.
                Tags = p.Tags.Select(t => t.Name).ToList()
            }).ToList();
            // Return the filtered list of products with an HTTP 200 OK status.
            return Ok(result);
        }

        // GET: api/products/{id}
        // Retrieves a single product by its ID.
        [HttpGet("{id}")]
        public async Task<ActionResult<ProductResponseDTO>> GetProduct(int id)
        {
            // Retrieve the product with the given ID, including its Tags.
            // AsNoTracking() is used here since no update is needed (improves performance).
            var product = await _context.Products
                                        .AsNoTracking()
                                        .Include(p => p.Tags)
                                        .FirstOrDefaultAsync(p => p.ProductId == id);

            // If the product is not found, return a 404 Not Found response.
            if (product == null)
                return NotFound();
            // Map the Product entity to a ProductResponseDTO.
            var response = new ProductResponseDTO
            {
                ProductId = product.ProductId,
                SKU = product.SKU,
                Name = product.Name,
                Price = product.Price,
                Stock = product.Stock,
                CategoryId = product.CategoryId,
                Description = product.Description,
                Discount = product.Discount,
                ManufacturingDate = product.ManufacturingDate,
                ExpiryDate = product.ExpiryDate,
                Tags = product.Tags.Select(t => t.Name).ToList()
            };
            // Return the product details with an HTTP 200 OK status.
            return Ok(response);
        }

        // POST: api/products
        // Creates a new product based on the data provided in ProductCreateDTO.
        [HttpPost]
        public async Task<ActionResult<ProductResponseDTO>> CreateProduct([FromBody] ProductCreateDTO productDto)
        {
            // Validate asynchronously using FluentValidation
            var validationResult = await _createValidator.ValidateAsync(productDto);

            if (!validationResult.IsValid)
            {
                var errors = validationResult.Errors.Select(e => new
                {
                    Field = e.PropertyName,
                    Error = e.ErrorMessage
                });
                return BadRequest(new { Errors = errors });
            }

            // Map the incoming DTO to a new Product entity.
            var product = new Product
            {
                SKU = productDto.SKU,
                Name = productDto.Name,
                Price = productDto.Price,
                Stock = productDto.Stock,
                CategoryId = productDto.CategoryId,
                Description = productDto.Description,
                Discount = productDto.Discount,
                ManufacturingDate = productDto.ManufacturingDate,
                ExpiryDate = productDto.ExpiryDate
            };
            // Process Tags: For each tag provided in the DTO, check if it exists.
            // If the tag exists, use the existing Tag; otherwise, create a new Tag.
            if (productDto.Tags != null && productDto.Tags.Any())
            {
                foreach (var tagName in productDto.Tags)
                {
                    // Normalize the tag by trimming whitespace and converting to lower case.
                    var normalizedTagName = tagName.Trim().ToLower();
                    var existingTag = await _context.Tags.FirstOrDefaultAsync(t => t.Name.ToLower() == normalizedTagName);
                    if (existingTag != null)
                        product.Tags.Add(existingTag); // Associate the existing tag with the product.
                    else
                        product.Tags.Add(new Tag { Name = normalizedTagName }); // Create a new tag and associate it.
                }
            }
            // Add the new product to the database context.
            _context.Products.Add(product);
            // Save changes to persist the new product (and associated tags) to the database.
            await _context.SaveChangesAsync();
            // Map the newly created Product entity to a ProductResponseDTO.
            var response = new ProductResponseDTO
            {
                ProductId = product.ProductId,
                SKU = product.SKU,
                Name = product.Name,
                Price = product.Price,
                Stock = product.Stock,
                CategoryId = product.CategoryId,
                Description = product.Description,
                Discount = product.Discount,
                ManufacturingDate = product.ManufacturingDate,
                ExpiryDate = product.ExpiryDate,
                Tags = product.Tags.Select(t => t.Name).ToList()
            };
            // Return the created product with an HTTP 200 OK (or 201 Created) status.
            return Ok(response);
        }

        // PUT: api/products/{id}
        // Updates an existing product based on the data provided in ProductUpdateDTO.
        [HttpPut("{id}")]
        public async Task<ActionResult<ProductResponseDTO>> UpdateProduct(int id, [FromBody] ProductUpdateDTO productDto)
        {
            // Verify that the ID provided in the URL matches the ID in the request body.
            if (id != productDto.ProductId)
                return BadRequest(new { error = "Product ID in URL and body do not match." });
            // Validate asynchronously using FluentValidation
            var validationResult = await _updateValidator.ValidateAsync(productDto);

            if (!validationResult.IsValid)
            {
                var errors = validationResult.Errors.Select(e => new
                {
                    Field = e.PropertyName,
                    Error = e.ErrorMessage
                });
                return BadRequest(new { Errors = errors });
            }
            // Retrieve the existing product from the database, including its associated Tags.
            var product = await _context.Products
                                        .Include(p => p.Tags)
                                        .FirstOrDefaultAsync(p => p.ProductId == id);

            // If the product does not exist, return a 404 Not Found response.
            if (product == null)
                return NotFound();
            // Update the product properties with the new values from the DTO.
            product.SKU = productDto.SKU;
            product.Name = productDto.Name;
            product.Price = productDto.Price;
            product.Stock = productDto.Stock;
            product.CategoryId = productDto.CategoryId;
            product.Description = productDto.Description;
            product.Discount = productDto.Discount;
            product.ManufacturingDate = productDto.ManufacturingDate;
            product.ExpiryDate = productDto.ExpiryDate;
            // Clear the existing Tags associated with the product.
            product.Tags.Clear();
            // Process Tags: For each tag provided in the DTO, normalize and add it.
            if (productDto.Tags != null && productDto.Tags.Any())
            {
                foreach (var tagName in productDto.Tags)
                {
                    // Normalize the tag by trimming whitespace and converting to lower case.
                    var normalizedTagName = tagName.Trim().ToLower();
                    var existingTag = await _context.Tags.FirstOrDefaultAsync(t => t.Name.ToLower() == normalizedTagName);
                    if (existingTag != null)
                        product.Tags.Add(existingTag); // Associate the existing tag with the product.
                    else
                        product.Tags.Add(new Tag { Name = normalizedTagName }); // Create and add a new tag.
                }
            }
            // Mark the product entity as updated in the database context.
            _context.Products.Update(product);
            // Save changes to persist the updated product (and tag associations) to the database.
            await _context.SaveChangesAsync();
            // Map the updated product entity to a ProductResponseDTO.
            var response = new ProductResponseDTO
            {
                ProductId = product.ProductId,
                SKU = product.SKU,
                Name = product.Name,
                Price = product.Price,
                Stock = product.Stock,
                CategoryId = product.CategoryId,
                Description = product.Description,
                Discount = product.Discount,
                ManufacturingDate = product.ManufacturingDate,
                ExpiryDate = product.ExpiryDate,
                Tags = product.Tags.Select(t => t.Name).ToList()
            };
            // Return the updated product with an HTTP 200 OK status.
            return Ok(response);
        }
    }
}
```

#### Configure the Database Connection String in the appsettings.json file
To connect our DbContext to a database, we need to add a connection string in our `appsettings.json` file. So, please modify the `appsettings.json` file as follows:

```JSON
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=localhost;Initial Catalog=fluent_validation_db;Integrated Security=True;Pooling=False;Encrypt=True;Trust Server Certificate=True"
  }
}
```

### Register FluentValidation and DbContext Services in Program Class
Next, we need to register the DbContext and FluentValidation services to the Dependency Injection container.

```C#
using Fluent_API_Validation.Data;
using Fluent_API_Validation.DTOs;
using Fluent_API_Validation.Validators;
using FluentValidation;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

// Add controllers with JSON options
builder.Services.AddControllers()
    .AddJsonOptions(options =>
    {
        options.JsonSerializerOptions.PropertyNamingPolicy = null;
    });

// Configure OpenAPI
builder.Services.AddOpenApi();

// Register the ECommerceDbContext with dependency injection
builder.Services.AddDbContext<ECommerceDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection"),
        sqlServerOptions => sqlServerOptions.EnableRetryOnFailure(
            maxRetryCount: 5,
            maxRetryDelay: TimeSpan.FromSeconds(30),
            errorNumbersToAdd: null)));

// Register validators for specific DTOs
builder.Services.AddScoped<IValidator<ProductCreateDTO>, ProductCreateDTOValidator>();
builder.Services.AddScoped<IValidator<ProductUpdateDTO>, ProductUpdateDTOValidator>();

var app = builder.Build();

// Configure the HTTP request pipeline
if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
}

app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

#### Generate and Apply Database Migration:

```Bash
# 1. Make model changes
# 2. Create migration
dotnet ef migrations add AddNewFeature

# 3. Review migration files
# 4. Apply to database
dotnet ef database update

# 5. If needed, rollback
dotnet ef database update PreviousMigration
```

### Test the POST Endpoint (Create Product)

Create a New Product
Method: POST
URL: https://localhost:5001/api/products
Headers: Content-Type: application/json
Body: Use a sample JSON like the one below (adjust values as needed)

```JSON
{
  "SKU": "ELEC1234",
  "Name": "Ultra HD TV",
  "Price": 1200.00,
  "Stock": -15,
  "CategoryId": 2,
  "Description": "55-inch Ultra HD Smart TV",
  "Discount": 105,
  "ManufacturingDate": "2023-01-15T00:00:00",
  "ExpiryDate": "2024-01-15T00:00:00",
  "Tags": [ "electronics", "home" ]
}
```
You should get the following error message.

![image_306.png](image_306.png)

#### Differences Between Fluent API and Data Annotation Validation in ASP.NET Core Web API:

![image_307.png](image_307.png)

When to Use Which Approach
* Use **Data Annotations** for simple scenarios where validation rules are straightforward. There is no need to reuse them across different models. They are easy to implement and understand.
* Use **Fluent API** Validation for complex, reusable, or dynamic validation scenarios where separating the validation logic from the model helps maintain cleaner code. It’s also ideal when the same validation logic is needed across multiple models.


> Note: The FluentValidation.AspNetCore package is no longer maintained. Microsoft recommends to use the core FluentValidation package and adopting a manual validation approach.
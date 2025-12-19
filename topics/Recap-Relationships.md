# [Recap Workshop] Relationships

Entity Framework Core (EF Core) relationships allow us to model real-world associations between business entities. In an E-Commerce application, every entity interacts with others, customers place orders, orders contain products, products belong to categories, and so on. At the end of this session, you will understand the following pointers:

## Why Do We Need Relationships in Entity Framework Core?
In a relational database, data is spread across multiple tables, and those tables must be able to “talk” to each other. Relationships ensure this happens correctly and efficiently. Every business domain has entities that are logically connected to one another. For example, in an Ecommerce application:

* A Customer **places** Orders.
* Each Order **contains multiple** Products.
* Each Product **belongs to a** specific Category.

These are not random objects. They are connected through **Relationships** that define how one entity interacts with another.

What Are the Different Types of Relationships in EF Core?
EF Core supports three core types of relationships between entities, reflecting real-world relational database design:

* **One-to-One (1:1)** Relationship
* **One-to-Many (1:M)** Relationship
* **Many-to-Many (M:M)** Relationship

## E-Commerce Order Management API

The E-Commerce Web API, built with ASP.NET Core and Entity Framework Core, is a scalable, real-world online shopping backend system. It models how customers browse, order, and purchase products using a relational SQL Server database, while maintaining clean separation of concerns through **entities**, **relationships**, and **data-access abstractions**.

This project demonstrates how EF Core manages **different types of entity relationships (1:1, 1:M, M:M)** using the **EF Core Code-First Approach**. It focuses on designing a domain-driven model that captures all the logical connections found in a real-life commerce environment.

### Key Entities and Their Roles
1. **Customer** → The main user of the system. Each customer can have one Profile, multiple Addresses, and multiple Orders.
2. **Profile** → Holds personal information like gender and birth date. It has a strict 1:1 relationship with its customer.
3. **Address** → Stores delivery and billing locations. A Customer can own many addresses (1:M).
4. **Category** → Organizes products into logical groups such as “Electronics” or “Clothing.”
5. **Product** → Represents sellable items, each belonging to exactly one Category but linked to many Orders via OrderItems.
6. **Order** → Captures a customer’s purchase record and links to OrderItems, Addresses, and OrderStatus.
7. **OrderItem** → Acts as a junction table connecting Orders and Products, supporting a Many-to-Many relationship and storing transactional details like quantity and price.
8. **OrderStatus** → A lookup entity seeded from an enum, ensuring standardized status values like Pending, Confirmed, Shipped, Delivered, and Cancelled.
9. **BaseAuditableEntity** → A reusable base class that provides auditing and soft-delete columns (CreatedAt, UpdatedAt, CreatedBy, IsActive), ensuring consistency across all tables.

#### One-to-Many (1:M) Relationship

A One-to-Many (1:M) relationship means that **a single record in the Principal Entity** can be linked to **multiple records in the Dependent Entity**. However, each record in the dependent entity is always related to only **one principal entity**.
Such relationships are typically defined using:

1. A **Collection Navigation Property** (`ICollection<T>`) on the **principal side**, and
2. A **Foreign Key Property** on the dependent side with a **Reference Navigation Property**.

##### Example 1: Customer ↔ Address
* A **Customer** can have **multiple Addresses**, for example, Home, Office, Billing, or Shipping.
* Each **Address** belongs to **exactly one Customer**.

**Here:**

**Customer → Principal Entity**
**Address → Dependent Entity** (It contains a foreign key pointing to the Customer table)

##### Example 2: Category ↔ Product
A single **Category** (e.g., “Electronics”) can include many **Products** (e.g., Laptop, Camera, TV).
Each **Product** is linked to only **one Category**.

**Here:**

* **Category → Principal Entity**
* **Product → Dependent Entity** (It contains a foreign key pointing to the Category table)

##### Example 3: Customer ↔ Order
* A single **Customer** can place **multiple Orders** over time.
* Each **Order** belongs to **exactly one Customer**.

**Here:**

* **Customer → Principal Entity**
* **Order → Dependent Entity** (It contains a foreign key pointing to the Customer table)

> In Simple Words, a One-to-Many relationship means One parent → Many children. It models real-world scenarios like:
* > One **Customer** has many **Addresses** and **Orders**.
* > One **Category** containing many **Products**.
> EF Core automatically builds and manages these links using **Foreign Keys** and **Navigation Properties**, keeping both our database and our object model perfectly aligned. 

#### Many-to-Many (M:M) Relationship

A Many-to-Many (M:M) relationship **means that multiple records in one entity** can be linked to **multiple records in another entity**, and vice versa. In simple terms, **`each record in Entity A can be associated with several records in Entity B, and each record in Entity B can also be associated with several records in Entity A`**.

Because relational databases don’t support direct many-to-many relationships between two tables, **Entity Framework Core uses a junction (bridge or joining) table**, a third table that contains foreign keys from both entities. This bridge table connects the two entities and can also store additional information about their association.

##### Example: Order ↔ Product (via OrderItem)
In an E-Commerce system:

* A single **Order** can contain **multiple Products**.
* A single **Product** can appear in **multiple Orders**.

This is a perfect real-world case of a **Many-to-Many relationship**.

**How EF Core Represents It:**

To represent this relationship, we introduce a separate entity, **OrderItem**, which serves as a **linking table** between Orders and Products. The **OrderItem** entity includes:

1. `OrderId` → Foreign Key referencing Orders
2. `ProductId` → Foreign Key referencing Products

> A **Many-to-Many** relationship connects two entities **through a third entity** that acts as a bridge. In our e-commerce system:
 > * The **Order** represents the customer’s purchase.
 > * The **Product** represents items for sale.
 > * **OrderItem** connects them, showing which products belong to which orders, along with their quantities and prices.

### Create a New Project using Visual Studio

Name your project → ECommerceApp

### Install Required EF Core Packages
Open the Package Manager Console in Visual Studio: Tools → NuGet Package Manager → Package Manager Console. Then run the following commands:

* `Install-Package Microsoft.EntityFrameworkCore` -> Core EF ORM features.
* `Install-Package Microsoft.EntityFrameworkCore.SqlServer` -> SQL Server provider.
* `Install-Package Microsoft.EntityFrameworkCore.Tools` -> CLI commands like Add-Migration and Update-Database.
* `Install-Package Swashbuckle.AspNetCore` -> Swagger Docs
* `Install-Package Microsoft.AspNetCore.OpenApi` -> Swagger Schema

### Create the Folder Structure

Inside your project, create the following folders to organize your files:

* **`Models`** → Contains all entity classes that represent the database tables and define the relationships between them.
* **`Data`** → Holds the AppDbContext class and any data-related configurations like seeding or migrations.
* **`Enums`** → Stores enumerations such as OrderStatusEnum, used for predefined constant values throughout the application.
* **`DTOs`** → Contains Data Transfer Objects for handling structured request and response models between API endpoints and clients.

### Create Order Status Enum
Create a class file named **`OrderStatusEnum.cs`** within the Enums folder, then copy and paste the following code. The Enum is used in code to express allowed order states in a type-safe way and to seed the OrderStatus table, keeping domain logic readable while enforcing valid values at the database level.

```C#
namespace ECommerceOrderManagementAPI.Enums
{
    // Enum used in application logic to represent possible order statuses.
    public enum OrderStatusEnum
    {
        Pending = 1,
        Confirmed = 2,
        Shipped = 3,
        Delivered = 4,
        Cancelled = 5
    }
}
```
### Create Entity Classes

We will create 9 Entities in the models folder: `BaseAuditableEntity`, `Customer`, `Profile`, `Address`, `Category`, `Product`, `OrderStatus`, `Order`, and `OrderItem`.

We will use Default Conventions only (no Data Annotations, no Fluent API). Relationships are automatically inferred from navigation properties.

`BaseAuditableEntity.cs`

```C#
namespace ECommerceOrderManagementAPI.Models
{
    // Base class that provides soft-delete and audit tracking.
    // Every domain entity inherits this for consistency.
    public abstract class BaseAuditableEntity
    {
        // Indicates whether the record is active (used for soft delete)
        public bool IsActive { get; set; } = true;
        // Audit Columns
        public DateTime CreatedAt { get; set; } = DateTime.UtcNow;   // Record creation timestamp
        public DateTime? UpdatedAt { get; set; }                     // Last update timestamp
        public string? CreatedBy { get; set; }                       // Optional: can be set by middleware later
        public string? UpdatedBy { get; set; }                       // Optional: for tracking modifications
    }
}
```

`Customer.cs`
It serves as a Principal Entity for related data such as `Profiles`, `Addresses`, and `Orders`. It holds basic customer details such as **name, email, and phone number**, and **maintains 1:1, 1:M, and M:M relationships** through navigation properties.

```C#
using System.Net;
using static System.Runtime.InteropServices.JavaScript.JSType;

namespace ECommerceOrderManagementAPI.Models
{
    // Principal for Profile, Addresses, and Orders
    public class Customer : BaseAuditableEntity
    {
        // Primary Key (use explicit name to be clear)
        public int CustomerId { get; set; }
        // Basic Customer Info (Required by default through non-nullable refs)
        public string Name { get; set; } = null!;
        public string Email { get; set; } = null!;
        public string Phone { get; set; } = null!;
        // 1:1 → A customer may have one Profile
        public virtual Profile? Profile { get; set; }
        // 1:M → A customer can have many addresses and orders
        public virtual ICollection<Address>? Addresses { get; set; }
        public virtual ICollection<Order>? Orders { get; set; }
    }
}
```

`Profile.cs`
The **Profile** class stores extended customer personal details, such as gender, display name, and date of birth. It has a **one-to-one (1:1) relationship with the Customer**, meaning each customer has exactly one profile, and its primary key is also the foreign key referencing the customer.

```C#
using System.ComponentModel.DataAnnotations;

namespace ECommerceOrderManagementAPI.Models
{
    // Dependent in the 1:1 relationship with Customer
    // PK = FK (same property) is the canonical required 1:1 pattern.
    public class Profile : BaseAuditableEntity
    {
        // Both Primary Key and Foreign Key to Customer
        [Key] // Keep this to make the PK explicit for readers
        public int CustomerId { get; set; }
        // Required 1:1 nav back to principal Customer
        public virtual Customer Customer { get; set; } = null!;
        // Extra profile info
        public string DisplayName { get; set; } = null!;
        public string Gender { get; set; } = null!;
        public DateTime DateOfBirth { get; set; }
    }
}
```

`Address.cs`
The **Address** class stores physical addresses used for billing or shipping. It has a **one-to-many (1:M) relationship with the Customer**; one customer can have multiple addresses, but each address belongs to a single customer. It includes address-related fields like street, city, postal code, and country.

```C#
namespace ECommerceOrderManagementAPI.Models
{
    // Dependent in 1:M with Customer
    public class Address : BaseAuditableEntity
    {
        public int AddressId { get; set; }
        public string Line1 { get; set; } = null!;
        public string? Line2 { get; set; }
        public string Street { get; set; } = null!;
        public string City { get; set; } = null!;
        public string PostalCode { get; set; } = null!;
        public string Country { get; set; } = null!;
        // REQUIRED Relationship: Every Address must belong to a Customer
        public int CustomerId { get; set; }
        public virtual Customer Customer { get; set; } = null!;
    }
}
```

`Category.cs`

The Category class represents a logical grouping of products, such as “Electronics” or “Books.” It is a **Principal Entity** in a one-to-many relationship with Product. Each category can have multiple products, enabling easier organization, filtering, and reporting of items by category.

```C#
namespace ECommerceOrderManagementAPI.Models
{
    // Principal in 1:M with Product
    public class Category : BaseAuditableEntity
    {
        public int CategoryId { get; set; }
        public string Name { get; set; } = null!;
        public string? Description { get; set; }
        // One Category → Many Products
        public virtual ICollection<Product>? Products { get; set; }
    }
}
```

`Product.cs`
The Product class represents individual items available for purchase. It belongs to one category and can appear in multiple orders through the **OrderItem joining entity**. It contains properties like name, price, stock, SKU, and description, making it central to the product catalog.

```C#
using System.ComponentModel.DataAnnotations.Schema;

namespace ECommerceOrderManagementAPI.Models
{
    // Dependent in 1:M with Category, and principal for OrderItems
    public class Product : BaseAuditableEntity
    {
        public int ProductId { get; set; }
        public string Name { get; set; } = null!;
        [Column(TypeName = "decimal(18,2)")]
        public decimal Price { get; set; }
        public int Stock { get; set; }
        public string SKU { get; set; } = null!;
        public string? Description { get; set; }
        // Required FK to Category (1 Product belongs to 1 Category)
        public int CategoryId { get; set; }
        public virtual Category Category { get; set; } = null!;
        // Many-to-Many → OrderItem acts as the bridge entity
        public virtual ICollection<OrderItem>? OrderItems { get; set; }
    }
}
```

`OrderStatus.cs`

The OrderStatus class serves as a Master Lookup Table for storing predefined order states such as `Pending`, `Confirmed`, `Shipped`, `Delivered`, and `Cancelled`. It maintains a one-to-many relationship with the Order entity, ensuring consistent tracking of an order’s progress throughout its lifecycle.

```C#
namespace ECommerceOrderManagementAPI.Models
{
    // Represents a master table for all possible order statuses.
    // The enum values are seeded here for database persistence.
    public class OrderStatus : BaseAuditableEntity
    {
        public int OrderStatusId { get; set; }     // Primary Key
        public string Name { get; set; } = null!;  // e.g., "Pending", "Shipped", etc.
        public string? Description { get; set; }
        // Navigation Property (1:M) → One status can be assigned to many orders
        public virtual ICollection<Order>? Orders { get; set; }
    }
}
```

`Order.cs`

The Order class represents a single placed order. It connects customers, addresses, and products through various relationships: **1:M with OrderItems, 1:M with Customer**, and **M:M via OrderItem with Product**. It tracks order details, including the total amount, order date, and order status.

```C#
using System.ComponentModel.DataAnnotations.Schema;

namespace ECommerceOrderManagementAPI.Models
{
    // Dependent entity in 1:M with Customer
    // Principal in 1:M with OrderItems
    public class Order : BaseAuditableEntity
    {
        public int OrderId { get; set; }
        public DateTime OrderDate { get; set; }
        // REQUIRED: Each Order should have a valid Status
        public int OrderStatusId { get; set; }
        public virtual OrderStatus OrderStatus { get; set; } = null!;
        [Column(TypeName = "decimal(18,2)")]
        public decimal TotalAmount { get; set; }
        // Optional: Billing & Shipping addresses are Optional
        public int? ShippingAddressId { get; set; }
        public virtual Address? ShippingAddress { get; set; }
        public int? BillingAddressId { get; set; }
        public virtual Address? BillingAddress { get; set; }
        // REQUIRED: Who placed it
        public int CustomerId { get; set; }
        public virtual Customer Customer { get; set; } = null!;
        // 1:M → Each Order can have multiple items
        public virtual ICollection<OrderItem> OrderItems { get; set; } = null!;
    }
}
```

`OrderItem.cs`

The OrderItem class is a **Junction Entity** between Orders and Products, enabling a many-to-many relationship. It stores item-level details, such as product ID, quantity, and unit price, for each product in an order.

```C#
using System.ComponentModel.DataAnnotations.Schema;

namespace ECommerceOrderManagementAPI.Models
{
    // Dependent in 1:M with Order
    // Dependent in 1:M with Product
    // This is the Joining Entity enabling a Many-to-Many between Orders and Products.
    public class OrderItem : BaseAuditableEntity
    {
        public int OrderItemId { get; set; }
        public int Quantity { get; set; }
        [Column(TypeName = "decimal(18,2)")]
        public decimal UnitPrice { get; set; } // snapshot of price at purchase time
        // Foreign Keys
        // REQUIRED: each OrderItem belongs to an Order
        public int OrderId { get; set; }
        public virtual Order Order { get; set; } = null!;
        // REQUIRED: each OrderItem refers to a Product
        public int ProductId { get; set; }
        public virtual Product Product { get; set; } = null!;
    }
}
```

> Note: All navigations are **virtual**, so **you can later enable lazy loading if you wish**; for now, we will use eager loading in controller reads.

### Create the DbContext

Create a class file named **AppDbContext.cs** within the **Data folder**, then copy and paste the following code. The AppDbContext class is the **EF Core bridge to the SQL Server database**. It defines all DbSet properties for entities, manages relationships automatically through conventions, and seeds initial data for Customers, Products, Categories, Orders, and Order Statuses using the HasData() method.

```C#
using ECommerceOrderManagementAPI.Models;
using Microsoft.EntityFrameworkCore;

namespace ECommerceOrderManagementAPI.Data
{
    public class AppDbContext : DbContext
    {
        public AppDbContext(DbContextOptions<AppDbContext> options) : base(options) { }
        public DbSet<Customer> Customers { get; set; }
        public DbSet<Profile> Profiles { get; set; }
        public DbSet<Address> Addresses { get; set; }
        public DbSet<Category> Categories { get; set; }
        public DbSet<Product> Products { get; set; }
        public DbSet<Order> Orders { get; set; }
        public DbSet<OrderItem> OrderItems { get; set; }
        public DbSet<OrderStatus> OrderStatuses { get; set; }
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            DateTime seedDate = new(2025, 01, 01, 10, 00, 00);
            // Order Status
            modelBuilder.Entity<OrderStatus>().HasData(
                new OrderStatus { OrderStatusId = 1, Name = "Pending", Description = "Awaiting confirmation", CreatedAt = seedDate },
                new OrderStatus { OrderStatusId = 2, Name = "Confirmed", Description = "Confirmed by admin", CreatedAt = seedDate },
                new OrderStatus { OrderStatusId = 3, Name = "Shipped", Description = "Dispatched to courier", CreatedAt = seedDate },
                new OrderStatus { OrderStatusId = 4, Name = "Delivered", Description = "Delivered successfully", CreatedAt = seedDate },
                new OrderStatus { OrderStatusId = 5, Name = "Cancelled", Description = "Cancelled by user/system", CreatedAt = seedDate }
            );
            // Customers
            modelBuilder.Entity<Customer>().HasData(
                new Customer { CustomerId = 1, Name = "Pranaya Rout", Email = "pranaya@example.com", Phone = "9876543210", CreatedAt = seedDate },
                new Customer { CustomerId = 2, Name = "Rakesh Kumar", Email = "rakesh@example.com", Phone = "9876543211", CreatedAt = seedDate }
            );
            // Profiles 
            modelBuilder.Entity<Profile>().HasData(
                new Profile { CustomerId = 1, DisplayName = "Pranaya", Gender = "Male", DateOfBirth = new(1990, 05, 10), CreatedAt = seedDate },
                new Profile { CustomerId = 2, DisplayName = "Rakesh", Gender = "Female", DateOfBirth = new(1992, 08, 22), CreatedAt = seedDate }
            );
            // Addresses
            modelBuilder.Entity<Address>().HasData(
                new Address { AddressId = 1, Line1 = "123 Market Street", Street = "Main Rd", City = "Bhubaneswar", PostalCode = "751001", Country = "India", CustomerId = 1, CreatedAt = seedDate },
                new Address { AddressId = 2, Line1 = "Tech Park Avenue", Street = "Silicon Street", City = "Cuttack", PostalCode = "753001", Country = "India", CustomerId = 1, CreatedAt = seedDate },
                new Address { AddressId = 3, Line1 = "45 Lake View", Street = "West Road", City = "Bhubaneswar", PostalCode = "751010", Country = "India", CustomerId = 2, CreatedAt = seedDate }
            );
            // Categories
            modelBuilder.Entity<Category>().HasData(
                new Category { CategoryId = 1, Name = "Electronics", Description = "Electronic Devices", CreatedAt = seedDate },
                new Category { CategoryId = 2, Name = "Books", Description = "Educational and Fiction", CreatedAt = seedDate }
            );
            // Products
            modelBuilder.Entity<Product>().HasData(
                new Product { ProductId = 1, Name = "Wireless Mouse", Price = 1200, Stock = 50, SKU = "ELEC-MOUSE-001", CategoryId = 1, CreatedAt = seedDate },
                new Product { ProductId = 2, Name = "Bluetooth Headphones", Price = 2500, Stock = 40, SKU = "ELEC-HEAD-002", CategoryId = 1, CreatedAt = seedDate },
                new Product { ProductId = 3, Name = "C# Programming Book", Price = 899, Stock = 100, SKU = "BOOK-CSPROG-003", CategoryId = 2, CreatedAt = seedDate }
            );
            // Orders
            modelBuilder.Entity<Order>().HasData(
                new Order { OrderId = 1, OrderDate = seedDate, OrderStatusId = 2, TotalAmount = 3700, ShippingAddressId = 1, BillingAddressId = 2, CustomerId = 1, CreatedAt = seedDate },
                new Order { OrderId = 2, OrderDate = seedDate.AddDays(1), OrderStatusId = 3, TotalAmount = 899, ShippingAddressId = 3, BillingAddressId = 3, CustomerId = 2, CreatedAt = seedDate }
            );
            // OrderItems
            modelBuilder.Entity<OrderItem>().HasData(
                new OrderItem { OrderItemId = 1, OrderId = 1, ProductId = 1, Quantity = 1, UnitPrice = 1200, CreatedAt = seedDate },
                new OrderItem { OrderItemId = 2, OrderId = 1, ProductId = 2, Quantity = 1, UnitPrice = 2500, CreatedAt = seedDate },
                new OrderItem { OrderItemId = 3, OrderId = 2, ProductId = 3, Quantity = 1, UnitPrice = 899, CreatedAt = seedDate }
            );
        }
    }
}

```

### Configure Database Connection
Please add the database connection string in the `appsettings.json` file as follows:

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
    "DefaultConnection": "Data Source=localhost;Initial Catalog=order_management_db;Integrated Security=True;Pooling=False;Encrypt=True;Trust Server Certificate=True"
  }
}
```

### Configure DbContext in Program.cs
Please modify the Program class as follows.

```C#
using ECommerceOrderManagementAPI.Data;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.

builder.Services.AddControllers()
    .AddJsonOptions(options =>
    {
        // This will use the property names as defined in the C# model
        //options.JsonSerializerOptions.PropertyNamingPolicy = null;

        // This makes JSON property name matching case-insensitive ie "customerId" or "CustomerID" will bind to CustomerId
        options.JsonSerializerOptions.PropertyNameCaseInsensitive = true;
    });

// Register DbContext and Connection String
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));


builder.Services.AddSwaggerGen(options =>
{
    // Define API metadata (title, version, description, contact) shown in Swagger UI
    options.SwaggerDoc("v1", new()
    {
        Title = "E-Commerce Order Management API",
        Version = "v1",
        Description = "API for managing e-commerce orders, products, customers, and order processing operations.",
        Contact = new()
        {
            Name = "API Support",
            Email = "support@ecommerceordermanagement.com",
            Url = new Uri("https://www.ecommerceordermanagement.com/support")
        }
    });
});

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    // Enable middleware to serve generated Swagger as JSON endpoint
    app.UseSwagger();
    // Enable middleware to serve Swagger UI (HTML, JS, CSS, etc.)
    // Provides interactive documentation at /swagger
    app.UseSwaggerUI(options =>
    {
        // Configure the JSON endpoint for Swagger UI to consume
        options.SwaggerEndpoint("/swagger/v1/swagger.json", "E-Commerce Order Management API V1");
    });
}

app.UseHttpsRedirection();

app.UseAuthorization();

app.MapControllers();

app.Run();
```

### Creating DTOs

Let us create the DTOs for exchanging the Data.

`OrderItemRequestDTO.cs`

Create a class file named OrderRequestDTO.cs within the DTOs folder, then copy and paste the following code. It is used for placing new orders. It includes customer ID, address IDs, and a list of ordered items

```C#
using System.ComponentModel.DataAnnotations;

namespace ECommerceOrderManagementAPI.DTOs
{
    // Represents an individual product item in a new order request
    public class OrderItemRequestDTO
    {
        // The product being ordered
        [Required(ErrorMessage = "ProductId is required.")]
        [Range(1, int.MaxValue, ErrorMessage = "ProductId must be a positive integer.")]
        public int ProductId { get; set; }
        // Quantity of that product
        [Required(ErrorMessage = "Quantity is required.")]
        [Range(1, 1000, ErrorMessage = "Quantity must be between 1 and 1000.")]
        public int Quantity { get; set; }
    }
}
```

`OrderRequestDTO.cs`

Create a class file named OrderRequestDTO.cs within the DTOs folder, then copy and paste the following code. It is used for placing new orders. It includes customer ID, address IDs, and a list of ordered items.

```C#
using System.ComponentModel.DataAnnotations;

namespace ECommerceOrderManagementAPI.DTOs
{
    // Represents the full data required to place a new order
    public class OrderRequestDTO
    {
        // The customer placing the order
        [Required(ErrorMessage = "CustomerId is required.")]
        [Range(1, int.MaxValue, ErrorMessage = "CustomerId must be a positive integer.")]
        public int CustomerId { get; set; }
        // Chosen shipping address
        [Required(ErrorMessage = "ShippingAddressId is required.")]
        [Range(1, int.MaxValue, ErrorMessage = "ShippingAddressId must be a positive integer.")]
        public int ShippingAddressId { get; set; }
        // Chosen billing address
        [Required(ErrorMessage = "BillingAddressId is required.")]
        [Range(1, int.MaxValue, ErrorMessage = "BillingAddressId must be a positive integer.")]
        public int BillingAddressId { get; set; }
        // List of products in the order
        [Required(ErrorMessage = "Order must contain at least one item.")]
        [MinLength(1, ErrorMessage = "At least one order item must be provided.")]
        public List<OrderItemRequestDTO> Items { get; set; } = new();
    }
}

```

`OrderItemResponseDTO.cs`

Create a class file named OrderItemResponseDTO.cs within the DTOs folder, then copy and paste the following code. It represents each ordered product in response payloads with product and category info.

```C#
namespace ECommerceOrderManagementAPI.DTOs
{
    // Represents an item inside an order when returning data
    public class OrderItemResponseDTO
    {
        public string ProductName { get; set; } = null!;
        public string Category { get; set; } = null!;
        public int Quantity { get; set; }
        public decimal UnitPrice { get; set; }
    }
}
```
`CustomerResponseDTO.cs`

Create a class file named CustomerResponseDTO.cs within the DTOs folder, then copy and paste the following code. It returns summarized customer data, including name, email, and addresses.

```C#
namespace ECommerceOrderManagementAPI.DTOs
{
    // Summarized view of the customer associated with an order
    public class CustomerResponseDTO
    {
        public int CustomerId { get; set; }
        public string Name { get; set; } = null!;
        public string Email { get; set; } = null!;
        public string? Phone { get; set; }
        public string? DisplayName { get; set; }
        public string? Gender { get; set; }
        public string? DateOfBirth { get; set; }
    }
}
```

`OrderResponseDTO.cs`

Create a class file named OrderResponseDTO.cs within the DTOs folder, then copy and paste the following code. It wraps complete order details (customer info, order items, and addresses) for API responses.

```C#
// Represents an order returned from API endpoints
namespace ECommerceOrderManagementAPI.DTOs
{
    // Represents an order returned from API endpoints
    public class OrderResponseDTO
    {
        public int OrderId { get; set; }
        public string OrderDate { get; set; } = null!;
        public string Status { get; set; } = null!;
        public decimal TotalAmount { get; set; }
        public string ShippingAddress { get; set; } = null!;
        public string BillingAddress { get; set; } = null!;
        public CustomerResponseDTO Customer { get; set; } = null!;
        public List<OrderItemResponseDTO> Items { get; set; } = new();
    }
}

```

### Create Order API Controller

Please create a new Empty API Controller named **OrderController** within the **Controllers folder** and add the following code to it. The **OrderController** handles all **API Endpoints** related to order management, such as retrieving all orders, fetching by ID, and placing new orders. It demonstrates how EF Core relationships are utilized in queries and DTO mapping for clean data transfer. Now, we have only implemented the Place Order method; later, we will implement the other methods.

```C#
using ECommerceOrderManagementAPI.Data;
using ECommerceOrderManagementAPI.DTOs;
using ECommerceOrderManagementAPI.Enums;
using ECommerceOrderManagementAPI.Models;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;

// For more information on enabling Web API for empty projects, visit https://go.microsoft.com/fwlink/?LinkID=397860

namespace ECommerceOrderManagementAPI.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class OrderController : ControllerBase
    {
        private readonly AppDbContext _context;

        public OrderController(AppDbContext context)
        {
            _context = context;
        }

       
        // POST api/<OrderController>
        // PURPOSE: Places a new order with validation and business rules
        // Demonstrates: Validations, relationships, transactions, soft audit fields
        [HttpPost]
        public async Task<IActionResult> PlaceOrder([FromBody] OrderRequestDTO request)
        {
            try
            {
                // STEP 1: Validate Customer existence
                var customer = await _context.Customers
                    .Include(c => c.Addresses)
                    .FirstOrDefaultAsync(c => c.CustomerId == request.CustomerId && c.IsActive);

                if (customer == null)
                {
                    return BadRequest($"customer {request.CustomerId} not found or inactive");
                }

                // STEP 2: Validate Shipping & Billing Addresses (must belong to the same customer)
                var shipping = await _context.Addresses
                  .FirstOrDefaultAsync(a => a.AddressId == request.ShippingAddressId && a.CustomerId == request.CustomerId);
                var billing = await _context.Addresses
                    .FirstOrDefaultAsync(a => a.AddressId == request.BillingAddressId && a.CustomerId == request.CustomerId);
                if (shipping == null || billing == null)
                    return BadRequest("Invalid Shipping or Billing Address for this customer.");

                // STEP 3: Validate Products & Stock availability
                var productIds = request.Items.Select(i => i.ProductId).ToList();
                var products = await _context.Products
                    .Where(p => productIds.Contains(p.ProductId) && p.IsActive)
                    .ToListAsync();

                // Check product existence
                if (products.Count != request.Items.Count)
                    return BadRequest("Some products are invalid or inactive.");
                // Check stock for each product
                foreach (var item in request.Items)
                {
                    var product = products.First(p => p.ProductId == item.ProductId);
                    if (product.Stock < item.Quantity)
                        return BadRequest($"Insufficient stock for '{product.Name}'. Available: {product.Stock}, Requested: {item.Quantity}");
                }

                // STEP 4: Create new Order object
                var order = new Order
                {
                    CustomerId = request.CustomerId,
                    ShippingAddressId = request.ShippingAddressId,
                    BillingAddressId = request.BillingAddressId,
                    OrderStatusId = (int)OrderStatusEnum.Pending,
                    OrderDate = DateTime.UtcNow,
                    CreatedAt = DateTime.UtcNow,
                    CreatedBy = "System",
                    IsActive = true
                };

                // STEP 5: Map Order Items and Deduct Stock
                order.OrderItems = new List<OrderItem>();
                foreach (var item in request.Items)
                {
                    var product = products.First(p => p.ProductId == item.ProductId);

                    // Reduce stock from product
                    product.Stock -= item.Quantity;
                    product.UpdatedAt = DateTime.UtcNow;
                    product.UpdatedBy = "System";

                    // Create order item record
                    order.OrderItems.Add(new OrderItem
                    {
                        ProductId = product.ProductId,
                        Quantity = item.Quantity,
                        UnitPrice = product.Price // use price from DB, not client
                    });
                }

                // STEP 6: Compute total amount
                order.TotalAmount = order.OrderItems.Sum(i => i.Quantity * i.UnitPrice);

                // STEP 7: Save changes(transactionally)
                _context.Orders.Add(order);
                await _context.SaveChangesAsync();

                // STEP 8: Return success response
                return Ok(new
                {
                    Message = "Order placed successfully.",
                    OrderId = order.OrderId
                });


            }
            catch (Exception ex)
            {
                return StatusCode(500, new
                {
                    Message = "Unexpected error occurred while placing the order.",
                    ErrorMessage = ex.Message
                });
            }
        }

    }
}

```

### Run Migrations
Open the **Package Manager Console** and execute:

```bash
# Create migration
dotnet ef migrations add AddNewFeature
```

```bash
# Apply to database
dotnet ef database update
```



EF Core will:

* Create the database ECommerceDB.
* Create all tables.
* Insert the seed data defined in AppDbContext.
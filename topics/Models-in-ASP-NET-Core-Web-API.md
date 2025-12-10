# Models in ASP.NET Core Web API

In ASP.NET Core Web API, **Models** are **C# Classes** that define the shape and structure of the data your application works with. They describe how data is structured, how it relates to other data, and how it should be validated before being stored or returned. Models act as the **blueprint** for both database entities and data transfer between the client and server.

### Key Features of Models:

The following are the Key Features of Models in ASP.NET Core Web API:

#### Data Representation (POCOs)

Models are typically POCOs (Plain Old CLR Objects), simple classes that do not depend on any base class or framework inheritance. They define properties that reflect the data structure of your application or API.

````C#
namespace MyFirstWebAPIProject.Models
{
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; } = null!;
        public decimal Price { get; set; }
        public string Category { get; set; } = null!;
    }
}
````

> These models are used by EF Core (if enabled) to map to database tables.

#### Data Validation with Data Annotations

Models can be **annotated with attributes** from the `System.ComponentModel.DataAnnotations` namespace to apply validation rules. `These rules validate incoming data automatically before it reaches business logic`.

Common Data Annotations:

* `[Required]` → ensures a property is not null or empty.
* `[StringLength(50)]` → restricts text length.
* `[Range(1, 100)]` → enforces numeric ranges.
* `[RegularExpression]` → checks formats (e.g., email, phone numbers).

This reduces bugs and ensures the integrity of incoming data. For example:

```C#
using System.ComponentModel.DataAnnotations;
public class Product
{
    public int Id { get; set; }

    [Required]
    [StringLength(100, MinimumLength = 3)]
    public string Name { get; set; } = null!;

    [Range(1, 10000.00)]
    public decimal Price { get; set; }

    [Required]
    public string Category { get; set; } = null!;
}
```

#### Defining Relationships (EF Core Models)
When used with Entity Framework Core, models can define relationships such as:

This is done using **Navigation Properties** and **foreign key conventions**. For example, A Product belongs to one Category, while a Category can have many Products (one-to-many).

* One-to-Many
* One-to-One
* Many-to-Many

```C#
public class Category
{
    public int Id { get; set; }
    public string Name { get; set; }

    // One-to-Many
    public List<Product> Products { get; set; }
}

public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }

    public int CategoryId { get; set; } // FK
    public Category Category { get; set; }
}
```

**`Navigation properties (Category Category, List<Product> Products) allow EF Core to manage relationships`** and **`enable LINQ queries across related data`**. EF Core uses these relationships to generate the correct schema and handle joins.

#### DTOs (Data Transfer Objects)

**DTOs** are not **Entity Models** — they are **Lightweight Classes** designed to:

* Models can also act as **DTOs**, which are specialized objects for shaping the data exposed by the API.
* Hide sensitive/internal fields (like passwords or internal IDs) and only return what the client actually needs.
* DTOs improve **security, performance, and clarity** of API responses by sending only required data.

**Example:**

```C#
public class ProductDTO
{
    public string Name { get; set; }
    public decimal Price { get; set; }
}
```

> Note: You typically need to use AutoMapper or manual mapping to convert between Entity Models and DTOs

#### Entity Framework Core Integration
When working with EF Core:

* Models map to database **tables**.
* Properties map to **columns**.
* Annotations (such as [Key] and [ForeignKey]) control schema behavior.

EF Core uses these models to generate SQL commands for **CRUD** (Create, Read, Update, Delete) operations.

```C#
public class ApplicationDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
}
```

EF Core will map Product to a Products table and generate SQL queries like `SELECT * FROM Products`.
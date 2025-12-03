# Relationships in EF

In EF Core you declare entity types — your domain classes — and relationships between them using navigation properties and optionally configuration (via attributes or Fluent API).
The main kinds of relationships supported are:

* One-to-One (1:1) — one entity is associated with exactly one of another.
* One-to-Many / Many-to-One (1:n / n:1) — one entity has many related entities, each of which refers to one parent.
* Many-to-Many (n:n) — many entities on one side relate to many on the other. EF Core supports this — either with an explicit join entity or via “skip‑navigation” so that EF manages the join table under the hood.

## Blogging API Entity Design

This document describes a recommended data model for a blogging API using TypeORM. It includes entity definitions and relationship mappings for the following domain objects:

* Users
* Profiles
* Authors
* Blogs
* Comments
* Categories

The design includes one-to-one (1-1), one-to-many (1-n), and many-to-many (n-n) relationships and example TypeORM decorator usage for each entity.

### High-level relationship summary
* User - Profile: One-to-One (1-1) : Each User has exactly one Profile that contains profile metadata.
* User - Author: One-to-One (1-1) : A User can be an Author. Author contains publishing-related data.
* Author - Blog: One-to-Many (1-n) : An Author can write many Blog posts.
* Blog - Comment: One-to-Many (1-n) : A Blog can have many Comments.
* Blog - Category: Many-to-Many (n-n) : Blogs can belong to multiple Category entries and vice-versa.
* User - Comment: One-to-Many (1-n) : A User can write many Comments.

## Translating Your Blog Model to EF Core (Code‑First with .NET 10)

Create a new .NET 10 console app:

```bash
dotnet new console -n BloggingApp
cd BloggingApp
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.SqlServer  # or  Sqlite etc.
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet add package Microsoft.EntityFrameworkCore.Design # Migrations
```
Then install the EF CLI tools (if not already):

```Bash
dotnet tool install --global dotnet-ef
```

You can check installation with:

```Bash
dotnet ef --version
``` :contentReference[oaicite:3]{index=3}

Ensure your project references include a `DbContext` (we’ll add below), so the CLI tools know where to operate.  

---

## 🗂 Project Structure (Suggested)
```

- Microsoft.EntityFrameworkCore
![image_239.png](image_239.png)
- Microsoft.EntityFrameworkCore.Tools
![image_240.png](image_240.png)
- Microsoft.EntityFrameworkCore.SqlServer
![image_241.png](image_241.png)

#### Recommended Folder Structure for Your Console + EF Core App

Here’s one layout that works well — starting simple, but structured enough for growth:

```lua
/YourSolutionRoot
│
├── BloggingApp.csproj         <-- your .NET project file
├── Program.cs                 <-- entry point (or minimal Host setup)
│
├── Entities/                  <-- domain entity classes (each in its own file)
│   ├── User.cs
│   ├── Profile.cs
│   ├── Author.cs
│   ├── Blog.cs
│   ├── Comment.cs
│   └── Category.cs
│
├── Data/                      <-- data‑access related: DbContext, migrations, config
│   ├── BloggingContext.cs
│   └── (optionally) Migrations/   <-- if you plan to use EF Core migrations
│
├── Services/ (optional)       <-- if you have business‑logic or services / helpers
│   └── ... 
│
├── Infrastructure/ (optional) <-- for infra‑specific code (e.g. external integrations)
│   └── ...
│
└── README.md                  <-- description, how to run/build, etc.
```

* `Entities/` — contains plain C# classes representing domain entities (User, Blog, Comment, etc.). Each entity in its own .cs file. This helps with clarity: you immediately know where to find a given type; easier for large models.
* `Data/` — contains the EF Core DbContext class (e.g. BloggingContext.cs), plus migrations folder if you generate migrations. All DB‑access and persistence-related code lives here.
* `Program.cs` — minimal application entry point. Keep it focused (e.g. configuring DbContext, running seed logic, example operations) — avoid cluttering with domain or business logic.
* Optional folders — as your app grows, you can add `Services/` (for business logic, domain services), or `Infrastructure/` (for external dependencies: email providers, file storage, etc.). Even if not needed now, having them ready helps scale cleanly.
* README.md / docs — always good for onboarding or future reference.

Add the entity classes (User, Profile, Author, Blog, Comment, Category). They currently have simple scalar properties but no relationships / navigation properties / FK fields. Example:

`User.cs` :

```C#
using System.ComponentModel.DataAnnotations;


namespace EFCore_WorkShop_CodeFirst.Entities
{
    internal class User
    {
       public Guid Id { get; set; }

        [Required]
        [MaxLength(100)]
        public string Username { get; set; } = null!;

        [Required]
        [MaxLength(200)]
        public string Email { get; set; } = null!;

        [Required]
        public string PasswordHash { get; set; } = null!;

        public bool IsActive { get; set; } = true;

        public DateTime CreatedAt { get; set; }
        public DateTime UpdatedAt { get; set; }
        // <-- no navigation or relationships yet
    }
}

```

`Profile.cs` :

```C#
using System.ComponentModel.DataAnnotations;


namespace EFCore_WorkShop_CodeFirst.Entities
{
    internal class Profile
    {
        public Guid Id { get; set; }

        [MaxLength(200)]
        public string? FullName { get; set; }

        public string? Bio { get; set; }
        public string? AvatarUrl { get; set; }

        public DateTime CreatedAt { get; set; }
        public DateTime UpdatedAt { get; set; }
        // <-- no navigation or relationships yet
    }
}
```

`Author.cs` :

```C#
using System.ComponentModel.DataAnnotations;

namespace EFCore_WorkShop_CodeFirst.Entities
{
    internal class Author
    {
        public Guid Id { get; set; }

        [MaxLength(200)]
        public string? PenName { get; set; }

        public string? Biography { get; set; }

        public DateTime CreatedAt { get; set; }
        public DateTime UpdatedAt { get; set; }
        // <-- no navigation or relationships yet
    }
}
```

`Blog.cs` :

```C#
using System.ComponentModel.DataAnnotations;

namespace EFCore_WorkShop_CodeFirst.Entities
{
    internal class Blog
    {
        public Guid Id { get; set; }

        [Required]
        [MaxLength(200)]
        public string Title { get; set; } = null!;

        [Required]
        public string Content { get; set; } = null!;

        public bool Published { get; set; } = false;
        public string? Excerpt { get; set; }

        public DateTime CreatedAt { get; set; }
        public DateTime UpdatedAt { get; set; }
        // <-- no navigation or relationships yet
    }
}

```

`Comment.cs` :

```C#


using System.ComponentModel.DataAnnotations;

namespace EFCore_WorkShop_CodeFirst.Entities
{
    internal class Comment
    {
        [Key]
        public Guid Id { get; set; }

        [Required]
        public string Content { get; set; } = null!;

        public bool IsApproved { get; set; } = false;

        public DateTime CreatedAt { get; set; }
        public DateTime UpdatedAt { get; set; }
        // <-- no navigation or relationships yet
    }
}

```

`Category.cs` :

```C#
using System.ComponentModel.DataAnnotations;

namespace EFCore_WorkShop_CodeFirst.Entities
{
    internal class Category
    {
        public Guid Id { get; set; }

        [Required]
        [MaxLength(100)]
        public string Name { get; set; } = null!;

        public string? Description { get; set; }

        public DateTime CreatedAt { get; set; }
        public DateTime UpdatedAt { get; set; }

        public List<Blog> Blogs { get; set; } = new();
        // <-- no navigation or relationships yet
    }
}

```

Create the DbContext (e.g. BloggingContext inside Data folder) as shown below 

`BloggingContext.cs`

```C#
using EFCore_WorkShop_CodeFirst.Entities;
using Microsoft.EntityFrameworkCore;


namespace EFCore_WorkShop_CodeFirst.Data
{
    internal class BloggingContext : DbContext
    {
        public DbSet<User> Users => Set<User>();
        public DbSet<Profile> Profiles => Set<Profile>();
        public DbSet<Author> Authors => Set<Author>();
        public DbSet<Blog> Blogs => Set<Blog>();
        public DbSet<Comment> Comments => Set<Comment>();
        public DbSet<Category> Categories => Set<Category>();

        protected override void OnConfiguring(DbContextOptionsBuilder options)
        {
            // Example connection string — change as needed
            options.UseSqlServer(@"Server=localhost;Initial Catalog=blogging_db;User ID=SA;Password=pass;Trust Server Certificate=True");
        }

        protected override void OnModelCreating(ModelBuilder model)
        {   
         // no relationships yet
        }
    }
}
```

#### Entity Framework Migrations

in the console run this command `dotnet add package Microsoft.EntityFrameworkCore.Design`

It is used to add the `Microsoft.EntityFrameworkCore.Design` **NuGet** package to a .NET project. This package provides tools that are needed to manage Entity Framework (EF) Core migrations and design-time operations. It is a development-time-only dependency and is not included in the production build of the application.

Here are the key purposes of the Microsoft.EntityFrameworkCore.Design package:

* Migrations Support: It provides tools that are required to create and manage EF Core migrations. Migrations are used to update the database schema based on changes in the application's data model. The package enables commands like `dotnet ef migrations add`, `dotnet ef migrations update`, and `dotnet ef database update` to be executed from the command line.
* Scaffolding Support: It allows scaffolding of models from an existing database. This can be done using commands like `dotnet ef dbcontext scaffold`, which generates entity classes based on the schema of an existing database.
* Design-Time Services: It also includes design-time services required by EF Core to generate a context and model. These services are typically used by tools like the Entity Framework Core CLI or Visual Studio for tasks such as code generation and migrations.


## Common Workflow

**Development Cycle:**
```sh
# 1. Make model changes
# 2. Create migration
dotnet ef migrations add AddNewFeature

# 3. Review migration files
# 4. Apply to database
dotnet ef database update

# 5. If needed, rollback
dotnet ef database update PreviousMigration
```

**Fresh Start:**
```sh
dotnet ef database drop --force
dotnet ef migrations remove
dotnet ef migrations add InitialCreate
dotnet ef database update
```


### Then Lets continue:
Run below commands

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update

```
You can verify the migration. This creates the database with tables for each entity — useful baseline. 

![image_242.png](image_242.png)

## One‑to‑One (1:1) relationships
User - Profile: One-to-One (1-1) : Each User has exactly one Profile that contains profile metadata.

**Modify entities to add navigation and FK:**

```C#
// Entities/User.cs — add:
public Profile? Profile { get; set; }
public Author? Author { get; set; }
```

```C#
// Entities/Profile.cs — add:
public Guid UserId { get; set; }
[ForeignKey(nameof(UserId))]
public User User { get; set; } = null!;
```

```C#
// Entities/Author.cs — add:
public Guid UserId { get; set; }
[ForeignKey(nameof(UserId))]
public User User { get; set; } = null!;
```

**Then update** `DbContext.OnModelCreating`(...):

```C#
   // One-to-One relationship (User - Profile)
   modelBuilder.Entity<User>()
       .HasOne(u => u.Profile)
       .WithOne(p => p.User)
       .HasForeignKey<Profile>(p => p.UserId)
       .OnDelete(DeleteBehavior.Cascade);

  // One-to-One relationship (User - Author)
  modelBuilder.Entity<User>()
      .HasOne(u => u.Author)
      .WithOne(a => a.User)
      .HasForeignKey<Author>(a => a.UserId)
      .OnDelete(DeleteBehavior.Cascade);
```

**Now create a new migration:**

```Bash
dotnet ef migrations add AddUserProfileAuthor_OneToOne
dotnet ef database update
```

This will alter the database — adding the necessary FK columns and constraints for 1:1 relationships.

## One‑to‑Many (1:n) relationships

**Extend entities:**

```C#
// Author.cs
public List<Blog> Blogs { get; set; } = new();

// Blog.cs
public Guid AuthorId { get; set; }
[ForeignKey(nameof(AuthorId))]
public Author Author { get; set; } = null!;
public List<Comment> Comments { get; set; } = new();

// Comment.cs
public Guid UserId { get; set; }
[ForeignKey(nameof(UserId))]
public User User { get; set; } = null!;
public Guid BlogId { get; set; }
[ForeignKey(nameof(BlogId))]
public Blog Blog { get; set; } = null!;

// Also in User.cs
public List<Comment> Comments { get; set; } = new();
```

**Fluent API mappings:**

```C#
 // One-to-Many relationship (Author - Blogs)
 modelBuilder.Entity<Author>()
     .HasMany(a => a.Blogs)
     .WithOne(b => b.Author)
     .HasForeignKey(b => b.AuthorId)
     .OnDelete(DeleteBehavior.Cascade);


// One-to-Many relationship (Blog - Comments)
modelBuilder.Entity<Blog>()
    .HasMany(b => b.Comments)
    .WithOne(c => c.Blog)
    .HasForeignKey(c => c.BlogId)
    .OnDelete(DeleteBehavior.Restrict);

// One-to-Many relationship (User - Comments)
modelBuilder.Entity<User>()
    .HasMany(u => u.Comments)
    .WithOne(c => c.User)
    .HasForeignKey(c => c.UserId)
    .OnDelete(DeleteBehavior.Restrict);
```

**Then run a new migration:**

```Bash
dotnet ef migrations add AddBlogAndComment_Relations_OneToMany
dotnet ef database update
```

The database schema will now include foreign‑key constraints mapping `authors → blogs`, `blogs → comments`, `user → comments`.

## Many‑to‑Many (n:n) between Blog and Category

**Modify entities:**

```C#
// Blog.cs
// many-to-many relationship with Category
public List<Category> Categories { get; set; } = new();
     
// Category.cs
// many-to-many relationship with Blog
public List<Blog> Blogs { get; set; } = new();
```

**Fluent API mapping:**

```C#
modelBuilder.Entity<Blog>()
    .HasMany(b => b.Categories)
    .WithMany(c => c.Blogs)
    .UsingEntity(j => j.ToTable("BlogCategories"));
```

**Then run:**

```Bash
dotnet ef migrations add AddBlogCategory_ManyToMany
dotnet ef database update
```

EF Core will create a **join table** (e.g. `BlogCategories`) linking `Blog` and `Category`.

## Create & Query Data

In `Program.cs` file add

```C#
using EFCore_WorkShop_CodeFirst.Data;
using EFCore_WorkShop_CodeFirst.Entities;
using Microsoft.EntityFrameworkCore;

class Program
{
    static void Main()
    {
        using (var context = new BloggingContext())
        {
            // Apply migrations to ensure database is up to date
            context.Database.Migrate();

            // Create a new User with related entities
            var user = new User
            {
                Id = Guid.NewGuid(),
                Username = "john_doe",
                Email = "john.doe1@example.com",
                PasswordHash = "hashedpassword",    
                IsActive = true,
                CreatedAt = DateTime.Now,
                UpdatedAt = DateTime.Now
            };

            var profile = new Profile
            {
                Id = Guid.NewGuid(),
                FullName = "John Doe",
                Bio = "Tech Enthusiast",
                AvatarUrl = "https://example.com/avatar.jpg",
                UserId = user.Id,
                CreatedAt = DateTime.Now,
                UpdatedAt = DateTime.Now
            };

            var author = new Author
            {
                Id = Guid.NewGuid(),
                PenName = "TechGuru",
                Biography = "Author of several tech blogs.",
                UserId = user.Id,
                CreatedAt = DateTime.Now,
                UpdatedAt = DateTime.Now
            };

            context.Users.Add(user);
            context.Profiles.Add(profile);
            context.Authors.Add(author);
            context.SaveChanges();

            Console.WriteLine("Data saved successfully!");

            // Query and display added entities
            var savedUser = context.Users
                .Include(u => u.Profile)
                .Include(u => u.Author)
                .FirstOrDefault(u => u.Username == "john_doe");

            Console.WriteLine($"User: {savedUser?.Username}, Profile: {savedUser?.Profile?.FullName}, Author: {savedUser?.Author?.PenName}");

            // Create categories
            var techCategory = new Category
            {
                Id = Guid.NewGuid(),
                Name = "Technology",
                Description = "All about technology and innovation",
                CreatedAt = DateTime.Now,
                UpdatedAt = DateTime.Now
            };

            var programmingCategory = new Category
            {
                Id = Guid.NewGuid(),
                Name = "Programming",
                Description = "Software development and programming tips",
                CreatedAt = DateTime.Now,
                UpdatedAt = DateTime.Now
            };

            context.Categories.Add(techCategory);
            context.Categories.Add(programmingCategory);
            context.SaveChanges();

            Console.WriteLine("Categories created successfully!");

            // Create a blog associated with the author and categories
            var blog = new Blog
            {
                Id = Guid.NewGuid(),
                Title = "Introduction to Entity Framework Core",
                Content = "Entity Framework Core is a modern object-database mapper for .NET. It supports LINQ queries, change tracking, updates, and schema migrations.",
                Excerpt = "Learn the basics of EF Core",
                Published = true,
                AuthorId = author.Id,
                CreatedAt = DateTime.Now,
                UpdatedAt = DateTime.Now,
                Categories = new List<Category> { techCategory, programmingCategory }
            };

            context.Blogs.Add(blog);
            context.SaveChanges();

            Console.WriteLine("Blog created successfully!");

            // Create comments on the blog
            var comment1 = new Comment
            {
                Id = Guid.NewGuid(),
                Content = "Great article! Very informative.",
                IsApproved = true,
                BlogId = blog.Id,
                UserId = user.Id,
                CreatedAt = DateTime.Now,
                UpdatedAt = DateTime.Now
            };

            var comment2 = new Comment
            {
                Id = Guid.NewGuid(),
                Content = "Looking forward to more posts on this topic!",
                IsApproved = true,
                BlogId = blog.Id,
                UserId = user.Id,
                CreatedAt = DateTime.Now,
                UpdatedAt = DateTime.Now
            };

            context.Comments.Add(comment1);
            context.Comments.Add(comment2);
            context.SaveChanges();

            Console.WriteLine("Comments created successfully!");

            // Query and display the complete blog with all relationships
            var blogWithDetails = context.Blogs
                .Include(b => b.Author)
                    .ThenInclude(a => a.User)
                .Include(b => b.Comments)
                    .ThenInclude(c => c.User)
                .Include(b => b.Categories)
                .FirstOrDefault(b => b.Id == blog.Id);

            if (blogWithDetails != null)
            {
                Console.WriteLine("\n--- Blog Details ---");
                Console.WriteLine($"Title: {blogWithDetails.Title}");
                Console.WriteLine($"Author: {blogWithDetails.Author.PenName} ({blogWithDetails.Author.User.Username})");
                Console.WriteLine($"Published: {blogWithDetails.Published}");
                Console.WriteLine($"Categories: {string.Join(", ", blogWithDetails.Categories.Select(c => c.Name))}");
                Console.WriteLine($"\nComments ({blogWithDetails.Comments.Count}):");
                
                foreach (var comment in blogWithDetails.Comments)
                {
                    Console.WriteLine($"  - {comment.User.Username}: {comment.Content}");
                }
            }

            Console.WriteLine("\nAll operations completed successfully!");
        }
    }
}
```
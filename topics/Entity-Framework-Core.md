# Entity Framework Core

Entity Framework Core (EF Core) is Microsoft’s **Modern, Lightweight, and Cross-Platform Object-Relational Mapper (ORM)** for .NET applications.

When combined with **ASP.NET Core Web API** and **SQL Server**, EF Core provides a clean, scalable, and testable data access layer, essential for building real-time production APIs.

## Why Use EF Core in ASP.NET Core Web API?

* **Productivity Boost**: Automatically maps our C# classes (entities) to database tables. It eliminates repetitive SQL queries for common operations like insert, update, or delete.
* **Cross-Platform & Lightweight**: Runs on Windows, Linux, and macOS. Supports multiple databases like SQL Server, PostgreSQL, MySQL, SQLite, and more.
* **Code First Development**: Define database structure directly from our C# model classes. EF Core generates and maintains the database schema using **migrations**.
* **LINQ Support**: We can query our data using C# LINQ expressions instead of raw SQL, safer and more maintainable.
* **Change Tracking & Transactions**: EF Core automatically tracks entity states (Added, Modified, Deleted) and also handles database transactions efficiently.
* **Unit testing friendly**: Replace the real provider with in-memory or SQLite for tests; mock repositories/services around DbContext.
* **Performance**: Modern change tracker, compiled models, AsNoTracking, batch ops, and provider-specific optimizations.

### Which EF Core Packages to Install?

* `Microsoft.EntityFrameworkCore` → The core package that provides EF Core functionalities such as LINQ, Change Tracking, and Model Configuration.
* `Microsoft.EntityFrameworkCore.SqlServer` → The Database Provider for SQL Server, adds SQL Server-specific features and optimizations.
* `Microsoft.EntityFrameworkCore.Tools` → Provides PowerShell commands for database Migrations, Scaffolding, and Database Updates.

### Commonly Used EF Core Database Providers

The following are some of the commonly used database providers in Entity Framework Core. Each of the following packages must be installed separately, depending on your database system.

* `Microsoft.EntityFrameworkCore.SqlServer` → for SQL Server
* `Pomelo.EntityFrameworkCore.MySql` → for MySQL
* `Npgsql.EntityFrameworkCore.PostgreSQL` → for PostgreSQL
* `Microsoft.EntityFrameworkCore.Sqlite` → for SQLite
* `Oracle.EntityFrameworkCore` → for Oracle

So, the Database Provider Package is the essential layer that gives EF Core its ability to communicate with any relational database. Without it, EF Core cannot function because it wouldn’t know:

* What SQL syntax to use,
* How to translate LINQ,
* How to handle schema creation or migrations.

### What is a Database Connection String in Entity Framework Core? 
In Entity Framework Core (EF Core), a **Database Connection String** is a **Configuration String** that contains all the details required for your application to establish a connection with a database server. Think of it as the **address**, **credentials**, and instructions EF Core needs to locate and talk to your database.

When your DbContext runs (for example, while fetching data or saving changes), EF Core uses this connection string to:

* Know **Which Server** to connect to (e.g., SQL Server on your local machine or cloud),
* Know **Which Database** to use (e.g., ProductDB),
* Know How to **authenticate** (Windows Authentication or SQL Authentication),
* And manage other connection-level settings (like pooling, timeouts, etc.).

Without it, EF Core doesn’t know where to go. It’s a small string but carries powerful information, for example: `“Server=.; Database=ProductDB; Trusted_Connection=True; MultipleActiveResultSets=True;“`

Let’s break it down briefly:

1. `Server=.;` → The dot (.) means the local SQL Server instance.
2. `Database=ProductDB;` → Name of the database to connect to.
3. `Trusted_Connection=True;` → Use Windows Authentication instead of username/password.
4. `MultipleActiveResultSets=True;` → Allows multiple queries to run on one connection (used by EF Core internally).

So, in EF Core, when we call something like **options.UseSqlServer(connectionString)** inside your Program.cs, this connection string is what EF Core uses to communicate with SQL Server.

### Storing Database Connection String in `appsettings.json` File:

Let us proceed and store the connection string inside `appsettings.json` file. The appsettings.json file in ASP.NET Core acts like a **Central Configuration File** for our application. It’s similar to web.config in older .NET Framework apps, but it’s **JSON-based**, easier to manage, and supports hierarchical configurations.

Now, within the appsetings.json file, we need to add a section for the configuration string. So, please modify the appsetings.json file as follows:

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
    "DefaultConnection": "Server=LAPTOP-6P5NK25R\\SQLSERVER2022DEV;Database=ProductDB;Trusted_Connection=True;TrustServerCertificate=True;"
  }
}
```

**Understanding the Hierarchy**
* **“ConnectionStrings”** → A logical grouping of all connection strings (you can have multiple).
* **“DefaultConnection”** → The name/key of this connection string (you can name it whatever you like).
* The actual string value → “**Server=LAPTOP-6P5NK25R\\SQLSERVER2022DEV; Database=ProductDB; Trusted_Connection=True; TrustServerCertificate=True;**“.

#### How to Configure Database Connection String and DbContext?
Once the connection string is defined, we must tell EF Core how to use it. This is done by registering the DbContext and configuring it to use SQL Server inside the Program class. Let’s understand this step in depth.

In the modern ASP.NET Core Minimal Hosting Model (from .NET 6 onwards), everything is configured inside Program.cs. We no longer have Startup.cs, so the setup happens here. So, please modify the Program class as follows:

```C#
using Microsoft.EntityFrameworkCore;
using ProductManagementAPI.Data;

namespace ProductManagementAPI
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var builder = WebApplication.CreateBuilder(args);

            // Add services to the container.
            builder.Services.AddControllers()
            .AddJsonOptions(options =>
            {
                // This will use the property names as defined in the C# model
                options.JsonSerializerOptions.PropertyNamingPolicy = null;
            });

            builder.Services.AddEndpointsApiExplorer();
            builder.Services.AddSwaggerGen();

            builder.Services.AddDbContext<AppDbContext>(options =>
            options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

            var app = builder.Build();

            // Configure the HTTP request pipeline.
            if (app.Environment.IsDevelopment())
            {
                app.UseSwagger();
                app.UseSwaggerUI();
            }

            app.UseHttpsRedirection();

            app.UseAuthorization();

            app.MapControllers();

            app.Run();
        }
    }
}
```
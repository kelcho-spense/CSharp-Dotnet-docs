# IQueryable Workshop

![image_205.png](image_205.png)

In this workshop, we will walk through the steps to create a Console Application that retrieves data from an SQL Server database using the Entity Framework Database First approach. We will focus on retrieving information from a Student table, as shown in the provided images and SQL script.

## Database Creation
![image_206.png](image_206.png)

Create a new Console application call it IQueryable Workshop. Once you create the Console Application, add the ADO.NET Entity Data Model using the Database First Approach pointing to the above database.

Click on tools and select Connect to database
![image_208.png](image_208.png)

Select Microsoft SQL Server
![image_207.png](image_207.png)

Add 
- Server name: localhost
- User name: SA
- Password : <Yourpassword>

![image_209.png](image_209.png)

After successfully connecting, you should see this.

![image_210.png](image_210.png)

Right-click on data Connections and create a new SQL Server database.

![image_211.png](image_211.png)

Right-click on your database and click on `New Query`, then paste the sql code below 

```sql
-- Create the required Student table
CREATE TABLE Student
(
     ID INT PRIMARY KEY,
     FirstName VARCHAR(50),
     LastName VARCHAR(50),
     Gender VARCHAR(50)
)
GO

-- Insert the required test data
INSERT INTO Student VALUES (101, 'Steve', 'Smith', 'Male')
INSERT INTO Student VALUES (102, 'Sara', 'Pound', 'Female')
INSERT INTO Student VALUES (103, 'Ben', 'Stokes', 'Male')
INSERT INTO Student VALUES (104, 'Jos', 'Butler', 'Male')
INSERT INTO Student VALUES (105, 'Pam', 'Semi', 'Female')
GO
```
Then run the SQL script 

![image_212.png](image_212.png)

Expand on tables to see your newly created table

![image_213.png](image_213.png)

## Create a New Console Application
- [DONE] Open Visual Studio and create a new Console Application project. Name it `IQueryableWorkshop`.
- After the project is created, you’ll set up the Entity Framework to interact with your SQL Server database.

### Add Entity Data Model

- For EF Core (.NET 6/7/… Console App):

  - Install NuGet packages:

    - `Microsoft.EntityFrameworkCore.SqlServer`
    - `Microsoft.EntityFrameworkCore.Tools`
      These enable EF Core to work with SQL Server and design‑time tooling.
![image_214.png](image_214.png)

### Define your entity class(es)
For example, you have a Student table with columns `ID, FirstName, LastName, Gender`. You would create:

```C#
using System;
using System.Collections.Generic;
using System.Text;

namespace IQueryable_Workshop
{
    internal class Student
    {
        public int ID { get; set; }
        public string FirstName { get; set; } = null!;
        public string LastName { get; set; } = null!;
        public string Gender { get; set; } = null!;
    }
}

```

### Create the DbContext class
This is the class through which you query and save data. Example:

```C#
using Microsoft.EntityFrameworkCore;
using System;
using System.Collections.Generic;
using System.Text;

namespace IQueryable_Workshop
{
    internal class AppDbContext: DbContext
    {
        public DbSet<Student> Students { get; set; } = null!;
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            // Provide your connection string here
            optionsBuilder.UseSqlServer(
                @"Server=localhost;Database=YourDatabaseName;User Id=SA;Password=pass;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // Fluent API configurations (optional/customizations)
            modelBuilder.Entity<Student>(entity =>
            {
                entity.HasKey(e => e.ID);
                entity.Property(e => e.FirstName).HasMaxLength(50).IsRequired();
                entity.Property(e => e.LastName).HasMaxLength(50).IsRequired();
                entity.Property(e => e.Gender).HasMaxLength(50).IsRequired();
                entity.ToTable("Student");
            });

            // Similarly you can set up configuration for  other entities
        }

    }
}
```

**Explanation:**

* `OnConfiguring` sets up the DB provider (SQL Server) and the connection string.
* `OnModelCreating` uses the Fluent API to configure mappings, constraints, table names etc.
* `DbSet<Student>` allows you to query the `Student` table.

### Use the context in your console app
In `Program.cs` :

```C#
using IQueryable_Workshop;

using (AppDbContext context = new AppDbContext())
{
    // Example 1: Filtering and limiting results
    IQueryable<Student> topMaleStudents = context.Students
                                        .Where(x => x.Gender == "Male")
                                        .Take(2);
    Console.WriteLine("Top 2 Students Where Gender = Male");
    Console.WriteLine("=====================================");
    foreach (var s in topMaleStudents)
    {
        Console.WriteLine($"ID: {s.ID}, Name: {s.FirstName} {s.LastName}");
    }

    // Example 2: Sorting with multiple criteria
    IQueryable<Student> sortedStudents = context.Students
                                        .OrderByDescending(x => x.FirstName)
                                        .ThenBy(x => x.LastName)
                                        .Take(5);
    Console.WriteLine("\nTop 5 Students Sorted by FirstName Descending and LastName Ascending");
    Console.WriteLine("======================================================================");
    foreach (var s in sortedStudents)
    {
        Console.WriteLine($"ID: {s.ID}, Name: {s.FirstName} {s.LastName}");
    }

    // Example 3: Count aggregation
    int totalStudents = context.Students.Count();
    Console.WriteLine($"\nTotal Number of Students: {totalStudents}");

    // Example 4: Count with filtering
    int maleCount = context.Students.Count(s => s.Gender == "Male");
    int femaleCount = context.Students.Count(s => s.Gender == "Female");
    Console.WriteLine($"Male Students: {maleCount}");
    Console.WriteLine($"Female Students: {femaleCount}");

    // Example 5: Group by with count
    var genderGroups = context.Students
                            .GroupBy(s => s.Gender)
                            .Select(g => new 
                            { 
                                Gender = g.Key, 
                                Count = g.Count() 
                            })
                            .ToList();
    Console.WriteLine("\nStudents Grouped by Gender:");
    Console.WriteLine("============================");
    foreach (var group in genderGroups)
    {
        Console.WriteLine($"Gender: {group.Gender}, Count: {group.Count}");
    }

    // Example 6: Max and Min operations
    var maxId = context.Students.Max(s => s.ID);
    var minId = context.Students.Min(s => s.ID);
    Console.WriteLine($"\nHighest Student ID: {maxId}");
    Console.WriteLine($"Lowest Student ID: {minId}");

    // Example 7: First and FirstOrDefault
    var firstStudent = context.Students.FirstOrDefault();
    if (firstStudent != null)
    {
        Console.WriteLine($"\nFirst Student: {firstStudent.FirstName} {firstStudent.LastName}");
    }

    // Example 8: Any and All operators
    bool hasMaleStudents = context.Students.Any(s => s.Gender == "Male");
    bool allHaveFirstName = context.Students.All(s => !string.IsNullOrEmpty(s.FirstName));
    Console.WriteLine($"\nHas Male Students: {hasMaleStudents}");
    Console.WriteLine($"All Students Have First Name: {allHaveFirstName}");

    // Example 9: Complex query with multiple operations
    var complexQuery = context.Students
                            .Where(s => s.Gender == "Male")
                            .OrderBy(s => s.LastName)
                            .ThenBy(s => s.FirstName)
                            .Skip(1)
                            .Take(2)
                            .Select(s => new 
                            { 
                                FullName = $"{s.FirstName} {s.LastName}",
                                s.Gender 
                            })
                            .ToList();
    Console.WriteLine("\nComplex Query (Skip first male student, take next 2):");
    Console.WriteLine("======================================================");
    foreach (var item in complexQuery)
    {
        Console.WriteLine($"Name: {item.FullName}, Gender: {item.Gender}");
    }

    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
}

```


### What is `IQueryable<Student>`

* The type `IQueryable<Student>` represents a query that hasn’t yet been executed; it builds an expression tree that the EF Core provider will translate into SQL and send to the database. 
* Use of `IQueryable` (instead of something like `IEnumerable`) means that filtering, sorting, limiting, etc., will be executed in the database rather than in memory — which is more efficient.

---

### Walking through the above Code

1. **Filtering + Limiting results**

   ```C#
   IQueryable<Student> topMaleStudents = context.Students
       .Where(x => x.Gender == "Male")
       .Take(2);
   ```

    * `Where(x => x.Gender == "Male")`: builds a condition to only include students whose `Gender` is “Male”.
    * `Take(2)`: limits the result to the first two of those matched rows.
    * Because this is `IQueryable`, the WHERE + TOP(2) (or equivalent) will be included in the SQL sent to the database — only two male student rows will be fetched.

2. **Sorting with multiple criteria**

   ```C#
   IQueryable<Student> sortedStudents = context.Students
       .OrderByDescending(x => x.FirstName)
       .ThenBy(x => x.LastName)
       .Take(5);
   ```

    * `OrderByDescending(x => x.FirstName)`: sort students so those with later (alphabetically) first names come first.
    * `.ThenBy(x => x.LastName)`: for students with the same first name, sort by last name (ascending).
    * `.Take(5)`: pick only the top 5 after sorting.
    * This entire sorting + limiting is executed in the database side.

3. **Count aggregation**

   ```C#
   int totalStudents = context.Students.Count();
   ```

    * `Count()` is a terminal method (on IQueryable) which causes the query to execute and only retrieves the count (not the full entities).
    * Efficient: the database counts rows and returns just the number.

4. **Count with filtering**

   ```C#
   int maleCount = context.Students.Count(s => s.Gender == "Male");
   int femaleCount = context.Students.Count(s => s.Gender == "Female");
   ```

    * This uses overload of `Count(predicate)` to count only those rows matching the predicate.
    * Recognized by EF Core and translated into something like `SELECT COUNT(*) FROM Students WHERE Gender = 'Male'`.

5. **Group by with count**

   ```C#
   var genderGroups = context.Students
       .GroupBy(s => s.Gender)
       .Select(g => new { Gender = g.Key, Count = g.Count() })
       .ToList();
   ```

    * `GroupBy(s => s.Gender)`: group students by their gender value.
    * The `Select(...)` then projects each group into an anonymous object with the gender key and the number of students in that group (`g.Count()`).
    * `.ToList()` triggers execution — fetching the grouped results from the database.

6. **Max and Min operations**

   ```C#
   var maxId = context.Students.Max(s => s.ID);
   var minId = context.Students.Min(s => s.ID);
   ```

    * `Max` & `Min` are also terminal methods. They ask the database to compute the maximum / minimum `ID` value, rather than retrieving all students then computing in memory.

7. **First and FirstOrDefault**

   ```C#
   var firstStudent = context.Students.FirstOrDefault();
   ```

    * `FirstOrDefault()` will return the first entity from the result (or `null` if none). Because it’s `IQueryable`, the translation ensures only one row is retrieved from DB.

8. **Any and All operators**

   ```C#
   bool hasMaleStudents = context.Students.Any(s => s.Gender == "Male");
   bool allHaveFirstName = context.Students.All(s => !string.IsNullOrEmpty(s.FirstName));
   ```

    * `Any(predicate)`: returns true if **any** row matches the condition (efficient — stops once found in DB).
    * `All(predicate)`: returns true if **all** rows satisfy the condition.
    * Both are translated to optimized SQL.

9. **Complex query with multiple operations**

   ```C#
   var complexQuery = context.Students
       .Where(s => s.Gender == "Male")
       .OrderBy(s => s.LastName)
       .ThenBy(s => s.FirstName)
       .Skip(1)
       .Take(2)
       .Select(s => new { FullName = $"{s.FirstName} {s.LastName}", s.Gender })
       .ToList();
   ```

    * Step by step:

        * Filter male students.
        * Sort by `LastName`, then by `FirstName`.
        * `Skip(1)`: skip the first result after sorting.
        * `Take(2)`: then take the next two.
        * `Select(...)`: project into an anonymous type with `FullName` and `Gender`.
        * `ToList()`: triggers execution.
    * All of this is combined into a single SQL query that does filtering, ordering, skipping, limiting and projection on the database side — minimizing data transferred.

---

### Key Points to Keep in Mind

* **Deferred execution**: The query isn’t executed when you declare it (e.g., `var query = context.Students.Where(...)`), but when you iterate it (`foreach`) or call a materializing operation (`ToList()`, `Count()`, etc.). ([DEV Community][3])
* **Query composition**: Because the query remains an `IQueryable`, you can keep chaining filters/sorts etc, and EF will build a single query expression tree and then translate efficiently.
* **Pushing work to the database**: Using `IQueryable` ensures that operations like filtering, sorting, grouping happen in the database rather than fetching everything into memory and doing them in the app — which is better for performance with large datasets. ([Stack Overflow][4])
* **Materializing vs non‑materializing methods**: Methods like `Where`, `OrderBy`, `Take`, `Skip`, `GroupBy`, `Select` build up the query. Methods like `ToList`, `Count`, `FirstOrDefault`, `Any`, etc. execute the query.
* **Be aware of what queries are generated**: Because you’re using `IQueryable`, the LINQ provider (EF Core) will translate your expression tree into SQL — not all .NET methods are translatable, and adding extra operations after materialization can degrade performance.




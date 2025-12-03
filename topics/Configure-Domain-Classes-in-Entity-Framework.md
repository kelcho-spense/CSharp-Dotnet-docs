# Configure Domain Classes in Entity Framework

We have already discussed the default Entity Framework Code-First Conventions in the previous article. The EF Code-First builds the conceptual model from your domain classes using the default conventions.
The Entity Framework Code-First follows a programming pattern referred to as Convention Over Configuration. However, it is also possible to override these default conventions and, in this case, we need to provide the required configuration information to the Entity Framework API. And, you can configure your domain classes or you can provide the configuration information to the Entity Framework in two different ways. They are as follows:

* Data Annotation Attributes
* Fluent API

## Data Annotations Attributes in Entity Framework Code First Approach:

`Data Annotations are nothing but Attribute Based Configurations`, which we can apply to our domain classes and their properties. The point that you need to remember is that these attributes are not only for Entity Framework but also used in ASP.NET MVC or ASP.NET Web API, etc. In .NET Framework, the **Data Annotations Attributes** are included in separate namespaces called `System.ComponentModel.DataAnnotations` and `System.ComponentModel.DataAnnotations.Schema`.

```C#
using System;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace EFCodeFirstDemo
{
    [Table("StudentInfo")]
    public class Student
    {
        [Key]
        public int StudentId { get; set; }

        [Column("FName", TypeName = "ntext")]
        [MaxLength(50)]
        public string FirstName { get; set; }

        [Column("LName", TypeName = "ntext")]
        [MaxLength(50)]
        public string LastName { get; set; }

        [Column("DOB", TypeName = "DateTime2")]
        public DateTime DateOfBirth { get; set; }

        [NotMapped]
        public int Age { get; set; }

        [Required]
        public string Branch { get; set; }

        [Index]
        public int RegistrationNumber { get; set; }

        public int StandardId { get; set; }

        [ForeignKey("StandardId")]
        public virtual Standard Standard { get; set; }
    }
}
```

> The Problem with Data Annotation Attribute is that it does not support all the configuration options that are required for Entity Framework Code First Approach. So, in that case, we can go for **Fluent API**, which provides all the configuration options required for Entity Framework.

## Fluent API in Entity Framework Code First Approach:

Another approach to configuring the domain classes in Entity Framework Code First Approach is by using Fluent API. Entity Framework Fluent API is based on a Fluent API design pattern (Fluent Interface) where the result is formulated by method chaining. So, before understanding Entity Framework Fluent API, _first we need to understand Fluent Interface Design Pattern_.

> The Fluent Interfaces and Method chaining are related to each other. Or we can say that one is a concept and the other one is its implementation.

### What is the Fluent Interface Design Pattern?

The main objective of the Fluent Interface Design Pattern is that we can apply multiple properties (or methods) to an object by connecting them with dots (.) without having to re-specify the object name each time. Something like the following.

![image_233.png](image_233.png)

### What is Method Chaining?

Method Chaining is a common technique where each method returns an object and all these methods can be chained together to form a single statement. For a better understanding, please create a new console application and copy and paste the following code.

```C#
using System;
namespace FluentInterfaceDesignPattern
{
    class Program
    {
        static void Main(string[] args)
        {
            FluentEmployee obj = new FluentEmployee();

            obj.NameOfTheEmployee("Anurag Mohanty")
                    .Born("10/10/1992")
                    .WorkingOn("IT")
                    .StaysAt("Mumbai-India");

            Console.Read();
        }
    }

    public class Employee
    {
        public string FullName { get; set; }
        public DateTime DateOfBirth { get; set; }
        public string Department { get; set; }
        public string Address { get; set; }
    }

    public class FluentEmployee
    {
        private Employee employee = new Employee();

        public FluentEmployee NameOfTheEmployee(string FullName)
        {
            employee.FullName = FullName;
            return this;
        }

        public FluentEmployee Born(string DateOfBirth)
        {
            employee.DateOfBirth = Convert.ToDateTime(DateOfBirth);
            return this;
        }

        public FluentEmployee WorkingOn(string Department)
        {
            employee.Department = Department;
            return this;
        }

        public FluentEmployee StaysAt(string Address)
        {
            employee.Address = Address;
            return this;
        }
    }
}
```

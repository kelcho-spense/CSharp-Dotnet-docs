# IEnumerable and IQueryable in C#

Let us understand these two interfaces with examples. Please look at the following program we wrote using the LINQ Query Syntax in our previous article.

![image_199.png](image_199.png)

In the above example, we use the **var** keyword to create the variable and store the result of the **LINQ query**. So, let’s check what the type of variable is. To check this, _**just mouse over the pointer onto the QuerySynntax variable**_, and you will see that the type is **IEnumerable<int>**, which is a generic type. So, it is important to understand what is IEnumerable<T>.

So, in the above example, instead of writing the var keyword, you can also write **`IEnumerable<int>`**, and it should work as expected, as shown in the below example.

```C#
using System;
using System.Collections.Generic;
using System.Linq;

namespace LINQDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            List<int> integerList = new List<int>()
            {
                1, 2, 3, 4, 5, 6, 7, 8, 9, 10
            };

            IEnumerable<int> QuerySyntax = from obj in integerList
                              where obj > 5
                              select obj;
            
            foreach (var item in QuerySyntax)
            {
                Console.Write(item + " ");
            }

            Console.ReadKey();
        }
    }
}
```
With this kept in mind, let us understand what IEnumerable is.

## What is IEnumerable in C#?
IEnumerable in C# is an interface that defines one method, GetEnumerator, which returns an IEnumerator object. This interface is found in the **System.Collections namespace**. It is a key part of the .NET Framework and is used to iterate over a collection of objects. The following is the definition of the IEnumerator interface.

![image_200.png](image_200.png)

### Example to understand IEnumerable with Complex Type using C#
Whenever we want to work with in-memory objects, we need to use the IEnumerabe interface, and the reason for this will be discussed in our next article.

```C#
using System;
using System.Collections.Generic;
using System.Linq;

namespace LINQDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            List<Student> studentList = new List<Student>()
            {
                new Student(){ID = 1, Name = "James", Gender = "Male"},
                new Student(){ID = 2, Name = "Sara", Gender = "Female"},
                new Student(){ID = 3, Name = "Steve", Gender = "Male"},
                new Student(){ID = 4, Name = "Pam", Gender = "Female"}
            };

            // Query Syntax: Fetch all students with Gender Male
            //IEnumerable<Student> QuerySyntax = from std in studentList
            //                                   where std.Gender == "Male"
            //                                   select std;

            //// Iterate through the collection returned by query syntax
            //foreach (var student in QuerySyntax)
            //{
            //    Console.WriteLine($"ID : {student.ID}  Name : {student.Name}");
            //}

            // Method (Fluent) Syntax Equivalent (commented demonstration):
            IEnumerable<Student> MethodSyntax = studentList.Where(std => std.Gender == "Male");
            foreach (var student in MethodSyntax)
            {
                Console.WriteLine($"ID : {student.ID}  Name : {student.Name}");
            }


            Console.ReadKey();
        }
    }

    public class Student
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public string Gender { get; set; }
    }
}
```
When we execute the program, the result will be displayed as expected, as shown in the image below.

![image_201.png](image_201.png)

## What is IQueryable in C#?

IQueryable in C# is an interface that is used to query data from a data source. It is part of the System.Linq namespace and is a key component in LINQ (Language Integrated Query)
Unlike IEnumerable, _which is used for iterating over in-memory collections_, `IQueryable is designed for querying data sources where the query is not executed until the object is enumerated`. **`This is particularly useful for remote data sources, like databases`**, enabling efficient querying by allowing the query to be executed on the server side. The following is the definition of the IQueryable interface.

![image_202.png](image_202.png)

### Example to Understand IQueryable<T> Interface in C#.

```C#
using System;
using System.Collections.Generic;
using System.Linq;

namespace LINQDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            List<Student> studentList = new List<Student>()
            {
                new Student(){ID = 1, Name = "James", Gender = "Male"},
                new Student(){ID = 2, Name = "Sara", Gender = "Female"},
                new Student(){ID = 3, Name = "Steve", Gender = "Male"},
                new Student(){ID = 4, Name = "Pam", Gender = "Female"}
            };
            
            //Linq Query to Fetch all students with Gender Male
            IQueryable<Student> MethodSyntax = studentList.AsQueryable()
                                .Where(std => std.Gender == "Male");
                                              
            //Iterate through the collection
            foreach (var student in MethodSyntax)
            {
                Console.WriteLine( $"ID : {student.ID}  Name : {student.Name}");
            }

            Console.ReadKey();
        }
    }

    public class Student
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public string Gender { get; set; }
    }
}
```

Now run the application, and you will see the data as expected, as shown in the below image.

![image_203.png](image_203.png)

> The point you need to remember is to return a collection of IQueryable type and call the **AsQueryable()** method on the data source, as we did in the above example.


## Differences Between IEnumerable and IQueryable in C#
The IEnumerable and IQueryable in C# are used to hold a collection of data and also to perform data manipulation operations such as filtering, ordering, grouping, etc., based on the business requirements. This article will show you the difference between IEnumerable and IQueryable in C# with Examples. For a better understanding, please have a look at the following image. As you can see, IEnumerable<T> fetches the record from the database without applying the filter. But IQueryable<T> fetches the record from the database by applying the filter. 

![image_204.png](image_204.png)

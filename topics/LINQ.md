# LINQ

Most queries in the introductory **`Language Integrated Query`** (LINQ) documentation are written by using the LINQ declarative query syntax. The C# compiler translates query syntax into method calls. These method calls implement the standard query operators, and have names such as `Where`, `Select`, `GroupBy`, `Join`, `Max`, and `Average`. You can call them directly by using method syntax instead of query syntax.

![image_192.png](image_192.png)

Microsoft introduced LINQ (Language Integrated Query) with .NET Framework 3.5 and C# 3.0, available in the System.Linq namespace.

## Why Should We Learn LINQ?
Suppose we are developing a .NET Application that requires data from different sources. For example

1. The application needs data from the SQL Server Database. So, as a developer, to access the data from the SQL Server Database, _**we need to understand ADO.NET and SQL Server-specific syntaxes. We need to learn SQL Syntax specific to Oracle Database if the database is Oracle**_.
2. The application also needs data from an XML Document. So, as a developer, to work with XML documents, we need to understand XPath and XSLT queries.
3. The application also needs to manipulate the data (objects) in memory, such as `List<Products>`, `List<Orders>`, etc. So, as a developer, we should also understand how to work with in-memory objects.

![image_193.png](image_193.png)

> LINQ provides a Uniform Programming Model (i.e., Common Query Syntax), which allows us to work with different data sources such as databases, XML Documents, in-memory objects, etc., but using a standard or, you can say, unified coding style. As a result, we are not required to learn different syntaxes to query different data sources.

## How Does LINQ Work?

![image_194.png](image_194.png)

As shown in the above diagram, you can write the LINQ queries using any DOT NET Supported Programming Language such as C#, VB.NET, J#, F#

### Different Ways to Write LINQ Queries in C#

LINQ queries can be written in two ways:

1. **Query Syntax**: It is similar to SQL and is often more readable for those familiar with SQL. It starts with a from clause followed by a range variable and includes standard query operations like where, select, group, join, etc.

Each query is a combination of three things. They are as follows:

* **Initialization** (to work with a particular data source)
* **Condition** (where, filter, sorting condition)
* **Selection** (single selection, group selection, or joining)

![image_195.png](image_195.png)

**Characteristics:**
* Resembles SQL-like declarative style.
* It can be more readable, especially for those familiar with SQL.
* Not all operations can be expressed in Query Syntax; some require a switch to Method Syntax.

```C#
var query = from c in customers
            where c.City == "London"
            select c.Name;
```
2. **Method Syntax (Fluent/Lambda Syntax)**: It uses extension methods and lambda expressions. It can be more concise and is preferred when writing complex queries because it can be easier to read and compose.

![image_196.png](image_196.png)

```C#
var query = customers.Where(c => c.City == "London").Select(c => c.Name);
```

**Characteristics:**
* Utilizes lambda expressions.
* It can be more concise for complex queries.
* Offers slightly more methods and flexibility than Query Syntax.
* It can be easier to understand for those familiar with lambda expressions and functional programming.

## Example Using LINQ Query Syntax in C#:
The following Example code is self-explained, so please go through the comment lines. Here, we have created a collection of integers, i.e., going to be our data source

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
            //Step1: Data Source
            List<int> integerList = new List<int>()
            {
                1, 2, 3, 4, 5, 6, 7, 8, 9, 10
            };

            //Step2: Query
            //LINQ Query using Query Syntax to fetch all numbers which are > 5
            var QuerySyntax = from obj in integerList //Data Source
                              where obj > 5 //Condition
                              select obj; //Selection

            //Step3: Execution
            foreach (var item in QuerySyntax)
            {
                Console.Write(item + " ");
            }

            Console.ReadKey();
        }
    }
}
```
Now run the application, and it will display the values `6 7 8 9 10` as expected in the console window. Let us understand what we did in the above code.

![image_197.png](image_197.png)

## Example Using LINQ Method Syntax(Fluent/Lambda) in C#:
Let us rewrite the previous example using the LINQ Method Syntax. The following Example code is self-explained, so please go through the comment lines.

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
            //Step1: Data Source
            List<int> integerList = new List<int>()
            {
                1, 2, 3, 4, 5, 6, 7, 8, 9, 10
            };

            //Step2: Query
            //LINQ Query using Query Syntax to fetch all numbers which are > 5
            var QuerySyntax = integerList.Where(obj => obj > 5).ToList(); 

            //Step3: Execution
            foreach (var item in QuerySyntax)
            {
                Console.Write(item + " ");
            }

            Console.ReadKey();
        }
    }
}
```
Now, run the application, and you will get the output as expected. Let us have a look at the following diagram to understand Method Syntax.

![image_198.png](image_198.png)

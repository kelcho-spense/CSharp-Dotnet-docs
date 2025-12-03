# Index Attribute in Entity Framework

Entity Framework 6 provides the [Index] attribute to create an index on a particular column in the database. If you are using an earlier version of Entity Framework, then the Index Attribute will not work. It is also possible to create an index on one or more columns using the Index Attribute. Adding the Index Attribute to one or more properties of an Entity will cause Entity Framework to create the corresponding index in the database when it creates the database.

Before creating an Index using Entity Framework Code First Approach, let us first understand some basic concepts of an Index like what is Index, why we need an Index, what different types of Indexes and how the index impact DML Operations.

## What is an Index and why Index in a Database?

Indexes in SQL Server are nothing but database object in a database which is used to improve the performance of search operations. When we create an index on any column or columns of a table, the SQL Server Database internally maintains a separate table called the Index Table. And when we are trying to retrieve the data from the original table, depending on the index table, SQL Server directly goes to the original table and retrieves the data very quickly.

### When SQL Server uses Indexes?

The SQL Server Database uses indexes of a table provided that the select or update or delete statement contained the **“WHERE”** clause and moreover the where clause condition column must be an indexed column. If the select statement contains an **“ORDER BY”** clause then also the indexes can be used.

### Types of indexes in SQL Server
SQL Server Indexes are divided into two types. They are as follows:

1. `Clustered Index`: The Clustered Index in SQL Server defines the order in which the data is physically stored in a table.
2. `Non-Clustered Index`: In SQL Server Non-Clustered Index, the arrangement of data in the index table will be different from the arrangement of data in the actual table.

> When we create an Index by using the Unique option then it is called Unique Index. Then the column(s) on which the unique index is created will not allow duplicate values

### How does Index Affect the DML Operations?

The most important point that you need to keep in mind is that indexes make the retrieval of data faster and more efficient, in most cases. However, creating a lot of Indexes in a table or view could `affect the performance of other operations such as inserts, delete, or updates`.

Let us modify the Student Entity Class as follows to use the Index Attribute. Here, you can see, we have applied the Index Attribute on the RegistrationNumber property. 

```C#
using System.ComponentModel.DataAnnotations.Schema;
namespace EFCodeFirstDemo
{
    public class Student
    {
        public int StudentId { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }

        [Index]
        public int RegistrationNumber { get; set; }
    }
}
```

Now, if you want to give a different name to your Index name rather than the auto-generated index name, then you need to use the other overloaded version of the Constructor which takes the name parameter.

```C#
using System.ComponentModel.DataAnnotations.Schema;
namespace EFCodeFirstDemo
{
    public class Student
    {
        public int StudentId { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }

        [Index("Index_RegistrationNumber")]
        public int RegistrationNumber { get; set; }
    }
}
```

### Creating Index on Multiple Columns using Entity Framework Code First Approach

It is also possible to create an Index on Multiple Columns. For this, we need to use the overloaded version which takes the index name and order parameter. In this case, we need to specify the same name for both the Index Attribute. Let us understand this with an example. Please modify the Student class as follows. As you can see, here, we have applied the Index Attribute with the same name with RegistrationNumber and RollNumber properties and with the order values 2 and 1 respectively. In this case, the Entity Framework API will create one index based on the RegistrationNumber and RollNumber columns.

```C#
using System.ComponentModel.DataAnnotations.Schema;
namespace EFCodeFirstDemo
{
    public class Student
    {
        public int StudentId { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }

        [Index("Index_Roll_Registration", 2)]
        public int RegistrationNumber { get; set; }

        [Index("Index_Roll_Registration", 1)]
        public int RollNumber { get; set; }
    }
}
```

### How to Create Clustered and Unique Index using Entity Framework?
By default, _Entity Framework API creates Non-Clustered and Non-Unique Index_. If you want to create a Clustered and Unique Index, then you need to use the following two properties and you need to set the values to True.

* `IsClustered`: Set this property to true to define a clustered index. Set this property to false to define a non-clustered index.
* `IsUnique`: Set this property to true to define a unique index. Set this property to false to define a non-unique index.

Now, modify the Student class as shown below to create a clustered and unique index on the RegistrationNumber property.

```C#
using System.ComponentModel.DataAnnotations.Schema;
namespace EFCodeFirstDemo
{
    public class Student
    {
        public int StudentId { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }

        [Index("Index_RegistrationNumber", IsClustered =true, IsUnique =true)]
        public int RegistrationNumber { get; set; }
    }
}
```
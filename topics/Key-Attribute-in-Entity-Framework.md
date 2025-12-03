# Key Attribute in Entity Framework

Before understanding Key Attributes, let us first understand what is a Primary Key in a database.

## What is Primary Key in the Database?

The Primary Key is the combination of Unique and Not Null Constraints. That means it will not allow either NULL or Duplicate values into a column or columns on which the primary key constraint is applied. Using the primary key, we can enforce entity integrity i.e. using the primary key value we should uniquely identify a record.

> A table should contain only 1 Primary Key which can be either on single or multiple columns i.e. the composite primary key. A table should have a primary key to uniquely identify each record. 

### Key Attribute in Entity Framework

As per the default convention, the Entity Framework will create the primary key column for the property whose name is Id or <Entity Class Name> + “Id” (case insensitive). For example, let us modify the Student entity class as follows. Here, we have created the StudentId property in the Student class. So, this property will be created as a Primary Key column in the corresponding database table.

```C#
using System;
using System.ComponentModel.DataAnnotations.Schema;
namespace EFCodeFirstDemo
{
    public class Student
    {
        public int StudentId { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }
}
```

Now, we do not want to make StudentId as the Primary key column, instead, we want a different column let us say StudentRegdNo as the primary key.

So, modify the Student class as follows. As you can see in the below code, we just decorate the StudentRegdNo property with the Key Attribute.

```C#
using System.ComponentModel.DataAnnotations;
namespace EFCodeFirstDemo
{
    public class Student
    {
        [Key]
        public int StudentRegdNo { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }
}
```

With the above changes in place, now run the application code again, and this time you will not get any exceptions and the database should be updated as expected. You can also verify the database as shown in the below image.

![image_238.png](image_238.png)


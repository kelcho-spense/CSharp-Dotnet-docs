# Column Data Annotation Attribute in Entity Framework

The Column Data Annotation Attribute can be applied to one or more properties of a domain entity to configure the corresponding database column name, column order, and column data type. That means it represents the database column that a property is mapped to.

Now, if you go to the definition of Column Attribute, then you will see the following.

![image_235.png](image_235.png)

As you can see in the above image, this class has two constructors. The 0-Argument constructor will create the database column with the same name as the property name while the other overloaded constructor which takes the string name as a parameter will create the database table column with the passed name. Again, this Column attribute has three properties. The meaning of the properties are as follows:

* Name: Gets the name of the column the property is mapped to. This is a read-only property.
* Order: Gets or sets the zero-based order of the column the property is mapped to. It returns the order of the column. This is a read-write property.
* TypeName: Gets or sets the database provider-specific data type of the column the property is mapped to. It returns the database provider-specific data type of the column the property is mapped to. This is a read-write property.

Syntax to use Column Attribute: `[Column (string name, Properties: [Order = int], [TypeName = string])]`
Example to use Column Attribute: `[Column(“DOB”, Order = 2, TypeName = “DateTime2”)]`

### Examples to understand Column Data Annotation Attribute in Entity Framework:
With the FirstName property, we have not provided the string name i.e. using the 0-Argument constructor, so in this case, it will create the database table column with the same name as the Property name. With the LastName property, we have specified the name as LName i.e. using the constructor which takes one string parameter, so in this case, it will create the database table column with the name LName which is mapped to the LastName property. The Column attribute belongs to System.ComponentModel.DataAnnotations.Schema namespace.

```C#
using System.ComponentModel.DataAnnotations.Schema;
namespace EFCodeFirstDemo
{
    public class Student
    {
        public int StudentId { get; set; }
        [Column]
        public string FirstName { get; set; }
        [Column("LName")]
        public string LastName { get; set; }
    }
}
```

### Column Data Type in Entity Framework:

As we see, the Column Attribute class is having a property called TypeName, and that TypeName property is used to get or set the data type of a database column. That means this property gets or sets the database provider-specific data type of the column the property is mapped to. For a better understanding, please modify the Student Entity as follows. Here, you can see, we have set the DateOfBirth column name as DOB and Data type as DateTime2 using TypeName Property.

```C#
using System;
using System.ComponentModel.DataAnnotations.Schema;
namespace EFCodeFirstDemo
{
    public class Student
    {
        public int StudentId { get; set; }
        [Column]
        public string FirstName { get; set; }
        [Column("LName")]
        public string LastName { get; set; }
        [Column("DOB", TypeName = "DateTime2")]
        public DateTime DateOfBirth { get; set; }
    }
}
```

With the above changes in place, now run the application code. It should create a column with the name as DOB with the data type as DateTime2 instead of DateTime. You can verify the same in the database as shown in the below image.

![image_236.png](image_236.png)

### Column Order in Entity Framework Code First Approach:

Another property called Order is provided by the Column Attribute class which is used to set or get the order of the columns. It is 0 Based orders i.e. it is going to start from 0. As per the default convention, the Primary Key columns will come first, and then the rest of the columns based on the order that we specified in the Column Attribute Order Property. 

```C#
using System;
using System.ComponentModel.DataAnnotations.Schema;
namespace EFCodeFirstDemo
{
    public class Student
    {
        //Primary Key: Order Must be 0
        [Column(Order = 0)]
        public int StudentId { get; set; }

        [Column(Order = 2)]
        public string FirstName { get; set; }

        [Column("LName", Order =4)]
        public string LastName { get; set; }

        [Column("DOB", Order =3, TypeName = "DateTime2")]
        public DateTime DateOfBirth { get; set; }

        [Column(Order = 1)]
        public string Mobile { get; set; }
    }
}
```

Now, run the application and verify the database table columns order and it should be in proper order as per the Order parameter as shown in the below image.

![image_237.png](image_237.png)





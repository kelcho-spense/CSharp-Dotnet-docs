# Table Data Annotation Attribute in Entity Framework

The Table Data Annotation Attribute in Entity Framework Code First Approach can be applied to a domain class to configure the corresponding database table name and schema. It overrides the default convention in Entity Framework. As per the default conventions, Entity Framework API will create a database table whose name is matching with <Entity Name> + ‘s’ (or ‘es’) in a context class.

All the Data Annotation Attributes are classes that are inherited from the Attribute abstract class. Now, if you go to the definition of Table Attribute class, then you will see the following.

![image_234.png](image_234.png)

As you can see in the above TableAttribute class, it is having two properties and one constructor. The constructor is taking one string parameter which is nothing but the database table name and this is mandatory. The Schema property is optional and the use of the Name and Schema properties are as follows:

* Name: Gets the name of the table the class is mapped to. It is a read-only property.
* Schema: Gets or sets the schema of the table the class is mapped to. This is optional. It is a read-write property.

> Using square bracket [], we need to specify the attributes in .NET.

Syntax to use **Table Attribute**: `[Table(string name, Properties:[Schema = string])`
Example to use **Table Attribute**: `[Table(“StudentInfo”, Schema=”Admin”)]`

## Examples to understand Table Data Annotation Attribute in Entity Framework:

Let us understand Table Data Annotation Attribute in Entity Framework Code First Approach with an example. Let us modify the Student Entity class as follows. As you can see, we have specified the table name as StudentInfo.

```C#
using System.ComponentModel.DataAnnotations.Schema;
namespace EFCodeFirstDemo
{
    [Table("StudentInfo")]
    public class Student
    {
        public int StudentId { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }
    }
}
```

In the above example, the Table Attribute is applied to the Student Entity class. So, the Entity Framework will override the default conventions and create the StudentInfo database table instead of the Students table in the database which is going to be mapped with the above Student Entity class.

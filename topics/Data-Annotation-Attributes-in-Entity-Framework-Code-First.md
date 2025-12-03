# Data Annotation Attributes in Entity Framework Code-First

Data Annotations are nothing but .NET Attributes or .NET Classes that can be applied to our domain classes and their properties to override the default conventions in Entity Framework.

> Data Annotation Attributes give you only a subset of configuration options in Entity Framework Code-First. For the full set of configuration options in Code-First, you need to use Fluent API.

## System.ComponentModel.DataAnnotations Attributes:

The following Data Annotation Attributes impact the nullability or size of the column in a database and these Attributes belong to `System.ComponentModel.DataAnnotations` namespace.

1. **Key**: The Key Attribute can be applied to a property of a domain entity to specify a key property and make the corresponding database column a PrimaryKey column. That means it denotes one or more properties that uniquely identify an entity.
2. **Timestamp**: The Timestamp Attribute can be applied to a property of a domain entity to specify the data type of the corresponding database column as a row version.
3. **ConcurrencyCheck**: The ConcurrencyCheck Attribute can be applied to a property of a domain entity to specify that the corresponding column should be included in the optimistic concurrency check.
4. **Required**: The Required Attribute can be applied to a property of a domain entity to specify that the corresponding database column is a NotNull column. That means it specifies that a data field value is required.
5. **MinLength**: The MinLength Attribute can be applied to a property of a domain entity to specify the minimum string length allowed in the corresponding database column. That means it specifies the minimum length of array or string data allowed in a property.
6. **MaxLength**: The MaxLength Attribute can be applied to a property of a domain entity to specify the maximum string length allowed in the corresponding database column. That means it specifies the maximum length of array or string data allowed in a property.
7. **StringLength**: The StringLength Attribute can be applied to a property of a domain entity to specify the minimum and maximum string length allowed in the corresponding database column. That means it specifies the minimum and maximum length of characters that are allowed in a data field.

## System.ComponentModel.DataAnnotations.Schema Attributes

The following Data Annotation Attributes impact the Schema of a database and these Attributes belong to `System.ComponentModel.DataAnnotations.Schema` namespace.

1. **Table**: The Table Attribute can be applied to a domain entity to configure the corresponding table name and schema in the database. That means it specifies the database table that a class is mapped to. It has two properties i.e. Name and Schema which are used to specify the corresponding database table name and schema.
2. **Column**: The Column Attribute can be applied to a property of a domain entity to configure the corresponding database column name, order, and data type. That means it represents the database column that a property is mapped to. It has three properties i.e. Name, Order, and TypeName to specify the column name, order, and data type in the database.
3. **Index**: The Index Attribute can be applied to a property of a domain entity to configure that the corresponding database column should have an Index in the database. That means when this attribute is placed on a property it indicates that the database column to which the property is mapped has an index. This attribute is used by Entity Framework Migrations to create indexes on mapped database columns. Multi-column indexes are created by using the same index name in multiple attributes. The information in these attributes is then merged together to specify the actual database index. It is available from Entity Framework 6.1.
4. **ForeignKey**: The ForeignKey Attribute can be applied to a property of a domain entity to mark it as a foreign key column in the database. That means it denotes a property used as a foreign key in a relationship.
5. **NotMapped**: The NotMapped Attribute can be applied to a property or entity class that should be excluded from the model and should not generate a corresponding column or table in the database. That means it denotes that a property or class should be excluded from database mapping.
6. **InverseProperty**: The InverseProperty Attribute can be applied to a property to specify the inverse of a navigation property that represents the other end of the same relationship.
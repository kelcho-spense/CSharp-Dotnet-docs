# Default Code-First Conventions in Entity Framework

In Entity Framework Code-First Approach, the conventions are nothing but a set of default rules which automatically configure a Conceptual Model based on your domain classes

And, this is possible because of the Entity Framework Code-First Conventions. That means the database schema will be configured based on the conventions by Entity Framework API Automatically. It is also possible to change these default conventions which are followed by Entity Framework.

## What are the Default Entity Framework Code-First Conventions:

Let us understand the Default Entity Framework Code-First Conventions. They are as follows:

### Schema:

By default, the Entity Framework API creates all the database objects (tables, stored procedures, etc) with the dbo schema. If you verify that the two database tables are created with the dbo schema as shown in the below image.

![image_227.png](image_227.png)

### Table Name:

The Entity Framework API will create the Database table with the entity class name suffixed by s `i.e. <Entity Class Name> + ‘s’.` For example, the `**Student**` domain class (entity) would map to the `Students` database table and the Standard domain class would map to the Standards database table and you can verify the same in the database as shown in the below image

![image_228.png](image_228.png)

### Primary Key Name:

The Entity Framework API will create `a primary key column for the property named Id` or **`<Entity Class Name> + “Id” (case insensitive)`**. For example, we have created `StudentId` and `StandardId properties in the Student` and Standard domain classes. So, these two properties will be created as Primary Key columns in the corresponding database tables as shown in the below image.

![image_229.png](image_229.png)

> If both Id and `<Entity Class Name>` + “Id” properties are present in a single model class then it will create the Primary key based on the Id column only

### Foreign Key Column Name:

By default, the Entity Framework will look for the Foreign Key Property with the same name as the Principal Entity Primary key name in the Dependent Entity. If the foreign key property **does not exist** in the Dependent Entity class, then `Entity Framework will create a Foreign Key column in the Database table with <Dependent Navigation Property Name> + “_” + <Principal Entity Primary Key Property Name>`.
For example, as the Student Entity has the Standard Dependent Navigation Property, so the Entity Framework API will create Standard_StandardId as the foreign key column in the Students table as the Student entity does not contain a foreign key property for Standard as shown in the below image.

![image_230.png](image_230.png)

### Null and Not Null Columns:

By default, the `Entity Framework API will create a null column for all reference type properties` and `nullable primitive properties, for example, string, Nullable<int>,` Student, and Standard (all class type properties). And, the `Entity Framework will create Not Null columns for Primary Key properties and non-nullable value type properties, for example, int, float, decimal, DateTime`, etc.

![image_231.png](image_231.png)

### DB Columns Order:

The Entity Framework API will create the Database table columns in the same order as the properties are added in the entity class. However, the primary key columns would be moved to the first position in the table.

### Properties Mapping to DB:

By default, all properties will map to the database table columns. If you do not want to map any property, then you need to use the `[NotMapped]` attribute to exclude the property from column mapping

![image_232.png](image_232.png)


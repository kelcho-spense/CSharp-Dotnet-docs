# Entities in Entity Framework

At the end of this article, you will understand the following pointers in detail with Examples.

* What is an `Entity` in the Entity Framework?
* Understanding `Scalar Property` and `Navigation Property`

## What is an Entity in the Entity Framework?

![image_218.png](image_218.png)

As you can see in the above image, Departments property type is DbSet<Department> and Employees property type is DbSet<Employee>. So, here, Department and Employee are nothing but entities. An Entity in Entity Framework is a class that is included as a `DbSet<TEntity>` type property in the derived context class.

Entity Framework maps each entity to a database table and each property of an entity is mapped to a column in the database table. In our example, the Department entity is mapped with the Departments database table and the Employee entity is mapped with the Employees database table.

## Scalar Property in C#:

The Primitive Type Properties defined inside a class are called Scalar Properties. Scalar property stores the actual data. A scalar property maps to a single column in the database table. For example, in the Employee class below are the scalar properties

```C#
public int ID { get; set; }
public string Name { get; set; }
public string Email { get; set; }
public string Gender { get; set; }
public Nullable<int> Salary { get; set; }
public Nullable<int> DepartmentId { get; set; }
```
Similarly, in the Department Class Below are the Scalar properties

```C#
public int ID { get; set; }
public string Name { get; set; }
public string Location { get; set; }
```
## Navigation Property in C#:

The Navigation Property represents a relationship with another Entity. There are two types of navigation properties. They are as follows:

1. Reference Navigation Property
2. Collection Navigation Property

### Reference Navigation Property in C#:
If an entity includes a property of another entity type, then it is called a Reference Navigation Property in C#. It represents the multiplicity of one (1) i.e. it represents the one-to-one relationship between the entities. For example, in Employee Class, the following property is a Reference Navigation property. This indicates one Employee belongs to one Department.

```C#
public virtual Department Department { get; set; }
```

### Collection Navigation Property in C#:

If an entity includes a property of collection type, it is called a Collection Navigation Property in C#. It represents the multiplicity of many (*) i.e. represents one-to-many relationships. For example, in the Department class following property is a collection navigation property. This indicates that one Department has many employees.

```C#
public virtual ICollection<Employee> Employees { get; set; }
```
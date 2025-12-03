# Entity Framework

Before .NET 3.5 as a developer, we often used to write ADO.NET code to perform CRUD operations with the underlying database.

For this, we need to create a Connection Object with the database, then Open the Connection, Create the Command Object and execute the Command using Data Reader or Data Adapter. And then we create DataSet or DataTables to store the data in memory to perform different types of Operations on the Data as per the business requirements. Actually, this is a Time-Consuming, and Error-Prone Process. 

Microsoft has provided a framework called “Entity Framework” to automate all these database-related activities for our application and for this to work, we just need to provide the necessary details to the Entity Framework.

## What is the Entity Framework?
Entity Framework is an **Open-Source Object-Relational Mapping (ORM) Framework** for .NET applications that enables .NET developers to work with relational data using domain-specific objects without focusing on the underlying database tables and columns where actually the data is stored.

> **Official Definition**: Entity Framework is an object-relational mapper (O/RM) that enables .NET developers to work with a database using .NET objects. It eliminates the need for most of the data-access code that developers usually need to write.

## What is an Object-Relational Mapping Framework?

Object Relational Mapping framework automatically creates classes based on database tables, and vice versa is also true, that is, it can also automatically generate the necessary SQL to create database tables based on classes.

![image_215.png](image_215.png)

The above diagram shows that the Entity Framework fits between the Data Layer and the database. It saves the data in the database which are stored in the properties of the business entities (domain classes) and also retrieves the data from the database and converts it to business entities objects automatically.

## Why do we need to use Entity Framework in .NET Applications?

Entity Framework is an ORM Tool and ORMs Tools are basically used to increase the developer’s productivity by reducing the redundant task of doing CRUD operation against a database in a .NET Application.

1. Entity Framework can generate the necessary database commands for doing the database CRUD Operation i.e. can generate **SELECT**, **INSERT**, **UPDATE** and **DELETE** commands for us.
2. While working with Entity Framework, we can perform different types of operations on the domain objects (basically classes representing database tables) using LINQ to entities.
3. Entity Framework will generate and execute the SQL Command in the database and then store the results in the instances of your domain objects so that you can do different types of operations on the data.

## Entity Framework Features:

1. **Cross-platform**: EF Core is a cross-platform framework which means it can run on Windows, Linux, and Mac operating systems
2. **Modeling**: EF (Entity Framework) creates an EDM (Entity Data Model) based on POCO (Plain Old CLR Object) entities with get/set properties of different data types. It uses this model also when querying or saving entity data to the underlying database.
3. **Querying**: EF allows us to use LINQ queries (C#/VB.NET) to retrieve data from the underlying database. The database provider will translate these LINQ queries to the database-specific query language (e.g. SQL for a relational database). EF also allows us to execute raw SQL queries directly to the database.
4. **Change Tracking**: EF keeps track of changes that occurred to instances of our entities (Property values) that need to be submitted to the database.
5. **Saving**: EF executes INSERT, UPDATE, and DELETE commands to the database based on the changes that occurred to our entities when we call the SaveChanges() method. EF also provides the asynchronous SaveChangesAsync() method
6. **Concurrency**: EF uses Optimistic Concurrency by default to protect against overwriting changes made by another user since data was fetched from the database.
7. **Transactions**: EF performs automatic transaction management while querying or saving data. It also provides options to customize transaction management.
8. **Caching**: EF includes the first level of caching out of the box. So, repeated querying will return data from the cache instead of hitting the database.
9. **Built-in Conventions**: EF follows conventions over the configuration programming pattern, and includes a set of default rules which automatically configure the EF model.
10. **Configurations**: EF allows us to configure the EF model by using data annotation attributes or Fluent API to override default conventions.

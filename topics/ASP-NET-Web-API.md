# ASP.NET Web API

ASP.NET Core Web API is Microsoft’s modern framework for building **HTTP-based services** (**RESTful APIs**) on top of the .NET Core platform.NET Core platform. It allows developers to create robust and flexible APIs that various clients can consume, such as web applications, mobile apps, desktop applications, and third-party services.\
It enables developers to create lightweight, scalable, and secure APIs that multiple types of clients can consume.

## Prerequisites to Learn ASP.NET Core Web API

Learning to develop with ASP.NET Core Web API involves understanding general development concepts and specific technologies related to Web API development. Here are the prerequisites that are good if you know before learning ASP.NET Core Web API:

* **Basic Knowledge of C#**: ASP.NET Core is built on C#, so a solid understanding of C# Programming is essential. You should be comfortable with C# syntax, basic programming constructs like loops and conditionals, classes and objects, and more advanced concepts such as LINQ, async/await, and exception handling. This is Mandatory.
* **Understanding .NET Core Basics**: Familiarity with the .NET Core framework is important. This includes understanding the .NET Core CLI, the structure of .NET Core applications, basic concepts like dependency injection, and how to use NuGet packages. We have already discussed this in our ASP.NET Core Basic course, and it is mandatory.
* **Familiarity with Entity Framework Core**: Entity Framework Core (EF Core) is the recommended ORM for data access in ASP.NET Core applications. Understanding EF Core for performing CRUD operations with databases is highly beneficial. This is mandatory.
* **Basic Database Knowledge**: Basic knowledge of databases, especially relational databases like SQL Server, MySQL, or Oracle, is important. You should know how to design databases, write basic SQL queries, and understand concepts like tables, keys, and relationships. This is mandatory.

## Why is Web API
Web APIs (Application Programming Interfaces) are essential for allowing different software applications to communicate and exchange data with each other over the Internet. They enable data exchange between various platforms, applications, and devices, regardless of their underlying technologies.

* Web APIs enable systems developed on different technologies (like different programming languages or frameworks) to communicate effectively. For example, a mobile app built with Swift (iOS) can interact with a backend service written in ASP.NET Core (C#) using HTTP-based APIs.
* They facilitate integration between different services and applications. For instance, a weather forecasting service may expose a Web API that other applications can query to get weather data, which they can then integrate into their own functionality.
* Web APIs provide a flexible way to access and manipulate data. They allow developers to create custom client applications that can consume these APIs based on their specific needs.

## Key Characteristics of Web APIs:

* **HTTP-Based Communication**: Web APIs are designed to work over HTTP, the same protocol used for Web Browsing. This means APIs can be accessed using standard HTTP methods like GET, POST, PUT, DELETE, etc. The API endpoints are typically represented as URLs (Uniform Resource Locators).
* **Data Exchange Formats**: Web APIs use standardized data exchange formats such as JSON (JavaScript Object Notation) and XML (Extensible Markup Language) to structure and transmit data between the client and server. JSON has become the most popular format due to its simplicity and ease of use.
* **RESTful Architecture**: Web APIs are designed to follow Representational State Transfer (REST) principles. A RESTful API is stateless, uses standard HTTP methods, and organizes resources into a hierarchy with unique URLs for each resource.
* **Authentication** and **Authorization**: Web APIs implement security mechanisms for authentication and authorization to ensure that only authorized clients can access resources or perform specific actions. Common authentication methods include API keys, OAuth, and JWT (JSON Web Tokens).


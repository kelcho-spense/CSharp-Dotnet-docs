# Development Approach with Entity Framework

The entity framework provides three different approaches when working with the database and data access layer in your application as per your project requirement. These are

1. Database-First
2. Code-First
3. Model-First

## Database-First Approach of Entity Framework:

We can use the `Database First Approach` if the database schema already exists. In this approach, we generate the context and entities for the existing database using the EDM wizard. This approach is best suited for applications that use an already existing database.

![image_221.png](image_221.png)

Database First approach is useful:
* When we are working with a legacy database
* If we are working in a scenario where the database design is being done by another team and the application development starts only when the database is ready
* When we are working on a data-centric application

## Code-First Approach of Entity Framework:
You can use the Code First approach when you do not have an existing database for your application. In the code-first approach, you start writing your entities (domain classes) and context class first and then create the database from these classes using migration commands.

This approach is best suited for applications that are highly domain-centric and will have the domain model classes created first. The database here is needed only as a persistence mechanism for these domain models.

That means Developers who follow the `Domain-Driven Design (DDD) principles`, prefer to begin coding with their domain classes first and then generate the database required to persist their data.

![image_222.png](image_222.png)

Code First approach is useful:
* If there is no logic in the database
* When full control over the code, that is, there is no auto-generated model and context code
* If the database will not be changed manually

## Model-First Approach of Entity Framework:

This approach is very much similar to the Code First approach, but in this case, we use a visual EDMX designer to design our models. So in this approach, we create the entities, relationships, and inheritance hierarchies directly on the visual designer and then generate entities, the context class, and the database script from your visual model.

![image_223.png](image_223.png)

> The visual model will give us the SQL statements needed to create the database, and we can use them to create our database and connect it with our application.

The Model First approach is useful: When we really want to use the Visual Entity Designer

## Choosing the Development Approach for Your Application:
The below flowchart explains which is the right approach to develop your application using Entity Framework.

![image_224.png](image_224.png)

As per the above diagram, if you have an existing database, then you can go with Database First approach because you can create an EDM from your existing database.

But if you don’t have an existing database but you have an existing application with domain classes then you can go with the code first approach because you can create a database from your existing classes. But if you don’t have either an existing database or domain model classes then you can go with the model-first approach.


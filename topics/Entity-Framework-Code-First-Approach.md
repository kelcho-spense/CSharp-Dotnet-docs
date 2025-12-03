# Entity Framework Code First Approach

As already discussed, we need to use the Entity Framework Code First Approach when we do not have an existing database for our application. In this approach, we start developing our domain entities (domain classes) and context class first rather than designing the database first, and then based on the domain classes and context class, the entity framework will create the database. The following image shows the working style of Entity Framework Code First Approach. As you can see in the below image, the Entity Framework creates the database based on the domain classes and the context class.

![image_225.png](image_225.png)

This approach is best suited for `applications that are highly domain-centric and will have the domain model classes created first`.
`The database here is needed only as a persistence mechanism for these domain models. `

## Entity Framework Code-First Approach Workflow

The following diagram shows the workflow of the Entity Framework Code-First Approach. First, Create or modify domain classes, then configure these domain classes using Fluent-API or data annotation attributes, then create or update the database schema using automated migration or code-based migration

![image_226.png](image_226.png)
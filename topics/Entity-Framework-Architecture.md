# Entity Framework Architecture

The Architecture of the Entity Framework is composed of the following components

1. The Entity Data Model
2. LINQ to Entities
3. Entity SQL
4. The Object Services Layer
5. Entity Client Data Provider
6. ADO.NET Data Provider

The following diagram shows the overall architecture of the Entity Framework.

![image_216.png](image_216.png)

## EDM (Entity Data Model): 

The Entity Data Model (EDM) abstracts the logical or the relational schema and exposes the conceptual schema of the data using a three-layered approach i.e.

1. The Conceptual Model (C- Space) :
   The conceptual model contains the model classes (i.e. entities) and their relationships. This will be independent of your database table design. It defines your business objects and their relationships in XML files.
2. Mapping model (C-S Space):
   A Mapping Model consists of information about how the conceptual model is mapped to the storage model. The Mapping model is responsible for mapping the conceptual and logical layers (Storage layer). It maps the business objects and the relationships defined in the conceptual layer with the tables and relationships defined in the logical layer.
3. Storage model (S – Space):
   The storage model represents the schema of the underlying database. That means the storage model is the database design model which includes tables, views, Keys, stored procedures, and their relationships. The Entity Data Model uses the following three types of XML files to represent the C-Space, C-S Space, and the S-Space respectively.

## LINQ to Entities:

LINQ-to-Entities (L2E) is a query language used to write queries against the object model. It returns entities, which are defined in the conceptual model. You can use your LINQ skills here.

## Entity SQL: 

Entity SQL is another query language (For EF 6 only) just like LINQ to Entities. However, it is a little more difficult than LINQ-to-Entities (L2E) and the developer will have to learn it separately. These E-SQL queries are internally translated to data store-dependent SQL queries

## Object Service:

The Object Services layer is the Object Context, which represents the session of interaction between the applications and the data source.

* The main use of the Object Context is to perform different operations like add, update and delete instances of entities and to save the changed state back to the database with the help of queries.
* It is the ORM layer of Entity Framework, which represents the data result to the object instances of entities.
* This service allows the developer to use some of the rich ORM features like primary key mapping, change tracking, etc. by writing queries using LINQ and Entity SQL.

Apart from this, the Object Services Layer provides the following additional services:

1. Change tracking
2. Lazy loading
3. Inheritance
4. Optimistic concurrency
5. Merging data
6. Identity resolution
7. Support for querying data using Entity SQL and LINQ to Entities

## Entity Client Data Provider: 
The main responsibility of this layer is to convert LINQ-to-Entities or Entity SQL queries into a SQL query that is understood by the underlying database. It communicates with the ADO.Net data provider which in turn sends or retrieves data from the database.

## ADO.Net Data Provider:
This layer communicates with the database using standard ADO.Net.
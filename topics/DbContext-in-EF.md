# DbContext in EF

The DbContext class is one of the important classes in Entity Framework. It is a bridge between your domain or entity classes and the underlying database. For a better understanding, please have a look at the below image.

![image_217.png](image_217.png)

So, the following are the major responsibilities performed by the DbContext class:

1. `Querying`: Converts LINQ-to-Entities queries to SQL queries and then sends those queries to the database for execution.
2. `Change Tracking`: DbContext class also keeps track of the changes that occurred on the entities after querying from the database. For example, after fetching the data from the database, you have updated a few data, insert new data and delete some existing data. This information will keep tracked by the DbContext class.
3. `Persisting Data`: The DbContext class is also responsible for performing the Insert, Update and Delete operations to the database, based on entity states.
4. `Caching`: DbContext class also Provides first-level caching by default. In our upcoming articles,  we will discuss this concept in detail with Examples.
5. `Transaction`: It also provides the support to implement Transactions using the Unit of Work and Repository Pattern.
6. `Manage Relationship`: This class is also responsible for managing the relationships between the entities using CSDL, MSL, and SSDL in Db-First or Model-First approach, and using fluent API in the Code-First approach. We will discuss CSDL, MSL, SSDL, and Fluent API in detail in our coming articles.
7. `Object Materialization`: It is also used to convert the raw data which is retrieved from the database into entity objects.

## DbContext Methods in Entity Framework:

Now, let us proceed further and try to understand the important methods provided by the DbContext class in Entity Framework.

* **Entry(object entity)**: It is used to get a DbEntityEntry object for the given entity providing access to information about the entity and the ability to perform actions on the entity.
* **SaveChanges**: Saves all changes made in the context object to the underlying database. That means, it executes `INSERT`, `UPDATE`, and `DELETE` commands to the database for the entities with Added, Modified, and Deleted states.
* **SaveChangesAsync**:  This method is used to asynchronously saves all changes made in this context to the underlying database. That means, it executes `INSERT`, `UPDATE`, and `DELETE` commands to the database for the entities with **Added**, **Modified**, and **Deleted** states asynchronously.
* **Set(Type entityType)**: This method returns a non-generic DbSet instance for access to entities of the given type in the context and the underlying database. Here, the parameter entityType specifies the type of entity for which a set should be returned. That means it creates a `DbSet<TEntity>` that can be used to query and save instances of TEntity into the database.
* **OnModelCreating**: This method is called when the model for a derived context has been initialized, but before the model has been locked down and used to initialize the context. The default implementation of this method does nothing, but it can be overridden in a derived class such that the model can be further configured before it is locked down.

## Entity Framework DbContext Class Properties

1. **ChangeTracker { get; }**: This property provides access to features of the context that deal with change tracking of entities. This is a read-only property.
2. **Configuration { get; }**: This property provides access to configuration options for the context. This is a read-only property.
3. **Database { get; }**: This property creates a Database instance for the context object that allows for creation/deletion/existence checks for the underlying database. This is a read-only property.
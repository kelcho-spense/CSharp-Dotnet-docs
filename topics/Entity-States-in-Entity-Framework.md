# Entity States in Entity Framework

The Entity Lifecycle in Entity Framework describes the process in which an Entity is created, added, modified, deleted, etc. Entities have many states during their lifetime. Entity Framework maintains the state of each entity during its lifetime. Each entity has a state based on the operation performed on it via the context class (the class which is derived from DbContext class). 

![image_219.png](image_219.png)

That means the Entity State represents the state of an entity. An entity is always in any one of the following states.

* `Added`: The entity is marked as added. The entity is being tracked by the context but does not yet exist in the database.
* `Deleted`: The entity is marked as deleted. The entity is being tracked by the context and exists in the database, but has been marked for deletion from the database the next time SaveChanges is called.
* `Modified`: The entity has been modified. The entity is being tracked by the context and exists in the database, and some or all of its property values have been modified.
* `Unchanged`: The entity hasn’t been modified. The entity is being tracked by the context and exists in the database, and its property values have not changed from the values in the database.
* `Detached`: The entity is not being tracked by the context. An entity is in this state immediately after it has been created with the new operator or with one of the System.Data.Entity.DbSet Create methods.

The Context object not only holds the reference to all the entity objects as soon as retrieved from the database but also keeps track of entity states and maintains modifications made to the properties of the entity. This feature is known as Change Tracking.

## Entity Lifecycle in Entity Framework

The change in the entity state from the Unchanged to the Modified state is the only state that’s automatically handled by the context class. All other changes must be made explicitly by using the proper methods of DbContext class. The following diagram shows the different states of an entity in the Entity Framework.

![image_220.png](image_220.png)
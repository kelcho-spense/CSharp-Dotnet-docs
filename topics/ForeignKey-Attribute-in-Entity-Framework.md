# ForeignKey Attribute in Entity Framework

One of the most important concepts in a database is creating the relationship between the database tables. This relationship provides a mechanism for linking the data stores in multiple database tables and retrieving them in an efficient manner.

In order to create a link between two database tables, we must specify a Foreign Key in one table that references a unique column (Primary Key or Foreign Key) in another table. That means the Foreign Key constraint is used for binding two database tables with each other and then verifying the existence of one table’s data in other tables. A foreign key in one TABLE points to a primary key or unique key in another table.

## ForeignKey Attribute in Entity Framework:

The ForeignKey Attribute in Entity Framework is used to configure a Foreign Key Relationship between the two entities. It overrides the default Foreign Key convention which is followed by Entity Framework. As per the default convention, the Entity Framework API will look for the Foreign Key Property with the same name as the Principal Entity Primary Key Property name in the Dependent Entity.

A Relationship in Entity Framework always has two endpoints. `Each endpoint must return a navigational property that maps to the other end of the relationship`. Let us understand this with an example. The following Standard Entity is going to be `our Principal Entity and StandardId is the Primary Key Property`. Here, you can see, `we have one collection navigational property i.e. students` and this is mandatory in order to implement Foreign Key Relationships using Entity Framework Code First Approach.

```C#
using System;
using System.Collections.Generic;
namespace EFCodeFirstDemo
{
    public class Standard
    {
        public int StandardId { get; set; }
        public string StandardName { get; set; }
        public string Description { get; set; }
        public ICollection<Student> Students { get; set; }
    }
}
```

Now, modify the Student Entity as follows. Here, you can see, `we have added the StandardId property `whose name is the same as the Primary Key Property of the Standard table. `We have also added the Standard Reference Navigational Property and this is mandatory`.

```C#
using System;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
namespace EFCodeFirstDemo
{
    public class Student
    {
        public int StudentId { get; set; }
        public string FirstName { get; set; }
        public string LastName { get; set; }

        //The Following Property Exists in the Standard Entity
        public int StandardId { get; set; }

        //To Create a Foreign Key it should have the Standard Navigational Property
        public Standard Standard { get; set; }
    }
}
```

> The point that you need to remember, both Entities should and must have Navigational Properties to implement Foreign Key Relationships. In our example, Student Entity has the Standard Reference Navigational Property, and Standard Entity has the Students collection Navigation Property which will establish the one-to-many relationship between these two entities. That means one student has one standard and one standard can have multiple students.


# Application Development Process

So, generally, when we are developing an application, the process will be as follows.

- **Step1: Identify the Entities that are associated with the application :**
Suppose, we are developing an application for a School. Then for this Student Application, **who are the entities**. `The Student is an Entity. TeachingStaff is an Entity. NonTeachingStaff is another Entity`. Like this, we can identify the entities. So, in our application, we have identified three entities.

> Entities: Student, TeachingStaff, NonTeachingStaff

- **Step2: Identify the attributes of each and every entity.**
  - **Entity: Student** :
    - Attributes: Id, Name, Address, Phone, Class, Marks, Grade, Fees
  - **Entity: TeachingStaff**
    - TeachingStaff Attributes: Id, Name, Address, Phone, Designation, Salary, Qualification, Subject
  - **Entity: NonTeachingStaff**
    - NonTeachingStaff Attributes: Id, Name, Address, Phone, Designation, Salary, DeptName, ManagerId

For a better understanding, please have a look at the below diagram.

![image_107.png](image_107.png) 

- **Step3: Identify the common attributes and put them in a hierarchical order**

The Person contains four attributes `Id, Name, Address, and Phone`. Under the `Person`, we have `Student` and `Staff`. The `Student inherits from the Person`, so by default `Student will get all those four attributes`, and the `rest of all other attributes are defined inside the Student Entity`. Then we Staff Entity which is also `inherited from Person and hence all those four common attributes are also available` and plus we have defined the common attributes for Teaching and NonTeaching entities inside the Staff entity. So, `Staff will contain six attributes`. Finally, both Teaching and NonTeaching are inherited from the Staff Entity.

![image_108.png](image_108.png)

So, tomorrow if temporary staff comes into the picture, then also these properties are applicable to Temporary Staff. What you need to do is, create a new Entity with the specific properties and inherit it from the Staff entity.

- **Step4: Defining the classes that are representing the entities in Hierarchical order**

- After identifying of attributes of each entity.
- Define classes representing each and every entity. That is one class representing students, one class representing teaching staff, and another class representing the non-teaching staff. 

But, if we are defining three classes representing one entity, then there is a problem. T**he problem is there are some common attributes in each entity**. So, if we start defining three classes individually, `then there is code duplication. Why code duplication?` See, we need to define Id three times, Name three times, Address three times, and Phone number three times. Like this, we have duplication in the code.

For all the three entities which are the common attributes? Id, Name, Address, and Phone are the common attributes. Let us put these common attributes in a class called Person. Once we define this class and once, we make this class a Parent class, then no need to define these attributes three times. One time we need to declare in the parent class and then we are consuming these properties under all the child classes. That means reusability comes into the picture.

```C#
public class Person
{
    public int Id;
    public string Name;
    public string Address;
    public string Phone;
}
```

Now we can define a class called Student inheriting from the Person class. And in the student class, `we only need to define the Class, Marks, Grade, and Fees attributes as Id, Name, Address, and Phone are coming from the Person parent class`.

```C#
public class Student : Person
{
    public int Class;
    public float Fees;
    public float Marks;
    public char Grade;
}
```

Next, `you can create TeachingStaff and NonTeachingStaff classes inheriting from the Person class`. But if you look at the TeachingStaff and NonTeachingStaff entities, `apart from the four common attributes i.e. Id, Name, Address, Phone, these two entities also have another two common attributes i.e. Designation and Salary.` Again, if we put these two properties in TeachingStaff and NonTeachingStaff classes, duplication comes. So, we need to create a separate class, let us call that class Staff and this Staff class inheriting from the Person class and in this class, we will put the two common properties i.e. `Designation and Salary`. So, now the Staff class has 6 attributes, four are coming from the Person class and two are defined in this class itself.

```C#
public class Staff : Person
{
    string Designation;
    double Salary;
}
```

Now, if we make the Staff class a parent class for TeachingStaff and NonTeachingStaff, then by default six attributes are coming. So, in the TeachingStaff we only need to define properties that are only for TeachingStaff such as Qualification and Subject. On the other hand, in the NonTeachingStaff, we only need to define the properties which are only for NonTeachingStaff such as DeptName and ManagerId. And both the TeachingStaff and NonTeachingStaff classes will inherit from the Staff class. Now, we are not going to call them TeachingStaff and NonTeachingStaff, rather we call them Teaching and NonTeaching as they are inheriting from the Staff.

```C#
public class Teaching : Staff
{
    string Qualification;
    string Subject;
}
public class NonTeaching : Staff
{
    string Deptname;
    string ManagerId;
}
```

So, this should be the process of how-to apply Inheritance in Application development.

### How to Make use of Inheritance in Realtime Application Development?

Generally, when we develop an application, we will be following a process as follows.

1. Identify the entity associated with the application
2. Identify the attribute that is associated with the application.
3. Now separate the attribute of each entity in a hierarchical order without having any duplicates.
4. Convert those entities into classes.
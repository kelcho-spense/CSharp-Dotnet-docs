# OOP Principles: Inheritance

Inheritance in C# is a mechanism of `consuming the members that are defined in one class from another class`. See, we are aware that a class is a collection of members. The members defined in one class can be consumed by another class by establishing a parent/child relationship between the classes.

![image_99.png](image_99.png)

So, Inheritance in C# is a mechanism of consuming the members of one class in another class by establishing a parent/child relationship between the classes, which provides re-usability.

## How to Implement Inheritance

To Implement Inheritance in C#, we need to establish a Parent/Child relationship between classes. Let us understand how to establish a Parent/Child relationship in C#. Suppose we have a class called A with a set of members. And we have another class B, and we want this class B to be inherited from class A. The following code shows how to establish the `Parent-Child relationship between Class A and Class B.`

![image_100.png](image_100.png)

So, this is the basic process for establishing a Parent/Child relationship in C#. Now, let us see the basic syntax to establish a Parent/Child relationship between classes. The syntax is given below.

```Plain Text
[<modifiers>] class <child class> : <parent class>
```

> In Inheritance, the Child class can consume members of its Parent class as if it is the owner of those members (except private members of the parent).

## Example to Understand Inheritance
Let us see a simple example to understand Inheritance in C#. Let us create a class with two methods, as shown below.

```C#
class A
{
    public void Method1()
    {
        Console.WriteLine("Method 1");
    }
    public void Method2()
    {
        Console.WriteLine("Method 2");
    }
}
```
Here, we have created class A with two public methods, i.e., Method1 and Method2. Now, I want the same two methods in another class, i.e., class B. One way to do this is to copy the above two methods and paste them into class B as follows.

```C#
class B
{
    public void Method1()
    {
        Console.WriteLine("Method 1");
    }
    public void Method2()
    {
        Console.WriteLine("Method 2");
    }
}
```

If we do this, then it is not code re-usability. It is code rewriting that affects the size of the application. So, without rewriting, what we need to do is, we need to perform inheritance here as follows. Here, class B is inherited from class A, and hence, inside the Main method, we create an instance of class B and invoke the methods which are defined in Class A.

```C#
class B : A
{
    static void Main()
    {
        B obj = new B();
        obj.Method1();
        obj.Method2();
    }
}
```
Once you perform the Inheritance, class B can take the two members defined in class A. 

Why? Because all the properties of a Parent belong to Children. Here, class A is the Parent/Super/Base class, and Class B is the Child/Sub/Derived class.

```C#
using System;
namespace InheritanceDemo
{
    class A
    {
        public void Method1()
        {
            Console.WriteLine("Method 1");
        }
        public void Method2()
        {
            Console.WriteLine("Method 2");
        }
    }
    class B : A
    {
        static void Main()
        {
            B obj = new B();
            obj.Method1();
            obj.Method2();
            Console.ReadKey();
        }
    }
}
```
Output : 
```Plain Text
Method 1
Method 2
```

Now, let us add a new method, i.e., Method3 in Class B, as follows. Inside the Main method, if you see the method description, it shows that the method belongs to class B.

![image_101.png](image_101.png)

**The complete example is given below.**

```C#
using System;
namespace InheritanceDemo
{
    class A
    {
        public void Method1()
        {
            Console.WriteLine("Method 1");
        }
        public void Method2()
        {
            Console.WriteLine("Method 2");
        }
    }
    class B : A
    {
        public void Method3()
        {
            Console.WriteLine("Method 3");
        }
        static void Main()
        {
            B obj = new B();
            obj.Method1();
            obj.Method2();
            obj.Method3();
            Console.ReadKey();
        }
    }
}
```
## Implicit & Explicit Constructor
Right now, in our example, both class A and class B have implicit constructors. Yes, every class in C# contains an implicit constructor if as a developer we did not define any constructor explicitly. We already learned it in our constructor section.

If a constructor is defined implicitly, then it is a public constructor. In our example, class B can access the class A implicit constructor as it is public. Now, let us define one explicit constructor in Class A as follows.

```C#
class A
{
    public A()
    {
        Console.WriteLine("Class A Constructor is Called");
    }
    public void Method1()
    {
        Console.WriteLine("Method 1");
    }
    public void Method2()
    {
        Console.WriteLine("Method 2");
    }
}
```
Output :
```Plain Text
Class A Constructor is Called
Method 1
Method 2
```

> When you execute the code, the class A constructor is first called, and that is what you can see in the output. Why? This is because whenever the child class instance is created, the child class constructor will implicitly call its parent class constructors. This is a rule.

But remember, if you are defining an explicit constructor, if you make that constructor private, and if you don’t provide an access specifier, then by default, the class member’s access specifier is private in C#. For example, modify class A as follows. As you can see, we have removed the access specifier from the constructor which makes it private.

```C#
class A
{
    A()
    {
        Console.WriteLine("Class A Constructor is Called");
    }
    public void Method1()
    {
        Console.WriteLine("Method 1");
    }
    public void Method2()
    {
        Console.WriteLine("Method 2");
    }
}
```
As you can see in the code, the Class A constructor is private, so it is not accessible to Class B. Now, if you try to run the code, you will get the following compile-time error as shown in the below image which tells Class A Constructor is inaccessible due to its protection level.

![image_102.png](image_102.png)

We are getting the above error because when we create an instance of the child class, the child class constructor will implicitly call its parent class constructors. Right now, the Class B constructor trying to call the Class A constructor, which is not accessible because that constructor is private.

### How to pass dynamic value to Parent class constructor
Let us see this with an example. In the below example, the child class, i.e., class B constructor, takes one parameter and passes that parameter value to the parent class, i.e., Class A constructor. And when we are creating the instance of Class B, we need to pass the parameter value.

```C#
using System;
namespace InheritanceDemo
{
    class A
    {
        public A(int number)
        {
            Console.WriteLine($"Class A Constructor is Called : {number}");
        }
        public void Method1()
        {
            Console.WriteLine("Method 1");
        }
        public void Method2()
        {
            Console.WriteLine("Method 2");
        }
    }

    class B : A
    {
        public B(int num) : base(num)
        {
            Console.WriteLine("Class B Constructor is Called");
        }
        public void Method3()
        {
            Console.WriteLine("Method 3");
        }
        static void Main()
        {
            B obj1 = new B(10);
            B obj2 = new B(20);
            B obj3 = new B(30);
            Console.ReadKey();
        }
    }
}
```

Output:

![image_103.png](image_103.png)

So, in the above example, when we are creating the instance, we are passing the value. The value first reaches the child class constructor, and the child class constructor passes the same value to the parent class constructor. If you want, then you can also use the same value in the child class.

## Types of Inheritance

What these types of Inheritance will tell us is the number of parent classes a child class has or the number of child classes a parent class has. According to C++, why I am telling about C++ is because Object-Oriented Programming came into the picture from C++ only, there are five different types of Inheritances.

* Single Inheritance
* Multi-Level Inheritance
* Hierarchical Inheritance
* Hybrid Inheritance
* Multiple Inheritance

If you look at Single, Multilevel, and Hierarchical inheritances, they are looks very similar. At any point in time, they are having a single immediate parent class. But, if you look at Multiple and Hybrid, they are having more than one immediate parent class for a child class. So, we can broadly classify the above five categories of inheritances into two types based on immediate parent class as follows:

* Single Inheritance (Single, Multilevel, and Hierarchical)
* Multiple Inheritance (Multiple and Hybrid)

**Single Inheritance:**
If at all a class has 1 immediate parent class to it, we call it Single Inheritance in C#. For a better understanding, please have a look at the below diagram. See, how many immediate parent class C has? 1 i.e. B, and how many immediate parent class B has? 1 i.e. A. So, for class C, the immediate Parent is class B and for class B, the immediate Parent is class A.

![image_104.png](image_104.png)

**Multiple Inheritance in C#:**
If a class has more than 1 immediate parent class to it, then we call it Multiple Inheritance in C#. For a better understanding, please have a look at the below diagram. See, class C has more than one immediate Parent class i.e. A and B and hence it is Multiple Inheritance.

![image_105.png](image_105.png)

So, the point that you need to remember is how many immediate Parent classes a child class has. If one immediate Parent class, call it Single Inheritance, and if more than one immediate Parent class, call it is multiple inheritance. So, there should not be any confusion between 5 different types of inheritances, simply we have two types of inheritances.

**Example to Understand Single Inheritance:**

```C#
using System;
namespace InheritanceDemo
{
    public class Program
    {
        static void Main()
        {
            // Creating object of Child class and
            // invoke the methods of Parent and Child classes
            Cuboid obj =  new Cuboid(2, 4, 6);
            Console.WriteLine($"Volume is : {obj.Volume()}");
            Console.WriteLine($"Area is : {obj.Area()}");
            Console.WriteLine($"Perimeter is : {obj.Perimeter()}");
            Console.ReadKey();
        }
    }
    //Parent class
    public class Rectangle
    {
        public int length;
        public int breadth;
        public int Area()
        {
            return length * breadth;
        }
        public int Perimeter()
        {
            return 2 * (length + breadth);
        }
    }
    
    //Child Class
    class Cuboid : Rectangle
    {
        public int height;
        public Cuboid(int l, int b, int h)
        {
            length = l;
            breadth = b;
            height = h;
        }
        public int Volume()
        {
            return length * breadth * height;
        }
    }
}
```

Output:

![image_106.png](image_106.png)

**Example to Understand Multiple Inheritance:**

```C#
using System;
namespace InheritanceDemo
{
    public class Program
    {
        static void Main()
        {
            // Creating object of Child class and
            // invoke the methods of Parent classes and Child class
            SmartPhone obj = new SmartPhone(); ;
            obj.GetPhoneModel();
            obj.GetCameraDetails();
            obj.GetDetails();

            Console.ReadKey();
        }
    }
    //Parent Class 1
    class Phone
    {
        public void GetPhoneModel()
        {
            Console.WriteLine("Redmi Note 5 Pro");
        }
    }
    //Parent class2
    class Camera
    {
        public void GetCameraDetails()
        {
            Console.WriteLine("24 Mega Pixel Camera");
        }
    }

    //Child Class derived from more than one Parent class
    class SmartPhone : Phone, Camera
    {
        public void GetDetails()
        {
            Console.WriteLine("Its a RedMi Smart Phone");
        }
    }
}
```

`Output: Compile Time Error`

> Handling the complexity caused due to multiple inheritances is very complex. Hence it was not supported in dot net with class and it can be done with interfaces. So, in our Multiple Inheritance articles, we will discuss this concept in detail.

## How to use Inheritance in Application Development

Inheritance is something that comes into the picture, not in the middle of a project or middle of application development. This can also come in the middle of the project development but generally when we start application development, in the initial stages only we plan inheritance and implement it in our project.

### What is an Entity?
In DBMS terminology **what is an Entity?** `An Entity is something that is associated with a set of attributes`. An Entity can be a living or non-living object. But anything that is associated with a set of attributes is called Entity.

Remember, when we are going to develop an application, our application mainly deals with these Entities. Suppose, you are developing an application for a Bank. So, the Entity associated with the bank is a customer. `A customer is an Entity`. You are developing an application for a school; `the Student will be the Entity`. Suppose, you are developing an application for a business, t`hen Employee is an entity`. So, every application that we develop is associated with a set of entities.

# Access Specifiers

Every keyword that we use such as private, public, protected, virtual, sealed, partial, abstract, static, base, etc. is called Modifiers. _Access Specifiers are special kinds of modifiers using which we can define the scope of a type and its members_.

So, in simple words, we can say that the Access Specifiers are used to define the **scope of the type** (**Class, Interface, Structs, Delegate, Enum, etc.**) as well as the **scope of their members (Variables, Properties, Constructors, and Methods**). Scope means accessibility or visibility that is who can access them and who cannot access them are defined by the Access Specifiers. See, I have a class with a set of members, who can consume these members, and who cannot consume these members are defined by the access specifiers.

C# supports **6 types of access specifiers**. They are as follows

* Private
* Public
* Protected
* Internal
* Protected Internal
* Private Protected (C# Version 7.2 onwards)

![image_93.png](image_93.png)

Before understanding Access Specifier, let us first understand what are Types and Type Members in C#. Please have a look at the below diagram. Here, Example (which is created by using the class keyword) is a Type, and Variable ID, Property Name, Constructor Example, and Method Display are type members.

So, in general classes, structs, enums, interfaces, and delegates are called types, and variables, properties, constructors, methods, etc. that normally reside within a type are called type members.

### Private Access Specifier/Modifier
When we declare a type member (variable, property, method, constructor, etc) as private, then we can access that member with the class only

Let us understand Private Members with an example. Now, go to the class library project and modify class1.cs class file as follows. As you can see, here we have created three classes and in the AssemblyOneClass1 we have created one private variable and then tried to access the private variable within the same class (AssemblyOneClass1), from the derived class (AssemblyOneClass2), and from the non-derived class (AssemblyOneClass3). And all these classes are within the same assembly only.

```C#
using System;
namespace AssemblyOne
{
    public class AssemblyOneClass1
    {
        private int Id;
        public void Display1()
        {
            //Private Member Accessible with the Containing Type only
            //Where they are created, they are available only within that type
            Console.WriteLine(Id);
        }
    }
    public class AssemblyOneClass2 : AssemblyOneClass1
    {
        public void Display2()
        {
            //You cannot access the Private Member from the Derived Class
            //Within the Same Assembly
            Console.WriteLine(Id); //Compile Time Error
        }
    }

    public class AssemblyOneClass3
    {
        public void Dispplay3()
        {
            //You cannot access the Private Member from the Non-Derived Classes
            //Within the Same Assembly
            AssemblyOneClass1 obj = new AssemblyOneClass1();
            Console.WriteLine(obj.Id); //Compile Time Error
        }
    }
}
```

When you try to build the above code, you will get some compilation errors as shown in the below image. Here, you can see, it is clearly saying that you cannot access ‘AssemblyOneClass1.Id’ due to its protection level

![image_94.png](image_94.png)

These errors make sense that you cannot access the private members from derived and non-derived classes from different assemblies also.

> So, the scope of the **private member** in C#.NET is as follows:
> * With the Class: **YES**
> * Derived Class in Same Assembly: **NO**
> * Non-Derived Class in Same Assembly: **NO**
> * Derived Class in Other Assemblies: **NO**
> * Non-Derived Class in Other Assemblies: **NO**

### Public Access Specifier/Modifier
When we declare a type member (variable, property, method, constructor, etc) as public, then we can access that member from anywhere. That means there is no restriction for public members. 

```C#
using System;
namespace AssemblyOne
{
    public class AssemblyOneClass1
    {
        public int Id;
        public void Display1()
        {
            //Public Members Accessible with the Containing Type
            //Where they are created
            Console.WriteLine(Id);
        }
    }
    public class AssemblyOneClass2 : AssemblyOneClass1
    {
        public void Display2()
        {
            //We Can access public Members from Derived Class
            //Within the Same Assembly
            Console.WriteLine(Id); //No-Compile Time Error
        }
    }

    public class AssemblyOneClass3
    {
        public void Dispplay3()
        {
            //We Can access public Members from Non-Derived Classes
            //Within the Same Assembly
            AssemblyOneClass1 obj = new AssemblyOneClass1();
            Console.WriteLine(obj.Id); //No-Compile Time Error
        }
    }
}
```

> So, the scope of the `public member in C#.NET` is as follows:
>   * With the Class: **YES**
>   * Derived Class in Same Assembly: **YES**
>   * Non-Derived Class in Same Assembly: **YES**
>   * Derived Class in Other Assemblies: **YES**
>   * Non-Derived Class in Other Assemblies: **YES**

### Protected Access Specifier/Modifier
Protected Members in C# are available within the containing type as well as to the types that are derived from the containing type. That means protected members are available within the parent class (i.e. the containing type) as well as to the child/derived classes (classes derived from the containing type). 

```C#
using System;
namespace AssemblyOne
{
    public class AssemblyOneClass1
    {
        protected int Id;
        public void Display1()
        {
            //protected Members Accessible with the Containing Type 
            //Where they are created
            Console.WriteLine(Id);
        }
    }
    public class AssemblyOneClass2 : AssemblyOneClass1
    {
        public void Display2()
        {
            //We Can access protected Member from Derived Classes
            //Within the Same Assembly
            Console.WriteLine(Id); //No-Compile Time Error
        }
    }

    public class AssemblyOneClass3
    {
        public void Dispplay3()
        {
            //We Cannot access protected Member from Non-Derived Classes
            //Within the Same Assembly
            AssemblyOneClass1 obj = new AssemblyOneClass1();
            Console.WriteLine(obj.Id); //Compile Time Error
        }
    }
}
```
Output:
![image_95.png](image_95.png)

> So, the scope of the `protected members in C#.NET` is as follows:
> * With the Class: **YES**
> * Derived Class in Same Assembly: **YES**
> * Non-Derived Class in Same Assembly: **NO**
> * Derived Class in Other Assemblies: **YES**
> * Non-Derived Class in Other Assemblies: **NO**

### Internal Access Specifier/Modifier

Whenever a member is declared with Internal Access Specifier in C#, then it is available anywhere within the containing assembly. It’s a compile-time error to access an internal member from outside the containing assembly.

```C#
using System;
namespace AssemblyOne
{
    public class AssemblyOneClass1
    {
        internal int Id;
        public void Display1()
        {
            //internal Members Accessible with the Containing Type 
            //Where they are created
            Console.WriteLine(Id);
        }
    }
    public class AssemblyOneClass2 : AssemblyOneClass1
    {
        public void Display2()
        {
            //We can access internal Members from Derived Classes
            //Within the Same Assembly
            Console.WriteLine(Id); //No-Compile Time Error
        }
    }

    public class AssemblyOneClass3
    {
        public void Dispplay3()
        {
            //We can access internal Members from Non-Derived Classes
            //Within the Same Assembly
            AssemblyOneClass1 obj = new AssemblyOneClass1();
            Console.WriteLine(obj.Id); //No-Compile Time Error
        }
    }
}
```

> So, the scope of the `internal members in C#.NET` is as follows:
> * With the Class: **YES**
> * Derived Class in Same Assembly: **YES**
> * Non-Derived Class in Same Assembly: **YES**
> * Derived Class in Other Assemblies: **NO**
> * Non-Derived Class in Other Assemblies: **NO**

### Protected Internal Access Specifier/Modifier
Protected Internal Members in C# can be accessed anywhere within the same assembly i.e. in which it is declared or from within a derived class from another assembly. So, we can think, it is a combination of Protected and Internal access specifiers. If you understood the Protected and Internal access specifiers, then this should be very easy to follow. Protected means, members can be accessed within the derived classes, and Internal means within the same assembly. 

Let us understand this Protected Internal Access Specifier in C# with an example. Now, modify class1.cs class file as follows: Here, we are modifying the variable from internal to protected internal. Here, you can observe while accessing the protected internal member from the containing type, from the derived classes, and from the non-derived class within the same assembly, we are not getting any compilation error.

```C#
using System;
namespace AssemblyOne
{
    public class AssemblyOneClass1
    {
        protected internal int Id;
        public void Display1()
        {
            //protected internal Members Accessible with the Containing Type 
            //Where they are created
            Console.WriteLine(Id);
        }
    }
    public class AssemblyOneClass2 : AssemblyOneClass1
    {
        public void Display2()
        {
            //We can access protected internal Member from Derived Classes
            //Within the Same Assembly
            Console.WriteLine(Id); //No-Compile Time Error
        }
    }

    public class AssemblyOneClass3
    {
        public void Dispplay3()
        {
            //We can access protected internal Member from Non-Derived Classes
            //Within the Same Assembly
            AssemblyOneClass1 obj = new AssemblyOneClass1();
            Console.WriteLine(obj.Id); //No-Compile Time Error
        }
    }
}
```
Now, let us try to access the protected internal members from a different assembly. Modify the Program.cs class file as follows. From other assemblies, you can access the protected internal member from the derived classes, but you cannot access from the non-derived classes.

```C#
using AssemblyOne;
using System;
namespace AccessSpecifierDemo
{
    public class Program
    {
        static void Main(string[] args)
        {
        }
    }

    public class AnotherAssemblyClass1 : AssemblyOneClass1
    {
        public void Display4()
        {
            //We can access the protected internal Members from Derived Classes
            //from Other Assemblies
            Console.WriteLine(Id); //No-Compile Time Error
        }
    }

    public class AnotherAssemblyClass2
    {
        public void Dispplay3()
        {
            //We cannot access protected internal Members from Non-Derived Classes
            //from Other Assemblies
            AssemblyOneClass1 obj = new AssemblyOneClass1();
            Console.WriteLine(obj.Id); //Compile Time Error
        }
    }
}
```
Output:
![image_96.png](image_96.png)

> So, the scope of the `protected internal members in C#.NET` is as follows:
> * With the Class: **YES**
> * Derived Class in Same Assembly: **YES**
> * Non-Derived Class in Same Assembly: **YES**
> * Derived Class in Other Assemblies: **YES**
> * Non-Derived Class in Other Assemblies: **NO**

> Note: Here, I have shown the example by using a variable, but the same is applicable to other members of a class like properties, methods, and constructors. The following table shows the summary of all the access specifiers with the type members.
 
![image_97.png](image_97.png)
# Interface in C#

The Interface in C# is a Fully Unimplemented Class used for declaring a set of operations/methods of an object. So, we can define an interface as a pure abstract class, which allows us to define only abstract methods. The abstract method means a method without a body or implementation. It is used to achieve multiple inheritances, which the class can’t achieve. It is used to achieve full abstraction because it cannot have a method body.

### Differences Between Concrete Class, Abstract Class, and Interface

1. **Class**: Contains only the Non-Abstract Methods (Methods with Method Body).
2. **Abstract** Class: Contains both Non-Abstract Methods (Methods with Method Body) and Abstract Methods (Methods without Method Body).
3. **Interface**: Contain only Abstract Methods (Methods without Method Body).

> In Inheritance, we already learned that a class inherits from another class, and the child class consumes the Parent class members. Please observe the following diagram. Here, we have class A with a set of members, and class B inherits from class A. And there is a relationship called Parent/Child relation between them. Once the Parent/Child relationship is established, then the members of class A can be consumed under class B. So, this is what we learned in Inheritance.

![image_109.png](image_109.png)

Now, just like a class has another class as a Parent, a class can also have an Interface as a Parent. If a class has an interface as a Parent, the class is responsible for implementing all the abstract methods of the interface. For a better understanding, please have a look at the below diagram.

![image_110.png](image_110.png)

> Simply speaking, the Parent Interface imposes restrictions on the Child Classes. What restrictions? The restrictions are to implement each and every method of the interface under the child class.

> Generally, a class inherits from another class to consume members of its Parent. On the other hand, if a class inherits from an interface, it is to implement the members of its Parent (Interface), not for consumption.

## How to Define an Interface in C#

The way we define a class in the same way we need to define an interface in C#. In a class declaration, we need to use the class keyword, whereas in an interface declaration, we need to use the interface keyword. Moreover, in an interface, we can only declare abstract members. For a better understanding, please have a look at the below diagram.

![image_111.png](image_111.png)

For a better understanding, please have a look at the below example. Here, we have created one interface with the name ITestInterface by using the interface keyword.

```C#
interface ITestInterface
{
    //Abstract Member Declarations
}
```

### How to Define Abstract Methods in an Interface in C#?
In a class (i.e., Abstract Class), we generally use the abstract keyword to define abstract methods as follows.

**public abstract void Add(int num1, int num2);**

If you want to write the above abstract method in an interface, then you don’t require public and abstract keywords in the method signature as follows:

**void Add(int num1, int num2);**

While working with Interface, we need to remember some Rules. Let us discuss those rules one by one with Examples.

- Point 1: The first point that you need to remember is that `the default scope for an interface’s members is public`, `whereas it is private in the case of a class`.

- Point 2: The second point that you need to remember is `by default, every member of an interface is abstract, so we aren’t required to use the abstract modifier on it again`, just like we do in the case of an abstract class.

### Example to Understand Interface in C#:

```C#
using System;
namespace AbstractClassMethods
{     
    interface ITestInterface1
    {
        void Add(int num1, int num2);
    }
    interface ITestInterface2 : ITestInterface1
    {
        void Sub(int num1, int num2);
    }

    public class ImplementationClass1 : ITestInterface1
    {
        //Implement only the Add method
        public void Add(int num1, int num2)
        {
            Console.WriteLine($"Sum of {num1} and {num2} is {num1 + num2}");
        }
    }

    public class ImplementationClass2 : ITestInterface2
    {
        //Implement Both Add and Sub method
        public void Add(int num1, int num2)
        {
            Console.WriteLine($"Sum of {num1} and {num2} is {num1 + num2}");
        }

        public void Sub(int num1, int num2)
        {
            Console.WriteLine($"Divison of {num1} and {num2} is {num1 - num2}");
        }
    }
    class Program
    {
        static void Main()
        {
            ImplementationClass1 obj1 = new ImplementationClass1();
            //Using obj1 we can only call Add method
            obj1.Add(10, 20);
            //We cannot call Sub method
            //obj1.Sub(100, 20);

            ImplementationClass2 obj2 = new ImplementationClass2();
            //Using obj2 we can call both Add and Sub method
            obj2.Add(10, 20);
            obj2.Sub(100, 20);

            Console.ReadKey();
        }
    }
}
```

**Output:**

![image_112.png](image_112.png)
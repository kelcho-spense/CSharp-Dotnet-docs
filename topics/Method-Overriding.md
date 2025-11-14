# Method Overriding

The process of re-implementing the superclass non-static, non-private, and non-sealed method in the subclass with the same signature is called Method Overriding in C#. The same signature means the name and the parameters (type, number, and order of the parameters) should be the same. 

## When do we need to override a method in C#?
> If the Super Class or Parent Class method `logic is not fulfilling the Sub Class or Child Class business requirements`, `then the Sub Class or Child Class needs to override the superclass method` with the required business logic. Usually, in most real-time applications, the Parent Class methods are implemented with generic logic which is common for all the next-level sub-classes.

## How can we Override a Parent Class Method under Child Class 

If you want to override the Parent class method in its Child classes, first the method in the parent class must be declared as virtual by using the virtual keyword, then only the child classes get permission for overriding that method. **`Declaring the method as virtual is marking the method as overridable`**. If the child class wants to override the parent class virtual method, **`then the child class can override it with the help of the override modifier`**. But overriding the parent class virtual methods under the child classes is not mandatory. The syntax is shown below to implement Method Overriding in C#.

![image_123.png](image_123.png)

### Example to Understand Method Overriding

```C#
using System;
namespace PolymorphismDemo
{
    class Class1
    {
        //Virtual Function (Overridable Method)
        public virtual void Show()
        {
            //Parent Class Logic Same for All Child Classes
            Console.WriteLine("Parent Class Show Method");
        }
    }

    class Class2 : Class1
    {
        //Overriding Method
        public override void Show()
        {
            //Child Class Reimplementing the Logic
            Console.WriteLine("Child Class Show Method");
        } 
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            Class1 obj1 = new Class2();
            obj1.Show();

            Class2 obj2 = new Class2();
            obj2.Show();
            Console.ReadKey();
        }
    }
}
```

**Output:**

![image_124.png](image_124.png)
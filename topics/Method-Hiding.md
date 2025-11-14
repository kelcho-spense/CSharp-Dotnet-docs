# Method Hiding

Method Overriding is an approach of re-implementing the parent class methods under the child class exactly with the same signature (same name and same parameters).

Method Hiding/Shadowing is also an approach of re-implementing the parent class methods under the child class exactly with the same signature (same name and same parameters).

## How we can Re-Implement a Parent Method in the Child Class in C#

We can re-implement the parent class methods under the child classes in two different approaches. They are as follows

* Method Overriding
* Method Hiding
Then what are the differences between them, let us understand.

### How to Implement Method Hiding/Shadowing

Please have a look at the following image to understand the syntax of Method Hiding/Shadowing in C#. It does not matter whether the parent class method is virtual or not. We can hide both virtual and non-virtual methods under the child class. Again, we can hide the method in the child class in two ways i.e. **`by using the new keyword`** and also, `without using the new keyword`.

![image_125.png](image_125.png)

> When we use the new keyword to hide a Parent Class Methods under the child class, then it is called Method Hiding/Shadowing

> Using the new keyword for re-implementing the Parent Class Methods under the child class is optional.

If you look at the Show method, then it is declared as virtual inside the Parent class, so we can override this virtual method inside the Child class by using the override modifier. But we cannot override the Display method inside the Child class as it is not declared as virtual inside the Parent class. But we want to re-implement the method. In that case, we need to re-implement the Parent Class Display Method using the new keyword inside the Child class which is nothing but Method Hiding/Shadowing in C#. The complete example code is given below.

```C#
using System;
namespace MethodHiding
{
    public class Parent
    {
        public virtual void Show()
        {
            Console.WriteLine("Parent Class Show Method");
        }
        public void Display()
        {
            Console.WriteLine("Parent Class Display Method");
        }
    }
    public class Child : Parent
    {
        //Method Overriding
        public override void Show()
        {
            Console.WriteLine("Child Class Show Method");
        }

        //Method Hiding/Shadowing
        public new void Display()
        {
            Console.WriteLine("Child Class Display Method");
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Child obj = new Child();
            obj.Show();
            obj.Display();
            Console.ReadKey();
        }
    }
}
```

Output:

![image_126.png](image_126.png)

> So, here, you can observe both Method Overriding and Method Hiding doing the same thing. That is re-implementing the Parent class methods under the Child class. Then what are the differences between them? With method overriding, you can re-implement only virtual methods. On the other hand, with Method Hiding, you can re-implement any methods.
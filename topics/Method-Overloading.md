# Method Overloading

Method Overloading means it is an approach to defining multiple methods under the class with a single name. So, we can define more than one method with the same name in a class. 

![image_117.png](image_117.png)

## Example to Understand Method Overloading:

```C#
using System;
namespace MethodOverloading
{
    class Program
    {
     
        public void Method()
        {
            Console.WriteLine("1st Method");
        }
        public void Method(int i)
        {
            Console.WriteLine("2nd Method");
        }
        public void Method(string s)
        {
            Console.WriteLine("3rd Method");
        }
        public void Method(int i, string s)
        {
            Console.WriteLine("4th Method");
        }
        public void Method(string s, int i)
        {
            Console.WriteLine("5th Method");
        }  
           static void Main(string[] args)
        {
            Program obj = new Program();
            obj.Method(); //Invoke the 1st Method
            obj.Method(10); //Invoke the 2nd Method
            obj.Method("Hello"); //Invoke the 3rd Method
            obj.Method(10, "Hello"); //Invoke the 4th Method
            obj.Method("Hello", 10); //Invoke the 5th Method

            Console.ReadKey();
        }  
    }
}
```
**Output:**

![image_118.png](image_118.png)

### When should we Overload Methods

We have understood what is Method Overloading and how to implement the Method Overloading in C#. But, the important question is when we need to implement or when we need to go for Method Overloading in C#. Let us understand this with an example.

Here, we have created three methods with the same name to perform the addition of two integers, two floats, and two strings. So, when we give two integer numbers we will get one output and when we provide two string values, then we will get a different output, and similarly, when we give two float numbers we will get another output. That means when the input changes the output or behavior also automatically changes. This is called polymorphism in C#.

```C#
using System;
namespace MethodOverloading
{
    class Program
    {
        public void Add(int a, int b)
        {
            Console.WriteLine(a + b);
        }
        public void Add(float x, float y)
        {
            Console.WriteLine(x + y);
        }
        public void Add(string s1, string s2)
        {
            Console.WriteLine(s1 +" "+ s2);
        }
        static void Main(string[] args)
        {
            Program obj = new Program();
            obj.Add(10, 20);
            obj.Add(10.5f, 20.5f);
            obj.Add("Pranaya", "Rout");
            Console.ReadKey();
        }
    }
}
```
Output:

![image_119.png](image_119.png)

Suppose you are the user of the Program class and when you create the Program class instance and when you type the object name and dot operator, then you will not see three different Add methods, rather you will see only one Add method with two overloaded versions of the Add method as shown in the below image.

![image_120.png](image_120.png)

> So, the advantage of using Method overloading is that if we overload the methods, then the `user of our application gets a comfortable feeling in using the method with an impression that he/she calling one method by passing different types of values.`
> The best example for us is the system-defined “WriteLine()” method. It is an overloaded method, not a single method taking different types of values. If you go to the definition of the Console class, then you will see the following overloaded versions of the WriteLine method defined inside the Console class.

![image_121.png](image_121.png)

### What is Inheritance-Based Method Overloading

A method that is defined in the parent class can also be overloaded under its child class. It is called Inheritance Based Method Overloading in C#. See the following example for a better understanding. As you can see in the below code, we have defined the Add method twice in the class Class1 and also defined the Add method in the child class Class1. Here, notice every Add method takes different types of parameters.

```C#
using System;
namespace MethodOverloading
{
    class Class1
    {
        public void Add(int a, int b)
        {
            Console.WriteLine(a + b);
        }
        public void Add(float x, float y)
        {
            Console.WriteLine(x + y);
        }
    }
    class Class2 : Class1
    {
        public void Add(string s1, string s2)
        {
            Console.WriteLine(s1 +" "+ s2);
        }
    }
    class Program
    {
        static void Main(string[] args)
        {
            Class2 obj = new Class2();
            obj.Add(10, 20);
            obj.Add(10.5f, 20.7f);
            obj.Add("Pranaya", "Rout");
            Console.ReadKey();
        }
    }
}
```

> To overload a parent class method under its child class the child class does not require any permission from its parent class.

### Example to Understand Constructor Method Overloading

Please have a look at the following example. Here, we are creating three different versions of the Constructor, and each constructor takes a different number of parameters, and this is called Constructor Overloading in C#.

```C#
using System;
namespace ConstructorOverloading
{
    class ConstructorOverloading
    {
        int x, y, z;
        public ConstructorOverloading(int x)
        {
            Console.WriteLine("Constructor1 Called");
            this.x = 10;
        }
        public ConstructorOverloading(int x, int y)
        {
            Console.WriteLine("Constructor2 Called");
            this.x = x;
            this.y = y;
        }
        public ConstructorOverloading(int x, int y, int z)
        {
            Console.WriteLine("Constructor3 Called");
            this.x = x;
            this.y = y;
            this.z = z;
        }
        public void Display()
        {
            Console.WriteLine($"X={x}, Y={y}, Z={z}");
        }
    }
    class Test
    {
        static void Main(string[] args)
        {
            ConstructorOverloading obj1 = new ConstructorOverloading(10);
            obj1.Display();
            ConstructorOverloading obj2 = new ConstructorOverloading(10, 20);
            obj2.Display();
            ConstructorOverloading obj3 = new ConstructorOverloading(10, 20, 30);
            obj3.Display();
            Console.ReadKey();
        }
    }
}
```

Output:

![image_122.png](image_122.png)
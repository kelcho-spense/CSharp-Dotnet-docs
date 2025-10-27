# Constructors in C#

It is a special method present inside a class responsible for initializing the variables of that class.

## How to Define the Constructor Explicitly
We can also define the constructor explicitly in C#. The following is the explicit constructor syntax.

![image_89.png](image_89.png)

Whenever we are creating an instance, there will be a call to the class constructor. For a better understanding, please have a look at the below example. Here, we defined one parameter less constructor explicitly, and then from the Main method, we create an instance. When we create the instance, it will make a call to the constructor, and the statements written inside the constructor will be executed. In this case, it will execute the print statement in the console.

```C#
using System;
namespace ConstructorDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            ExplicitConstructor obj = new ExplicitConstructor();
            
            Console.ReadKey();
        }
    }
    class ExplicitConstructor
    {
        public ExplicitConstructor()
        {
            Console.WriteLine("Explicit Constructor is Called!");
        }
    }
}
```
**Output**: Explicit Constructor is Called!

One more important point that you need to remember is, how many instances you created, and that many times the constructor is called for us. Let us prove this. Please modify the example code as follows. Here, I am creating the instance four times and it should and must call the constructor 4 times and we should see the print statement four times in the console window.

```C#
using System;
namespace ConstructorDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            ExplicitConstructor obj1 = new ExplicitConstructor();
            ExplicitConstructor obj2 = new ExplicitConstructor();
            ExplicitConstructor obj3 = new ExplicitConstructor();
            ExplicitConstructor obj4 = new ExplicitConstructor();

            Console.ReadKey();
        }
    }
    class ExplicitConstructor
    {
        public ExplicitConstructor()
        {
            Console.WriteLine("Explicit Constructor is Called!");
        }
    }
}
```
output :
```Plain Text
Explicit Constructor is Called!
Explicit Constructor is Called!
Explicit Constructor is Called!
Explicit Constructor is Called!
```

We should not use the word Implicitly while calling the constructor in C#, why?
See, if we are not defining any constructor explicitly, then the compiler will provide the constructor which is called Implicitly Constructor. See, the following example. If you move the mouse pointer over the Test class, then you will see the following. Here, Test is a class present under the ConsructorDemo namespace.

![image_90.png](image_90.png)

Now, move the mouse pointer to Test() as shown in the below image. Here, the first Test is the class name and the second Test() is the constructor. That means we are calling the constructor explicitly.

![image_91.png](image_91.png)

Here, we are explicitly making a call to the constructor and when we call the constructor, the implicit constructor which is provided by the compiler is called and will initialize the variables.

### What are the rules to follow while working with C# Constructor?
* The constructor’s name should be the same as the class name.
* It should not contain a return type even void also.
* As part of the constructor body return statement with a value is not allowed.

### User-Defined Default Constructor
The constructor which is defined by the user without any parameter is called the user-defined default constructor. This constructor does not accept any argument but as part of the constructor body, you can write your own logic.

#### Example to understand User-defined Default Constructor

In the below example, within the Employee class, we have created a public parameterless constructor which is used to initialize the variables with some default hard-coded values. And then from the Main method, we created an instance of the Employee class and invoke the Display method.

```C#
using System;
namespace ConstructorDemo
{
    class Employee
    {
        public int Id, Age;
        public string Address, Name;
        public bool IsPermanent;

        //User Defined Default Constructor
        public Employee()
        {
            Id = 100;
            Age = 30;
            Address = "Bhubaneswar";
            Name = "Anurag";
            IsPermanent = true;
        }

        public void Display()
        {
            Console.WriteLine("Employee Id is:  " + Id);
            Console.WriteLine("Employee Age is:  " + Age);
            Console.WriteLine("Employee Address is:  " + Address);
            Console.WriteLine("Employee Name is:  " + Name);
            Console.WriteLine("Is Employee Permanent:  " + IsPermanent);
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Employee e1 = new Employee();
            e1.Display();

            Console.ReadKey();
        }
    }
}
```

### When should we define a parameterized constructor in a class?
If we want to initialize the object dynamically with the user-given values or if we want to initialize each instance of a class with a different set of values then we need to use the Parameterized Constructor in C#. The advantage is that we can initialize each instance with different values.

### What is Parameterized Constructor?
If a constructor method is defined with parameters, we call it a Parameterized Constructor in C#, and these constructors are defined by the programmers only but never can be defined implicitly. So, in simple words, we can say that the developer-given constructor with parameters is called Parameterized Constructor in C#.

![image_92.png](image_92.png)

```C#
using System;
namespace ConstructorDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            ParameterizedConstructor obj = new ParameterizedConstructor(10);
            Console.ReadKey();
        }
    }

    public class ParameterizedConstructor
    {
        public ParameterizedConstructor(int i)
        {
            Console.WriteLine($"Parameterized Constructor is Called: {i}");
        }
    }
}
```

**Output**: Parameterized Constructor is Called: 10
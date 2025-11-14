# Custom Exceptions

## Types of Exceptions in C#:
Before understanding how to create Custom Exceptions or used Defined Exceptions in C#, let us first understand what are the different types of Exceptions available. In C#, the exceptions are divided into two types. They are as follows:

* System Exception: These exceptions are caused by the CLR.
* Application Exception: These exceptions are caused by the programmer.

For a better understanding, please have a look at the below image. The Exception is the parent class of all Exception classes. From the Exception class, SystemException and ApplicationException classes are defined.

![image_145.png](image_145.png)

### What are System Exceptions?
An exception that is raised implicitly under a program at runtime by the Exception Manager (Component of CLR) because of some logical mistakes (some predefined conditions) is known as System Exception. For example, if you are diving an integer number by zero, then one system exception is raised called DivideByZero. Similarly, if you are taking an alphanumeric string value from the user and trying to store that value in an integer variable, then one system exception is raised called FormatException.

1. DivideByZeroException
2. IndexOutOfRangeException
3. FormatException
4. SQLException
5. NullReferenceException, Etc.

### What are Application Exceptions
An exception that is raised explicitly under a program based on our own condition (i.e. user-defined condition) is known as an application exception. As a programmer, we can raise application exceptions at any given point in time. For example, our requirement is that while performing the division operation, we need to check that if the second number is an odd number, then we need to throw an exception. 

To raise an Application Exception in C#, we need to adopt the following process. First, we need to create a custom Exception class by inheriting it from the Parent Exception class and then we need to create an instance of the Custom Exception class and then we need to throw that instance.

![image_146.png](image_146.png)

#### Why do we need to create Custom Exceptions in C#?
If none of the already existing .NET exception classes serve our purpose then we need to go for custom exceptions in C#.

#### How to Create Own Custom Exception Class in C#?
Before creating the Custom Exception class, we need to see the class definition of the Exception class as our Custom Exception class is going to be inherited from the parent Exception class. If you go to the definition of Exception class, then you will see the following.

![image_147.png](image_147.png)

Now, to create a Custom Exception class in C#, we need to follow the below steps.

1. Step1: **`Define a new class inheriting from the predefined Exception class`** so that the new class also acts as an Exception class.
2. Step2: Then as per your requirement, **`override the virtual members that are defined inside the Exception class like Message, Source, StackTrace`**, etc with the required error message.

```C#
using System;
namespace ExceptionHandlingDemo
{
    //Creating our own Exception Class by inheriting Exception class
    public class OddNumberException : Exception
    {
        //Overriding the Message property
        public override string Message
        {
            get
            {
                return "Divisor Cannot be Odd Number";
            }
        }

        //Overriding the HelpLink Property
        public override string HelpLink
        {
            get
            {
                return "Get More Information from here: https://dotnettutorials.net/lesson/create-custom-exception-csharp/";
            }
        }
    }
}
```

Now, as per our business logic, we can explicitly create an instance of the OddNumberException class and we can explicitly throw that instance from our application code. For a better understanding, please have a look at the following code. Here, inside the Main method, we are taking two numbers from the user and then checking if the second number is odd or not. If the second number i.e. divisor is odd, then we are creating an instance of the OddNumberException class and throwing that instance.

```C#
using System;
namespace ExceptionHandlingDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int Number1, Number2, Result;
            try
            {
                Console.WriteLine("Enter First Number:");
                Number1 = int.Parse(Console.ReadLine());

                Console.WriteLine("Enter Second Number:");
                Number2 = int.Parse(Console.ReadLine());

                if (Number2 % 2 > 0)
                {
                    //OddNumberException ONE = new OddNumberException();
                    //throw ONE;
                    throw new OddNumberException();
                }
                Result = Number1 / Number2;
                Console.WriteLine(Result);
            }
            catch (OddNumberException one)
            {
                Console.WriteLine($"Message: {one.Message}");
                Console.WriteLine($"HelpLink: {one.HelpLink}");
                Console.WriteLine($"Source: {one.Source}");
                Console.WriteLine($"StackTrace: {one.StackTrace}");
            }

            Console.WriteLine("End of the Program");
            Console.ReadKey();
        }
    }
}
```
Output:

![image_148.png](image_148.png)
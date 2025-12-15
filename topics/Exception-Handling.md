# Exception Handling

![image_287.png](image_287.png)

This is one of the most important concepts in C#. As a developer, handling an exception is your key responsibility while developing an application. Exception Handling is a procedure for handling an exception that occurs during the execution of a program. As part of this article, we will discuss the following pointers in detail.

## Types of Errors in C#
When we write and execute our code in the .NET framework, two types of error occurrences are possible. They are as follows:

* Compilation Errors
* Runtime Errors

## Compilation Error in C#
The error that occurs in a program during compilation is known as a compilation error (compile-time error). These **`errors occur due`** to `syntactical mistakes in the program`. These errors occur by typing the `wrong syntax`, like `missing double quotes in a string value` and `missing terminators in a statement`, `typing wrong spelling for keywords, assigning wrong data to a variable`, `trying to create an object for abstract class and interface`, etc. 

**`So, whenever we compile the program, the compiler recognizes these errors and shows us the list of errors`**.

## Runtime Error in C#
The errors that occur at the time of program execution are called runtime errors. **`These errors occur at runtime due to various reasons`**, such as when we are `entering the wrong data into a variable`, trying to `open a file for which there is no permission`, `trying to connect to the database with the wrong user ID and password`, the `wrong implementation of logic, and missing required resources`, etc. 

**`So, in simple words, we can say that the errors that come while running the program are called runtime errors.`**

### What is an Exception
An Exception is a class in C# that is responsible for abnormal program termination when runtime errors occur while running the program. These errors (runtime) are very dangerous because whenever they occur, the program terminates abnormally on the same line where the error occurs without executing the next line of code.

> Most people say Runtime Errors are Exceptions, which is not true. Exceptions are classes that are responsible for the abnormal termination of the program when runtime errors occur


#### Who is Responsible for the Abnormal Termination of the Program whenever Runtime Errors occur?

Objects of Exception classes are responsible for abnormal termination of the program whenever runtime errors occur. These exception classes are predefined under BCL (Base Class Libraries), where a separate class is provided for every different type of exception, like

* IndexOutOfRangeException
* FormatException
* NullReferenceException
* DivideByZeroException
* FileNotFoundException
* SQLException,
* OverFlowException, etc.

Each exception class provides a specific exception error message. All the above exception classes are responsible for abnormal program termination and will display an error message that specifies the reason for abnormal termination, i.e., they provide an error message specific to that error.

> Exception class is the superclass of all Exception classes in C#.

#### What happens if an Exception is Raised in the Program in C#?
When an Exception is raised in C#, the program execution is terminated abnormally. That means the statements placed after the exception-causing statements are not executed, but the statements placed before that exception-causing statement are executed by CLR.

The following example shows program execution with an exception. As you can see in the code below, we are dividing an integer number by 0, which is impossible in mathematics. So, it will throw the Divide By Zero Exception in this case. The statements present before the exception-causing statement, i.e., before c = a / b, are executed, and the statements present after the exception-causing statement will not be executed.

```C#
using System;
namespace ExceptionHandlingDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int a = 20;
            int b = 0;
            int c;
            Console.WriteLine("A VALUE = " + a);
            Console.WriteLine("B VALUE = " + b);
            c = a / b;
            Console.WriteLine("C VALUE = " + c);
            Console.ReadKey();
        }
    }
}
```

Output:

![image_135.png](image_135.png)

**After printing the above value, we will get the error below.**

![image_136.png](image_136.png)

The CLR terminates the program execution by throwing DivideByZeroException because the logical mistake we committed here is dividing an integer number by integer zero. As we know, dividing an integer number by zero is impossible. So, what CLR will do in this case, first it will check what type of logical error is this.

## How can we handle an Exception in .NET

There are two methods to handle the exception in .NET

* Logical Implementation
* Try Catch Implementation

### What is the Logical Implementation in C# to Handle Exception?
In a logical implementation, we need to handle exceptions using logical statements. In real-time programming, the first and foremost importance is always given to logical implementation only. If it is impossible to handle an exception using logical implementation, then we need to go for try-catch implementation.

#### Handling Exceptions in C# using Logical Implementation
The following example shows how to handle exceptions in C# using logical Implementation. Here, we are checking the second number, i.e., the variable Number2 value. If it equals 0, then we are printing one message saying the second number should not be zero. Otherwise, if the second number is not zero, then we are performing our division operation and showing the results on the console. Here, we are using an IF-ELSE logical statement to handle the exception.

```C#
using System;
namespace ExceptionHandlingDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int Number1, Number2, Result;
            Console.WriteLine("Enter First Number:");
            Number1 = int.Parse(Console.ReadLine());
            Console.WriteLine("Enter Second Number:");
            Number2 = int.Parse(Console.ReadLine());
            if (Number2 == 0)
            {
                Console.WriteLine("Second Number Should Not Be Zero");
            }
            else
            {
                Result = Number1 / Number2;
                Console.WriteLine($"Result = {Result}");
            }
            Console.ReadKey();
        }
    }
}
```

Output:

![image_137.png](image_137.png)

In the above case, when the user enters the second number, a zero exception will be raised, and that is handled using the logical implementation in C#. But while we are entering two numbers, if we enter any character instead of a number, then it will give you one exception, which is FormatException, which is not handled in this program, as shown below.

![image_138.png](image_138.png)

Here we entered the second value as “abc”. So, it will give us the below exception.

![image_139.png](image_139.png)

To handle such types of exceptions in C#, we need to implement a try-catch. The point that you need to remember is that if we are unable to handle the exception using logical implementation, we need to implement a **`try-catch`**.

### Exception handling in C# using Try Catch implementation
The .NET framework provides three keywords to implement the try-catch implementation. They are as follows:

* Try : The try keyword establishes a block in which we need to write the exception causing and its related statements. That means exception-causing statements and the related statements, which we should not execute when an exception occurs, must be placed in the try block.
* Catch : The catch block is used to catch the exception that is thrown from its corresponding try block. It has the logic to take necessary actions on that caught exception. The Catch block syntax in C# looks like a constructor. It does not take accessibility modifiers, normal modifiers, or return types. It takes only a single parameter of type Exception or any child class of the parent Exception class
* finally : The keyword finally establishes a block that definitely executes the statements placed in it irrespective of whether any exception has occurred or not. That means the statements that are placed in finally block are always going to be executed irrespective of whether any exception is thrown or not, irrespective of whether the thrown exception is handled by the catch block or not

### Syntax to use Exception Handling
The following image shows the syntax for using exception handling in C#. It starts with the try block, followed by the catch block, and writing the finally block is optional.

![image_140.png](image_140.png)

### Exception Handling in C# using Try-Catch Implementation with Exception Catch Block

In the below example, we have created a catch block that takes the Exception class as a parameter. Within the catch block, we print the exception information using the Exception class properties, i.e., Message, Source, StackTrace, and Helplink. As you can see in the below code, we are using the super Exception class.

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
                Result = Number1 / Number2;
                Console.WriteLine($"Result = {Result}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Message: {ex.Message}");
                Console.WriteLine($"Source: {ex.Source}");
                Console.WriteLine($"HelpLink: {ex.HelpLink}");
                Console.WriteLine($"StackTrace: {ex.StackTrace}");
            }
            Console.ReadKey();
        }
    }
}
```

**Output1:** Enter the value as 10 and 0

![image_141.png](image_141.png)

**Output2:** Enter the value as 10 and abc

![image_142.png](image_142.png)

### The Finally Block
The keyword finally establishes a block that definitely executes the statements placed in it irrespective of whether any exception has occurred or not. 

![image_143.png](image_143.png)

#### Example to Understand the use of finally block
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
                Console.WriteLine("Enter First Number");
                Number1 = int.Parse(Console.ReadLine());
                Console.WriteLine("Enter Second Number");
                Number2 = int.Parse(Console.ReadLine());
                Result = Number1 / Number2;
                Console.WriteLine($"Result: {Result}");
            }
            catch (DivideByZeroException DBZE)
            {
                Console.WriteLine("Second Number Should Not Be Zero");
            }
            catch (FormatException FE)
            {
                Console.WriteLine("Enter Only Integer Numbers");
            }
            finally
            {
                Console.WriteLine("Hello this is finally block...");
            }
            Console.ReadKey();
        }
    }
}
```
Output:

![image_144.png](image_144.png)
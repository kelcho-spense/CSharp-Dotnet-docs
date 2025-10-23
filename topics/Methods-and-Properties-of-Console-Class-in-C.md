# Methods and Properties of Console Class in C#

## What is Console Class in C#?

In order to implement the user interface in console applications, Microsoft provided us with a class called Console. The Console class is available in the System namespace. This Console class provides some methods and properties using which we can implement the user interface in a console application.

In order words, if we want to work with the console window either for taking user input or to show the output, we are provided with the Console in C#.

According to Microsoft documentation the Console class represents the standard input, output, and error streams for console applications and this class cannot be inherited because it is a static class i.e. declared as static as shown in the below image.

## Properties of Console Class in C#:

There are many properties available in the Console class. Some of them are as follows:

* **Title**: It gets or sets the title to display in the console title bar. It returns the string to be displayed in the title bar of the console. The maximum length of the title string is 24500 characters.
* **BackgroundColor**: It gets or sets the background color of the console. It returns a value that specifies the background color of the console; that is, the color that appears behind each character. The default is black.
* **ForegroundColor**: It gets or sets the foreground color of the console. It returns a ConsoleColor that specifies the foreground color of the console; that is, the color of each character that is displayed. The default is gray.
* **CursorSize**: It gets or sets the height of the cursor within a character cell. It returns the size of the cursor expressed as a percentage of the height of a character cell. The property value ranges from 1 to 100.

## Methods of Console Class in C#:

There are many methods available in the Console class. Some of them are as follows:

* **Clear()**: It is used to clear the console buffer and corresponding console window of display information. In simple words, it is used to clear the screen.
* **Beep()**: This method plays the sound of a beep through the console speaker. That means it plays a beep sound using a PC speaker at runtime.
* **ResetColor()**: This method is used to set the foreground and background console colors to their defaults.
* **Write(“string”)**: This method is used to write the specified string value to the standard output stream.
* **WriteLine(“string”)**: This method is used to write the specified string value, followed by the current line terminator, to the standard output stream. That means this method same as the write method but automatically moves the cursor to the next line after printing the message.
* **Write(variable)**: This method is used to write the value of the given variable to the standard output stream.
* **WriteLine(variable)**: This method is used to write the value of the given variable to the standard output stream along with moving the cursor to the next line after printing the value of the variable.
* **Read()**: This method read a single character from the keyboard and returns its ASCII value. The Datatype should be int as it returns the ASCII value.
* **ReadLine()**: This method reads a string value from the keyboard and returns the entered value only. As it returns the entered string value so the DataType is going to be a string.
* **ReadKey()**:  This method reads a single character from the keyboard and returns that character information like what key has been entered, and what its corresponding ASCII value is. The Datatype should be ConsoleKeyInfo which contains the entered key information

### Example to show the use of the Write and WriteLine method in C#:
```C#
//Program to show the use of WriteLine and Write Method
//First Import the System namespace as the
//Console class belongs to System namespace
using System;
namespace MyFirstProject
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //We can access WriteLine and Write method using class name
            //as these methods are static

            //WriteLine Method Print the value and move the cursor to the next line
            Console.WriteLine("Hello");
            //Write Method Print the value and stay in the same line
            Console.Write("HI ");
            //Write Method Print the value and stay in the same line
            Console.Write("Bye ");
            //WriteLine Method Print the value and move the cursor to the next line
            Console.WriteLine("Welcome");
            //Write Method Print the value and stay in the same line
            Console.Write("C#.NET ");
            Console.ReadKey();
        }
    }
}
```
Output:
![image_26.png](image_26.png)

### Example to show how to print the value of a variable in C#.

```C#
//Program to show how to print the value of a variable 
using System;
namespace MyFirstProject
{
    internal class Program
    {
        static void Main(string[] args)
        {
            string name = "Pranaya";
            Console.WriteLine(name);
            Console.WriteLine("Hello " + name);
            Console.Write($"Hello {name}");
            Console.ReadKey();
        }
    }
}
```
Output:
![image_27.png](image_27.png)

### Reading Value from the user in C#:

```C#
//Program to show how to read value at runtime
using System;
namespace MyFirstProject
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //Giving one message to the user to enter his name
            Console.WriteLine("Enter Your Name");

            //ReadLine method reads a string value from the keyboard 
            string name = Console.ReadLine();

            //Printing the entered string in the console
            Console.WriteLine($"Hello {name}");
            Console.ReadKey();
        }
    }
}
```
Output:
![image_28.png](image_28.png)

### How do you Read Integer Numbers in C# from the keyword?
Whenever we entered anything whether a string or numeric value from the keyword using the ReadLine method, the input stream is taking it as a string. So, we can directly store the input values in a string variable. If you want to store the input values in integer variables, then we need to convert the string values to integer values.

```C#
//Program to show how to read integer values
using System;
namespace MyFirstProject
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Eneter two Numbers:");

            //Converting string to Integer
            int Number1 = Convert.ToInt32(Console.ReadLine());

            //Converting string to Integer
            int Number2 = Convert.ToInt32(Console.ReadLine());

            int Result = Number1 + Number2;
            Console.WriteLine($"The Sum is: {Result}");
            Console.WriteLine($"The Sum is: {Number1 + Number2}");
            Console.ReadKey();
        }
    }
}
```

Output:
![image_29.png](image_29.png)

### Example to Show to Student Mark using Console Class Methods:
Write a Program to enter the Student Registration Number, Name, Mark1, Mark2, Mark3, and calculate the total mark and average marks and then print the student details in the console.

```C#
using System;
namespace MyFirstProject
{
    internal class Program
    {
        static void Main(string[] args)
        {
            //Ask the user to Enter Student Details
            Console.WriteLine("Enter Student Details");
            Console.WriteLine("Enter Registration Number");
            int RegdNumber = Convert.ToInt32(Console.ReadLine());
            Console.WriteLine("Enter Name");
            string Name = Console.ReadLine();
            Console.WriteLine("Enter Marks of three Subjects:");
            Console.WriteLine("Subject1");
            int Mark1 = Convert.ToInt32(Console.ReadLine());
            Console.WriteLine("Subject2");
            int Mark2 = Convert.ToInt32(Console.ReadLine());
            Console.WriteLine("Subject3");
            int Mark3 = Convert.ToInt32(Console.ReadLine());

            int TotalMarks = Mark1 + Mark2 + Mark3;
            int AverageMark = TotalMarks / 3;

            //Display the Student Details
            Console.WriteLine("\nStudent Details are as Follows:");
            Console.WriteLine($"Registration Number: {RegdNumber}");
            Console.WriteLine($"Name: {Name}");
            Console.WriteLine($"Total Marks : {TotalMarks}");
            Console.WriteLine($"Average Mark: {AverageMark}");
            Console.ReadKey();
        }
    }
}
```
Output:
![image_30.png](image_30.png)


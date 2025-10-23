# C#.NET Basics

## Basic Structure of C# Program
Now, let’s understand the Basic Structure of the C# Program using a Console Application.

![image_21.png](image_21.png)

The above process is shown in the below diagram.
![image_22.png](image_22.png)

> Note: C#.NET is a Case-Sensitive Language and Every Statement in C# should end with a Semicolon.

![image_23.png](image_23.png)

A project called **MYFirstProject** will be created in Visual Studio. This project will contain all the necessary required files (Properties, References, App.Config files) to run the Console application. The Main program called **Program.cs** is the default code file that is created when a new console application is created in Visual Studio it contains the Main method by default and from that Main method our application is going to start its execution, but if you want you can also change this default behavior. So, if you look at the **Program.cs** class file, then you will see the following code.
![image_24.png](image_24.png)

## Understanding the Code:

Using Visual Studio, if we are creating a console application (excluding .NET 6), then automatically we are getting four sections which are shown in the below image.

![image_25.png](image_25.png)

#### Importing Namespace Section:

This section contains importing statements that are used to import the BCL (Base Class Libraries) as well as user-defined namespaces if required. This is similar to the included statements in the C programming language. Suppose you want to use some classes and interfaces in your code, then you have to include the namespace(s) from where these classes and interfaces are defined. For example, if you are going to use the Console class in your code, then you have to include the System namespace as the Console class belongs to the System namespace.

```c#
# Syntax: using NamespaceName;
# Example:
using System;
 ```

If the required namespace is a member of another namespace, we have to specify the parent and child namespaces separated by a dot as follows:
```c#
using System.Data;
using System.IO;
 ```
> Note: A namespace is a container that contains a group of related classes and interfaces, as well as, a namespace can also contain other namespaces.

#### Namespace Declaration Section:

Here a user-defined namespace is declared. In .NET applications, all classes and interfaces or any type related to the project should be declared inside some namespace. Generally, we put all the related classes under one namespace and in a project, we can create multiple namespaces.
```c#
# Syntax: namespace NamespaceName {}
# Example:
namespace MyFirstProject {}
 ```

Generally, the namespace name will be the same as the project name but it is not mandatory, you can give any user-defined name to the namespace.

#### Class Declaration Section:

For every Desktop Application in .NET, we need a start-up class file. For example, for every .NET Desktop Application like Console and Windows, there should be a Start-Up class that should have the Main method from where the program execution is going to start. When we create a Console Application using Visual Studio, by default, Visual Studio will Create the Start-Up class file with the name **Program.cs**

```C#
# Syntax:
class ClassName
{
}

# Example:
class Program
{
}
```

> Note: You can change this default behavior. You can create your own class and you can also make that class as the Start-Up Class by including the Main method. In our upcoming articles, we will discuss this in detail.

#### Main() Method Section:

The main() method is the entry point or starting point of the application to start its execution. When the application starts executing, the main method will be the first block of the application to be executed. The Main method contains the main logic of the application.

#### What is using?

Using is a keyword. Using this keyword, we can refer to .NET BCL in C# Applications i.e. including the BCL namespaces as well as we can also include user-defined namespaces that we will discuss as we progress in this course. Apart from importing the namespace, other uses of using statements are there, which we will also discuss as progress in this course. For now, it is enough.
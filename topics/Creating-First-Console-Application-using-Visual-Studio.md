# Creating First Console Application using Visual Studio

A console application is an application that can be run in the command prompt in Windows. For any beginner on .Net, building a console application is ideally the first step to learning the C# Language.

#### Step1

The first step involves the creation of a new project in Visual Studio. For that, open Visual Studio 2022 (the latest version at this point in time) and then click on the Create a New Project option as shown in the below image.
![image_14.png](image_14.png)

#### Step2

The next step is to choose the project type as a Console application. Type Console in the search bar and you can see different types of console applications using C#, VB, and F# languages using both .NET Framework and Core Framework. Here, I am selecting Console App (.NET Framework) using C# language and then clicking on the Next button as shown in the below image.
![image_15.png](image_15.png)

#### Step3

The next step is to configure the new project. Here, you need to provide the project name and solution name. You can also give the same name to both project and solution but it is not mandatory. You need to provide the location where you need to create the project. Here, you also need to provide the .NET Framework version you want to use. The latest version of the .NET Framework is 8.0 So, I am selecting .NET Framework 8.0 and then clicking on the Create button as shown in the below image.
![image_16.png](image_16.png)

Once you click on the Create button, visual studio will create the Console Application with the following structure.
![image_17.png](image_17.png)

A project called MYFirstProject will be created in Visual Studio. This project will contain all the necessary required files to run the Console application. The Main program called Program.cs is the default code file that is created when a new console application is created in Visual Studio. This Program.cs class will contain the necessary code for our console application. So, if you look at the Program.cs class file, then you will see the following code.
![image_18.png](image_18.png)

#### Step4

Now let’s write our code which will be used to display the message “Hello World” in the console application. 

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace MyFirstProject
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
            Console.ReadKey();
        }
    }
}
```

#### Step5

The next step is to run the .Net program. To run any program in Visual Studio, you need to click the Start button as shown in the below image.
![image_19.png](image_19.png)

Once you click on the Start, you should get the following console window showing the message.
![image_20.png](image_20.png)

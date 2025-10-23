# Introduction & Environment Setup



## What does .NET Represent?

**NET** stands for **Network Enabled Technology** (Internet). In .NET, dot (.) refers to **Object-Oriented**, and NET refers to the internet. So, the complete .NET means through Object-Oriented we can implement internet-based applications.

According to Microsoft, **.NET** is a **Free**, **Cross-Platform**, **Open-Source** developer platform for building many different types of applications. With .NET, we can use multiple languages (C#, VB, F#, etc.), Editors (Visual Studio, Visual Studio Code, Visual Studio for Mac, OmniSharp, JetBrains Rider, etc), and Libraries to build for Web, Mobile, Desktop, Games, IoT, and more.

**Cross Platform**: Whether you are working in C#, F#, or Visual Basic, your code will run on any compatible operating system. You can build many types of apps with .NET. Some are Cross-Platform, and some target a specific set of operating systems and devices.

**Libraries**: To extend functionality, Microsoft and others maintain a healthy .NET package ecosystem. [NuGet](https://www.nuget.org/) is a package manager built specifically for .NET that contains over 100,000 packages.

## What is a Framework?

A framework is a software. Or you can say a framework is a collection of many small technologies integrated together to develop applications that can be executed anywhere.

#### What does the .NET Framework Provide?

The DOT NET Framework provides two things as follows

1. **BCL** (Base Class Libraries)
2. **CLR** (Common Language Runtime

#### What is BCL?

Base Class Libraries (BCL) are designed by Microsoft. Without BCL we can’t write any code in .NET. So, BCL is also known as the basic building block of .NET Programs. These are installed into the machine when we installed the .NET framework. BCL contains pre-defined classes and these classes are used for the purpose of application development.
The physical location of BCL is **C:\Windows\assembly**

#### What is CLR?

CLR stands for Common Language Runtime and it is the core component under the .NET framework which is responsible for converting the MSIL (Microsoft Intermediate Language) code into native code. In our CLR session, we will discuss CLR in detail.

![image.png](image.png)

In the .NET framework, the code is compiled twice.

![image_2.png](image_2.png)

* In the 1st compilation, the source code is compiled by the respective language compiler and generates the intermediate code which is known as **MSIL (Microsoft Intermediate Language)** or **CIL (Common Intermediate language code)**, or Managed Code.
* In the 2nd compilation, **MSIL** is converted into **Native code/ Machine Code** (native code means code specific to the Operating system so that the code is executed by the Operating System) and this is done by **CLR**.

_Always **1st compilation is slow** and **2nd compilation is fast**._

#### What is JIT?

JIT stands for the **Just-in-Time** compiler. It is the component of **CLR** that is responsible for _converting MSIL code into Native Code_. Native code is code that is directly understandable by the operating system.

### Different types of .NET Framework

The .NET framework is available in many different flavors, but below are the common ones

* .NET Framework: .NET Framework is the original implementation of .NET. It supports running websites, services, desktop applications, and more on Windows OS Only.
* .NET: .NET is a cross-platform implementation for running websites, services, and console applications on Windows, Linux, and macOS. .NET is open source on GitHub and .NET was previously called .NET Core.
* Xamarin/Mono: Xamarin/Mono is a .NET implementation for running apps on all the major mobile operating systems, including iOS and Android.

> Note: **.NET Framework** is **Platform-Dependent** while **.NET** or **.NET Core** is **Platform Independent**. Here, we are not talking about Web Applications. Web Applications are independent of Operating Systems.

What is Exactly .NET?
.NET is a framework tool that supports many programming languages and many technologies. .NET support 60+ programming languages. Of 60+ programming languages, 9 are designed by Microsoft and the remaining are designed by non-Microsoft. Microsoft-designed programming languages are as follows:

1. VB.NET
2. **C#.NET**
3. VC++.NET
4. J#.NET
5. F#.NET
6. Jscript.NET
7. WindowsPowerShell
8. Iron phyton
9. Iron Ruby

Technologies supported by the .NET framework are as follows

1. **ASP.NET** (Active Server Pages.NET) – MVC, **Web API**, Core MVC, Core Web API, Core Blazor, etc.
2. ADO.NET (Active Data Object.NET)
3. WCF (Windows Communication Foundation)
4. WPF (Windows Presentation Foundation)
5. WWF (Windows Workflow Foundation)
6. AJAX (Asynchronous JavaScript and XML)
7. **LINQ** (Language Integrated Query) : It is a query language
8. **Entity Framework** : It is an ORM-based open-source framework that is used to work with a database using .NET objects.


<seealso>

<!--List any additional resources, such as tutorials or guides, that can help users understand and use the API effectively.-->

</seealso>

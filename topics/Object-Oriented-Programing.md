# Object-Oriented Programing

![image_286.png](image_286.png)

In this article, I will give an overview of Object-Oriented Programming (OOPs) in C#, i.e., discuss the OOPs Principles in C#. Object-Oriented Programming, commonly known as OOPs, is a technique, not a technology. It means it doesn’t provide any syntaxes or APIs; instead, it provides suggestions to design and develop objects in programming languages. As part of this article, we will cover the following OOP concepts in C#.

## How do we Develop Applications?

Object-Oriented Programming is a strategy that provides some principles for developing applications or software. It is a methodology. Like OOPs, other methodologies exist, such as Structured Programming, Procedural Programming, or Modular Programming. But nowadays, one of the well-known and famous styles is Object Orientation, i.e., Object-Oriented Programming

Nowadays, almost all the latest programming languages support object orientation. This object orientation is more related to the designing of software, and this deals with the internal design of the software, not the external design. So, it is nowhere related to the users of the software. It is related to the programmers who are working on developing software.

## Object-Oriented vs Modular Programming

Now, I will explain to you Object Orientation by comparing it with Modular Programming. The reason is that people who came to learn C# already know the C language. The C programming language supports Modular or Procedural Programming. Based on that, I can give you an idea of how object orientation differs from modular programming. Let us compare Object-Oriented vs Modular Programming through some examples.

### Bank Example
So first, we are taking an example of a bank. If you’re developing an application for a bank using modular programming, how do you see the system, how do you see the workings of a bank, and what will be your design? That depends on how you understand it and how you see the system. So, let us see how we look at the bank system using modular programming.

In a bank, you can _open an account_, you can _deposit an amount_, you can _withdraw an amount_, you can _check your account balance_, or you can also _apply for a loan_, and so on. So, these are the things that you can do at the bank.

So, **Opening an Account**, **Depositing Money**, **Withdrawing Money**, **Checking Your Balance**, and **Applying For a Loan** are functions. All these are nothing but functions. And you can do the specific operations by calling that specific function. So, if you’re developing software for a bank, it is nothing but a collection of functions. So, the bank application will be based on these functions, and the user of the bank application will be utilizing these functions to perform his required task. So, you will develop software as a set of functions in Modular Programming

### Why Object Orientation?
Let us talk about a manufacturing firm which manufactures cars or vehicles. If you look at that manufacturing farm, then it may be working in the form of departments like one is an inventory department that maintains the stock of raw materials and one is manufacturing, which is the production work that they do, and one department will be looking at sales and one department is looking at marketing. One is about payroll, and one is for accounts, and so on. So, there may be many departments.

Suppose you are developing software only for _payroll_ or _inventory purposes_. In that case, you may look at the system just like a _modular approach_, and in that, you can find functions like _placing an order_ and _checking the item in stock_. These types of things can have a set of functions so that you can develop the software only for the inventory system as a collection of functions. Still, **when developing software for the entire organization, you must see things in objects**.

> So, the **inventory item is an object**, an **employee is an object**, an **account is an object**, and a **product manufacturer is an object**. The **machines used for production are an object**. So, all these things are objects.

### What are the Problems of Modular Programming?

Modular programming has the following problems.

![image_84.png](image_84.png)

* Reusability:  In Modular Programming, we must write the same code or logic at multiple places, increasing code duplication. Later, if we want to change the logic, we must change it everywhere.
* Extensibility: It is not possible in modular programming to extend the features of a function. Suppose you have a function and you want to extend it with some additional features; then it is not possible. You have to create an entirely new function and then change the function as per your requirement.
* Simplicity: As extensibility and reusability are impossible in Modular Programming, we usually end up with many functions and scattered code.
* Maintainability: As we don’t have Reusability, Extensibility, and Simplicity in modular Programming, it is very difficult to manage and maintain the application code.

### How Can We Overcome Modular Programming Problems?
We can overcome the modular programming problems _(Reusability, Extensibility, Simplicity, and Maintainability)_ using Object-Oriented Programming. OOPs provide some principles, and using those principles, we can overcome Modular Programming Problems

## What Is Object-Oriented Programming?
Let us understand Object-Oriented Programming, i.e., OOP concepts using C#. Object-oriented programming (OOPs) in C# is a design approach where we think in terms of real-world objects rather than functions or methods. Unlike procedural programming language, in OOPs, programs are organized around objects and data rather than action and logic. Please have a look at the following diagram to understand this better.

![image_85.png](image_85.png)

* Reusability:
To address reusability, object-oriented programming provides something called Classes and Objects. So, rather than copy-pasting the same code repeatedly in different places, you can create a class and make an instance of the class, which is called an object, and reuse it whenever you want.

* Extensibility:
Suppose you have a function and want to extend it with some new features that were impossible with functional programming. You have to create an entirely new function and then change the whole function to whatever you want. OOPs, this problem is addressed using concepts called Inheritance, Aggregation, and Composition. In our upcoming article, we will discuss all these concepts in detail.

* Simplicity:
Because we don’t have extensibility and reusability in modular programming, we end up with lots of functions and scattered code, and from anywhere we can access the functions, security is less. In OOPs, this problem is addressed using Abstraction, Encapsulation, and Polymorphism concepts.

* Maintainability:
As OOPs address Reusability, Extensibility, and Simplicity, we have good, maintainable, and clean code, increasing the application’s maintainability.

### What are the OOPs Principles or OOPs Concepts?
OOPs provide 4 principles. They are : 

* Abstraction:
  The process of representing the essential features without including the background details. In simple words, we can say that it is a process of defining a class by providing necessary details to the external world, which are required by hiding or removing unnecessary things.
* Inheritance: The process by which the members of one class are transferred to another class. The class from which the members are transferred is called the Parent/Base/Superclass, and the class that inherits the Parent/Base/Superclass members is called the Derived/Child/Subclass. We can achieve code _extensibility_ through inheritance.
* Polymorphism: is derived from the Greek word, where Poly means many and morph means faces/ behaviors. So, the word polymorphism means the ability to take more than one form. Technically, we can say that the same function/operator will show different behaviors by taking different types of values or with a different number of values.
* Encapsulation : The process of binding the data and functions together into a single unit (i.e., class). In simple words, we can say that it is a process of defining a class by hiding its internal data members from outside the class and accessing those internal data members only through publicly exposed methods or properties.

> 🤣 Note: Don’t consider Class and Objects as OOPs principle. We use classes and objects to implement OOP Principles.
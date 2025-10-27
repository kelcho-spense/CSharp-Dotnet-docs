# Class &amp; Objects

As we already discussed in our previous article, class, and objects addresses the reusability functionality. Again we discussed in Object-Oriented Programming, we need to think in terms of objects rather than functions. So, let us discuss what exactly classes and objects are from the Layman point of view as well as from the programming point of view.

**Class:**
A class is simply a _user-defined data type_ that represents both _state_ and _behavior_. The _state represents the properties_ and _behavior is the action_ that objects can perform. In other words, we can say that a class is the blueprint/plan/template that describes the details of an object. A class is a blueprint from which the individual objects are created. In C#, a Class is composed of three things i.e. a name, attributes, and operations.

**Objects:**
It is an instance of a class. A class is brought live by creating objects. An object can be considered as a thing that can perform activities. The set of activities that the object performs defines the object’s behavior. All the members of a class can be accessed through the object. To access the class members, we need to use the dot (.) operator. The dot operator links the name of an object with the name of a member of a class.

## How can we create a Class and Object in C#?
Let us understand how to create class and object in C#. In order to understand this, please have a look at the following image. As you can see in the below image, a class definition starts with the keyword class followed by the class name (here the class name is Calculator), and the class body is enclosed by a pair of curly braces. As part of the class body, you define class members (properties, methods, variables, etc.). Here as part of the body, we define one method called CalculateSum. The class Calculator is just a template. In order to use this class or template, you need an object. As you can see in the second part of the image, we create an object of the class Calculator using the new keyword. And then store the object reference on the variable calObject which is of type Calculator. Now, using this calObject object we can access the class members using a dot.

![image_86.png](image_86.png)

So, the point that you need to remember is, to create a class you need to use the class keyword while if you want to create an object of a class then you need to use the new keyword. Once you create the object then you can access the class members using the object.

```C#
using System;
namespace ClassObjectsDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating object
            Calculator calObject = new Calculator();

            //Accessing Calculator class member using Calculator class object
            int result = calObject.CalculateSum(10, 20);

            Console.WriteLine(result);
            Console.ReadKey();
        }
    }

    //Defining class or blueprint or template
    public class Calculator
    {
        public int CalculateSum(int no1, int no2)
        {
            return no1 + no2;
        }
    }
}
```
**Output**: 30

## Difference between Class and Objects in C#
Many programmers or developers still get confused by the difference between class and object. As we already discussed, in object-oriented programming, a Class is a template or blueprint for creating Objects, and every Object in C# must belong to a Class. Please have a look at the following diagram to understand the difference between them.

![image_87.png](image_87.png)

As you can see in the above image, here we have one class called “Employee”. All the Employees have some properties such as employee id, name, salary, gender, department, etc. These properties are nothing but the attributes (properties or fields) of the Employee class.

If required you can also add some methods (functions) that are common to all Employees such as InsertData and DisplayData to insert and display the Employee Data.

So, the idea is that the template or blueprint of the Employee is not going to change. Each and every Object is going to build from the same template (Class) and therefore contains the same set of methods and properties. Here, all Objects share the same template but maintain a separate copy of the member data (Properties or fields).

### Types of Classes in C#:
![image_88.png](image_88.png)
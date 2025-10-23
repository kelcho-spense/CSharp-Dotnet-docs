# Variables in C#

Whenever we are processing the data or information, the data or information must be at some location. And we call that location a Memory Location. Every computer has memory locations, and every memory location is identified by an address. Just consider in a movie hall, the seating arrangement as memory locations.

So, every memory location in the computer is identified by an address. For a better understanding, please have a look at the below image. As you can see in the below image, 128, 572, 1024, 5098, etc. are one-one memory addresses. We can treat all the addresses are positive integer values.
![image_34.png](image_34.png)

## What is the relation between variable and memory locations?

Suppose I want to store a value of 10 in the computer memory locations. Just consider a classroom, there is no restriction on the students where they can sit. That means the students will go and sit randomly at different locations. In the same way, the value 10 that we want to store in the computer memory locations will also go and be stored randomly at a particular memory location. For a better understanding, please have a look at the below image.
![image_35.png](image_35.png)

## How we can set Identity to Memory Locations?

We can set the identity of the memory location by using variables or you can say identifiers. The following is the syntax to declare a variable by setting the identity of the memory location in the C# language. First, we need to write the data type followed by the identifier.
```C#
## Syntax: data_type Identifier;
## Example: 
int a; 
/* Here int is the data type and the identifier can be any name and here we set it as a. 
So, whenever we declare a variable, it gets memory allocated. 
To one memory location, the identity is set as shown in the below image.
*/
```

![image_36.png](image_36.png)

Here “a” is a named memory location to location 10344. Later we can store an element in that memory location that is identified by the identifier “a” as follows.

![image_37.png](image_37.png)

## What is a Variable in C# Language?

>> A name that is given for any computer memory location is called a variable. The purpose of the variable is to provide some name to a memory location where we store some data. The user will access the data by the variable name and the compiler will access the data by the memory address. So, the variable is a named location in the computer memory where a program can store the data. 

### Rules for variable declaration in C#:

1. A variable name must begin with a `letter` or `underscore`.
2. Variables in C# are `case sensitive`
3. They can be constructed with `digits and letters`.
4. `No special symbols` are allowed other than `underscores`.
5. `sum`, `Height`, `_value`, and `abc123`, etc. are some examples of the variable name

## How to declare a variable in C#?

```C#
## Syntax: 
data_type variable_name;

## Example: 
int age;
```

Here, data_type is the type of data to be stored in the variable, and variable_name is the name given to that variable.

## How to initialize a Variable in C#?

The Syntax for initializing a variable in C# is as follows:

```C#
## Syntax: 
data_type variable_name = value;

## Example: 
int age = 20;
```
Here, data_type is the type of data to be stored in the variable, variable_name is the name given to the variable and value is the initial value stored in the variable.

## Types of Variables in a Class in C#:
let us understand the different kinds of variables a class can have and their behavior. Basically, there are four types of variables that we can declare inside a class in C#. They are as follows:

1. Non-Static/Instance Variable
2. Static Variable
3. Constant Variable
4. Readonly Variable

### Static and Non-Static Variables in C#

> If we declare a variable explicitly by using the static modifier, we call it a static variable, and the rest of all are non-static variables.

> Again, if we declare a variable inside a static block, then also that variable is a static variable.

> And if we declare a variable inside a non-static block, then that becomes a non-static variable. 

For a better understanding, please have a look at the following example. In the below example, we have declared three variables. 

```C#
using System;
namespace TypesOfVariables
{
    internal class Program
    {
        static int x; //Static Variable
        int y; //Non-Static or Instance Variable
        static void Main(string[] args)
        {
            int z; //Static Variable
        }
    }
}
```

> Now, let us try to print the value of x and y inside the Main method. Let us initialize the x value to 100 and the y value to 200. Here, you can print the value of x directly inside the Main method. But you cannot print the value of y directly inside the Main method.

```C#
using System;
namespace TypesOfVariables
{
    internal class Program
    {
        static int x = 100; //Static Variable
        int y = 200; //Non-Static or Instance Variable
        static void Main(string[] args)
        {
            Console.WriteLine($"x value: {x}");
            Console.Read();
        }
    }
}
```

```Plain Text
Output: x value: 100
```

Now, let us try to print the y value also directly. If we try to print the y value directly, then we will get a compile-time error saying `an object reference is required for the non-static field, method, or property ‘Program.y’.`

```C#
using System;
namespace TypesOfVariables
{
    internal class Program
    {
        static int x = 100; //Static Variable
        int y = 200; //Non-Static or Instance Variable
        static void Main(string[] args)
        {
            Console.WriteLine($"x value: {x}");
            Console.WriteLine($"x value: {y}");
            Console.Read();
        }
    }
}
```

When you try to run the above code, you will get the following Compile Time Error.

![image_38.png](image_38.png)

> This is because the memory for the variable y is going to be created only when we create an instance of the class Program and for each instance. 

> But x does not require an instance of the class. The reason is a static variable is initialized immediately once the execution of the class starts.

> So, until and unless we created the instance of the Program class, the memory will not be allocated for the variable y and as long as the memory is not allocated for the variable y, we cannot access it. So, once we create the instance of the Program class, the memory for variable y will be allocated, and then only we can access the variable y.

In the below example, we are creating an instance of the Program class, and using that instance we are accessing the y variable. But we are accessing directly the x variable.

```C#
using System;
namespace TypesOfVariables
{
    internal class Program
    {
        static int x = 100; //Static Variable
        int y = 200; //Non-Static or Instance Variable
        static void Main(string[] args)
        {
            Console.WriteLine($"x value: {x}");
            Program obj = new Program();
            Console.WriteLine($"y value: {obj.y}");
            Console.Read();
        }
    }
}
```

output : 

```Plain Text
x value: 100
y value: 200
```

### Difference Between Static and Non-Static Variables in C#
* In the case of an Instance Variable, each object will have its own copy whereas We can only have one copy of a static variable irrespective of how many objects we create.
* In C#, the Changes made to the instance variable using one object will not be reflected in other objects as each object has its own copy of the instance variable. In the case of static variables, changes made in one object will be reflected in other objects as static variables are common to all objects of a class.
* We can access the instance variables through object references whereas the Static Variables can be accessed directly by using the class name in C#.
* In the life cycle of a class, a static variable is initialized only once, whereas instance variables are initialized for 0 times if no instance is created and n times if n number of instances are created.

#### Instance/Non-Static Variables in C#
* Scope of Instance Variable: Throughout the class except in static methods.
* The lifetime of Instance Variable: Until the object is available in the memory.
#### Static Variables in C#
* Scope of the Static Variable: Throughout the class.
* The Lifetime of Static Variable: Until the end of the program.

### Constant Variables in C#:

In C#, if we declare a variable by using the const keyword, then it is a constant variable and the value of the constant variable can’t be modified once after its declaration. 

> So, it is mandatory to initialize the constant variable at the time of its declaration only.

Suppose, you want to declare a constant PI in your program, then you can declare the constant as follows:

```C#
const float PI = 3.14f;
```

> If you are not initializing the const variable at the time of its declaration, then you get a compiler error as shown in the below image.

![image_39.png](image_39.png)

As you can see it saying a const field requires a value to be provided which means while declaring `a constant it is mandatory to initialize the constant variable.`

```C#
using System;
namespace TypesOfVariables
{
    internal class Program
    {
        const float PI = 3.14f; //Constant Variable
        static int x = 100; //Static Variable
        //We are going to initialize variable y through constructor
        int y; //Non-Static or Instance Variable

        //Constructor
        public Program(int a)
        {
            //Initializing non-static variable
            y = a; 
        }

        static void Main(string[] args)
        {
            //Accessing the static variable without instance
            Console.WriteLine($"x value: {x}");
            //Accessing the constant variable without instance
            Console.WriteLine($"PI value: {PI}");

            Program obj1 = new Program(300);
            Program obj2 = new Program(400);
            //Accessing Non-Static variable using instance
            Console.WriteLine($"obj1 y value: {obj1.y}");
            Console.WriteLine($"obj2 y value: {obj2.y}");
            Console.Read();
        }
    }
}
```

Output:

```Plain Text
x value: 100
PI value: 3.14
obj1 y value: 300
obj2 y value: 400
```

The following diagram shows the memory representation of the above example.

![image_40.png](image_40.png)

Now, you might have one question, if both static and constant are behaving in the same way, then what are the differences between them?

### Read-only Variable in C#

When we declare a variable by using the `**readonly**` keyword, then it is known as a read-only variable and these variables can’t be modified like constants but after initialization. 
> That means it is not mandatory to initialize a read-only variable at the time of its declaration, 
they can also be initialized under the constructor. 

> That means we can modify the read-only variable value only within a constructor.

##### Example to Understand Read-Only Variables in C#:

In the below example, the read-only variable z is not initialized with any value but when we print the value of the variable, the default value of int i.e. 0 will be displayed.

```C#
using System;
namespace TypesOfVariables
{
    internal class Program
    {
        const float PI = 3.14f; //Constant Variable
        static int x = 100; //Static Variable
        //We are going to initialize variable y through constructor
        int y; //Non-Static or Instance Variable
        readonly int z; //Readonly Variable

        //Constructor
        public Program(int a)
        {
            //Initializing non-static variable
            y = a; 
        }

        static void Main(string[] args)
        {
            //Accessing the static variable without instance
            Console.WriteLine($"x value: {x}");
            //Accessing the constant variable without instance
            Console.WriteLine($"PI value: {PI}");

            Program obj1 = new Program(300);
            Program obj2 = new Program(400);
            //Accessing Non-Static variable using instance
            Console.WriteLine($"obj1 y value: {obj1.y} and Readonly z value: {obj1.z}");
            Console.WriteLine($"obj2 y value: {obj2.y} and Readonly z value: {obj2.z}");
            Console.Read();
        }
    }
}
```

Output:

```Plain Text
x value: 100
PI value: 3.14
obj1 y value: 300 and Readonly z value: 0
obj2 y value: 400 and Readonly z value: 0
```

In the below example, we are initializing the readonly variable through the class constructor. Now, the constructor takes two parameters. The first parameter will initialize the non-static variable and the second parameter will initialize the readonly variable. So, while creating the instance, we need to pass two integer values to the constructor function.

```C#
using System;
namespace TypesOfVariables
{
    internal class Program
    {
        const float PI = 3.14f; //Constant Variable
        static int x = 100; //Static Variable
        //We are going to initialize variable y through constructor
        int y; //Non-Static or Instance Variable
        readonly int z; //Readonly Variable

        //Constructor
        public Program(int a, int b)
        {
            //Initializing non-static variable
            y = a;
            //Initializing Readonly variable
            z = b;
        }

        static void Main(string[] args)
        {
            //Accessing the static variable without instance
            Console.WriteLine($"x value: {x}");
            //Accessing the constant variable without instance
            Console.WriteLine($"PI value: {PI}");

            Program obj1 = new Program(300, 45);
            Program obj2 = new Program(400, 55);
            //Accessing Non-Static variable using instance
            Console.WriteLine($"obj1 y value: {obj1.y} and Readonly z value: {obj1.z}");
            Console.WriteLine($"obj2 y value: {obj2.y} and Readonly z value: {obj2.z}");
            Console.Read();
        }
    }
}
```

Output:

```Plain Text
x value: 100
PI value: 3.14
obj1 y value: 300 and Readonly z value: 45
obj2 y value: 400 and Readonly z value: 55
```

![image_41.png](image_41.png)

##### Difference Between Non-Static and Readonly in C#:

> The only difference between a non-static and readonly variable is that after initialization, you can modify the non-static variable value but you cannot modify the readonly variable value.

In the below example, after creating the first instance we are trying to modify the non-static y and readonly z variable value.

```C#
using System;
namespace TypesOfVariables
{
    internal class Program
    {
        const float PI = 3.14f; //Constant Variable
        static int x = 100; //Static Variable
        //We are going to initialize variable y through constructor
        int y; //Non-Static or Instance Variable
        readonly int z; //Readonly Variable

        //Constructor
        public Program(int a, int b)
        {
            //Initializing non-static variable
            y = a;
            //Initializing Readonly variable
            z = b;
        }

        static void Main(string[] args)
        {
            //Accessing the static variable without instance
            Console.WriteLine($"x value: {x}");
            //Accessing the constant variable without instance
            Console.WriteLine($"PI value: {PI}");

            Program obj1 = new Program(300, 45);
            //Accessing Non-Static variable using instance
            Console.WriteLine($"obj1 y value: {obj1.y} and Readonly z value: {obj1.z}");

            obj1.y = 500; //Modifying Non-Static Variable
            obj1.z = 400; //Trying to Modify Readonly Variable, Getting Error

            Console.Read();
        }
    }
}
```

When you try to execute the above code, you will get the following Compilation Error.

![image_42.png](image_42.png)

> A readonly field cannot be assigned to (except in a constructor or init-only setter of the type in which the field is defined or a variable initializer).

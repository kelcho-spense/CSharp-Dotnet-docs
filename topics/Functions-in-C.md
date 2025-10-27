# Functions in C#

## What are the Functions of C#
A function is a group of related instructions that performs a specific task. It can be a small or big task, but the function will perform that task completely. Functions take some input as parameters and return the result as a return value.

## Why do we need functions?
Let us understand why we need functions with an example. Functions are also called modules or procedures. Instead of writing a single main program, i.e., everything inside the main function, we can break the main function into small manageable size pieces and separate the repeating tasks or smaller tasks as a function.

**For Example**, if we write one program and put everything inside the main function, then such a type of programming approach is called Monolithic Programming. If your main function contains thousands of lines of code, then it is becoming very difficult to manage. This is actually not a good programming approach.

![image_80.png](image_80.png)

## Problems in Monolithic Programming:
1. **First problem**: if there is an error in a single line, it’s an error in the entire program or main function.
2. **Second problem**: 10000 lines of code we cannot finish in one hour or one day; it might take a few days, and we should remember everything throughout that time. Then only we can make changes or write new lines in the program. So, we should memorize the whole program.
3. **Third problem**: How many people can write this one single main function? Only one person can write. We cannot make it a teamwork and more than one person can’t work on the same main function. So, work cannot be distributed in a team.
4. **Fourth problem**: when this program becomes very big, it may fit in some computer memories, and it may not fit in some of the memories. It Depends on the size and the hardware contribution of the computer you are running.

So, these are the few problems due to _monolithic programming_. Monolithic means everything is a single unit.

We prefer to break the program into pieces, manageable and small pieces, and reusable pieces.

So, suppose we break the program into smaller tasks, i.e., into many smaller functions, and each function performs a specific task. In that case, such type of programming is called “modular programming” or “procedural programming,” and this approach is good for development.

![image_81.png](image_81.png)

As shown in the above image, the first function, i.e., function1(), will perform some specific task, and another function, i.e., function2(), will perform some other task, and similarly, function3() may perform some task. So, in this way, we can break the larger task into smaller simple tasks, and then we can use them all together inside the main function.

Here, in the **Modular Programming Approach**, you can break the program into smaller tasks, focus on smaller tasks, finish them, and make them perfect. It is easy for one single individual to develop the application, even if you can break this software project into a team of programmers where each programmer will focus on one or many smaller tasks.

## Types of Functions in C#:
Basically, there are two types of Functions in C#. They are as follows:

* Built-in Functions
* User-Defined Functions

![image_82.png](image_82.png)

> The function which is already defined in the framework and available to be used by the developer or programmer is called a built-in function, whereas if the function is defined by the developer or programmer explicitly, then it is called a user-defined function.

### Functions Vs Methods in C#
Functions: 
* > A function is a generic term for a block of code that performs a specific task and can return a value.
* > In C#, technically all functions are methods, because C# doesn’t allow free-floating functions — every function must be defined inside a class or struct.
* > However, when we say “function,” we’re usually talking in general programming terms, not C# specifically.

Example (conceptual “function”):
```C#
int Add(int a, int b)
{
    return a + b;
}

```
> In C#, this would be inside a class — so it’s actually a method (see below).

Methods:

* > A method is a function that belongs to a class or object.
* > It defines the behavior of that class or object.
* Methods can be:
  * > Instance methods (require an object instance)* 
  * > Static methods (belong to the class itself, not any instance)

```C#
public class Calculator
{
    // Instance method
    public int Add(int a, int b)
    {
        return a + b;
    }

    // Static method
    public static int Multiply(int a, int b)
    {
        return a * b;
    }
}

```
**Usage:**

```C#
Calculator calc = new Calculator();
int sum = calc.Add(3, 5);          // Instance method
int product = Calculator.Multiply(3, 5);  // Static method
```

## User-Defined Function in C#
First of all, the function should have a **name that is mandatory**. 
Then it should have a **parameter list** (the parameters it is taking) which is optional.
Then the function should have **a return type which is mandatory**. 
A function can have an access specifier, which is optional, and a modifier which is also optional. 
For a better understanding, please have a look at the below image.

![image_83.png](image_83.png)

### Example to Understand No Arguments Passed and No Return Value Function:

In the below example, the Sum() function does not take any parameters, or even it does not return a value. The return type of the function is void. Hence, no value is returned from the function.

```C#
using System;
namespace FunctionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Sum();
            Console.ReadKey();
        }
        static void Sum()
        {
            int x = 10;
            int y = 20;
            int sum = x + y;
            Console.WriteLine($"Sum of {x} and {y} is {sum}");
        }
    }
}
```

**Output:** Sum of 10 and 20 is 30

### No Arguments Passed but Return a Value Function
When a function has no arguments, it receives no data from the calling function but returns a value. The calling function receives the data from the called function. So, there is no data transfer between the calling function to called function but data transfer from the called function to the calling function. The called function is executed line by line in a normal fashion until the return statement is encountered.

#### Example to Understand No Arguments Passed but Return a Value Function :
In the below example, the empty parentheses in int Result = Sum(); statement indicates that no argument is passed to the function. And the value returned from the function is assigned to the Result variable. 

```C#
using System;
namespace FunctionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int Result=Sum();
            Console.WriteLine($"Sum is {Result}");
            Console.ReadKey();
        }
        static int Sum()
        {
            int x = 10;
            int y = 20;
            int sum = x + y;
            return sum;
        }
    }
}
```
**Output**: Sum is 30

### Argument Passed but no Return Value Function :
When a function has arguments, it receives data from the calling function but does not return any value. So, there is data transfer between the calling function to called function, but there is no data transfer from the called function to the calling function. The nature of data communication between the calling function and the called function with arguments but no return value.

#### Example to Understand Argument Passed but no Return Value Function :
In the example below, we pass two values to the Sum function, but the Sum function does not return any value to the main function.

```C#
using System;
namespace FunctionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int x = 10, y = 20;
            Sum(x, y);
            Console.ReadKey();
        }
        static void Sum(int x, int y)
        {
            int sum = x + y;
            Console.WriteLine($"Sum is {sum}");
        }
    }
}
```
**Output**: Sum is 30

### Argument Passed and Return Value Function in C# Language:
A self-contained and independent function should behave like a _“black box”_ that receives an input and outputs a value. Such functions will have two-way data communication.

```C#
using System;
namespace FunctionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int x = 10, y = 20;
            int Result = Sum(x, y);
            Console.WriteLine($"Sum is {Result}");
            Console.ReadKey();
        }
        static int Sum(int x, int y)
        {
            int sum = x + y;
            return sum;
        }
    }
}
```
**Output**: Sum is 30

## Function Overloading

n C#, we can write more than one function with the **same name**, but with a different argument or parameter list, and when we do so, it is called function overloading. Let us understand this with an example.

```C#
static void main(){
       int a = 10, b = 2, c;
       c = add(a, b);
}
```
This is our main function. Inside this function, we have declared 3 variables. Next, we store the result of the ‘_add()_’ function in the ‘c’ variable. The following is the add function.
```C#
static int add(int x, int y){
      return x + y;
}
```
Here we haven’t declared any variable; return ‘x + y’. When we call the ‘add’ function inside the main function, then a will be copied in ‘x’, and ‘b’ will be copied in ‘y’, and it will add these two numbers, and the result will store in ‘c’. Now we want to write one more function here,
```C#
static int add(int x, int y, int z){
       return x + y + z;
}
```
We have changed the main function as follows.
```C#
static void main(){
int a = 10, b = 2, c, d;
       c = add (a, b);
       d = add (a, b, c);
}
```
Here we have created another function with the same name, ‘add’, but it takes 3 parameters. Inside the main function, we have called ‘**add(x,y,z)**’ and stored the result in the ‘d’ variable. So, we can have two functions with the same name but with different parameters

So when we call “**add(a, b)**” it will be calling **add(int x, int y)**, and when we call ‘**add(a, b, c)**’ it will be “**add(int x, int y, int z)**”. The C# compiler can differentiate between these two functions, and this is the concept of **_function overloading in C#_**.

### Example to Understand Function Overloading
```C#
using System;
namespace FunctionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int a = 10, b = 2, c, d;
            c = add(a, b);
            Console.WriteLine($"Sum of {a} and {b} is {c}");
            d = add(a, b, c);
            Console.WriteLine($"Sum of {a} and {b} and {c} is {d}");
            Console.ReadKey();
        }
        static int add(int x, int y)
        {
            return x + y;
        }
        static int add(int x, int y, int z)
        {
            return x + y + z;
        }

    }
}
```
# Control Flow Statements in C#

The Control Flow Statements in C# are the statements that Alter the Flow of Program Execution and provide better control to the programmer on the flow of execution. The Control Flow Statements are useful to write better and more complex programs. A program executes from top to bottom except when we use control statements. We can control the order of execution of the program, based on logic and values.

By default, when we write statements in a program, the statements are going to be executed sequentially from top to bottom line by line. For example, in the below program we have written five statements. Now, if you execute the below program, the statements are going to be executed one by one from the top to bottom. That means, first it will execute statement1, then statement2, then statement3, then statement4, and finally statement5.

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Executing Statement1");
            Console.WriteLine("Executing Statement2");
            Console.WriteLine("Executing Statement3");
            Console.WriteLine("Executing Statement4");
            Console.WriteLine("Executing Statement5");
            Console.ReadKey();
        }
    }
}
```
Output:

```Plain Text
Executing Statement1
Executing Statement2
Executing Statement3
Executing Statement4
Executing Statement5
```
## Types of Control Flow Statements in C#:
In C#, the control flow statements are divided into the following three categories:

1. **Selection Statements** or **Branching Statements**: (Examples: if-else, switch case, nested if-else, if-else ladder)
2. **Iteration Statements** or **Looping Statements**: (Examples: while loop, do-while loop, for-loop, and foreach loop)
3. **Jumping Statements**: (Examples: break, continue, return, goto)

![image_45.png](image_45.png)

## Selection Statements

### If Statements in C#
It executes a block of statements (one or more instructions) when the condition in the if block is true and when the condition is false, it will skip the execution of the if block. Using else block is optional in C#. Following is the syntax to use the if block in the C# language.

![image_46.png](image_46.png)

**Flow Chart of If Block:**
![image_47.png](image_47.png)

Here, the block of statements executes only when the condition is true. And if the condition is false, then it will skip the execution of the statements.

#### Example to Understand If Block in C#:

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int number;
            Console.WriteLine("Enter a Number: ");
            number = Convert.ToInt32(Console.ReadLine());
            if (number > 10)
            {
                Console.WriteLine($"{number} is greater than 10 ");
                Console.WriteLine("End of if block");
            }
            Console.WriteLine("End of Main Method");
            Console.ReadKey();
        }
    }
}
```

Output:
If we take 43 as an input, 43 > 10 means the condition is true, then if block statement gets executed.

```Plain Text
Enter a Number:
43
43 is greater than 10
End of if block
End of Main Method
```

If we take 5 as an input, 5 > 10 means the condition is false, then the if block statements will be skipped
```Plain Text
Enter a Number:
5
End of Main Method
```

### If Else Statements in C# Language:
The If-Else block in C# Language is used to provide some optional information whenever the given condition is FALSE in the if block. That means if the condition is true, then the if block statements will be executed, and if the condition is false, then the else block statement will execute.

![image_48.png](image_48.png)

**Flow Chart of If-Else Block:**
![image_49.png](image_49.png)

> In C# Programming Language, if and else are reserved words. That means you cannot use these two keywords for the naming of any variables, properties, class, methods, etc. The expressions or conditions specified in the if block can be a Relational or Boolean expression or condition that evaluates to TRUE or FALSE. Now let us see some examples to understand the if-else conditional statements

#### Example to Understand IF-ELSE Statement in C#:
Let us write a Program to Check Whether a Number is Even or Odd using If Else Statements in C# Language. Here we will take the input number from the user and then we will check whether that number is even or odd using the if-else statement in C# Language. The following program exactly does the same.

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Enter a Number: ");
            int number = Convert.ToInt32(Console.ReadLine());
            if (number % 2 == 0)
            {
                Console.WriteLine($"{number} is an Even Number");
            }
            else
            {
                Console.WriteLine($"{number} is an Odd Number");
            }

            Console.ReadKey();
        }
    }
}
```

If we take 16 as an input, 16%2 == 0 means the condition is true, then the if block statement gets executed. And the output will be 16 is an Even Number.

```Plain Text
Enter a Number:
16
16 is an Even Number
```

If we take 13 as an input, 13%2 == 0 means the condition is false, then the else block statements get executed. And the output will be 13 is an Odd Number.

```Plain Text
Enter a Number:
13
13 is an Odd Number
```

### Nested If-Else Statements in C# Language:
When an if-else statement is present inside the body of another if or else then this is called nested if-else. Nested IF-ELSE statements are used when we want to check for a condition only when the previous dependent condition is true or false

#### What is the Nested If block?
Nested if block means defining if block inside another if block. We can also define the if block inside the else blocks. Depending on our logic requirements, we can use nested if block either inside the if block or inside the else.

![image_50.png](image_50.png)

Now, we will take one example and try to understand the flow chart. We are taking the following syntax. Here, we have an if-else block inside the if block, as well as, an if-else block inside the else block.

![image_51.png](image_51.png)

Let us understand the above code.

* Condition1: First, it will check the first if condition i.e. the outer if condition and if it is true, then the outer else block will be terminated. So, the control moves inside the first or the outer if block. Then again it checks the inner if condition and if the inner if condition is true, then the inner else block gets terminated. So, in this case, the outer if and inner if block statements get executed.
* Condition2: Now, if the outer if condition is true, but the inner if condition is false, then the inner if block gets terminated. So, in this case, the outer if and inner else block statements get executed.
* Condition3: Now, if the outer if condition is false, then the outer if block gets terminated and control moves to the outer else block. And inside the outer else block, again it checks the inner if condition, and if the inner if condition is true, then the inner else block gets terminated. So, in this case, the outer else and inner if block statements get executed.
* Condition4: Now, if the outer if condition is false as well as the if condition inside the outer else blocks also failed, then the if block gets terminated. And in this case, the outer else and inner else block statements get executed. This is how statements get executed in Nested if. Now we will see the flow chart of nested if blocks.


#### Flow Chart of Nested If – Else Block in C# Language:
![image_52.png](image_52.png)

#### Example to understand Nested IF-ELSE Statements in C# Language:

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int a = 15, b = 25, c = 10;
            int LargestNumber = 0;

            if (a > b)
            {
                Console.WriteLine($"Outer IF Block");
                if (a > c)
                {
                    Console.WriteLine($"Outer IF - Inner IF Block");
                    LargestNumber = a;
                }
                else
                {
                    Console.WriteLine($"Outer IF - Inner ELSE Block");
                    LargestNumber = c;
                }
            }
            else
            {
                Console.WriteLine($"Outer ELSE Block");
                if (b > c)
                {
                    Console.WriteLine($"Outer ELSE - Inner IF Block");
                    LargestNumber = b;
                }
                else
                {
                    Console.WriteLine($"Outer ELSE - Inner ELSE Block");
                    LargestNumber = c;
                }
            }

            Console.WriteLine($"The Largest Number is: {LargestNumber}");

            Console.ReadKey();
        }
    }
}
```
Output:
```Plain Text
Outer ELSE Block
Outer ELSE - Inner IF Block
The Largest Number is: 25
```
### Ladder if-else statements in C# Language:
In Ladder if-else statements one of the statements will be executed depending upon the truth or false of the conditions. If the condition1 is true then Statement 1 will be executed, and if condition2 is true then statement 2 will be executed, and so on. But if all conditions are false, then the last statement i.e. else block statement will be executed. The C# if-else statements are executed from top to bottom. As soon as one of the conditions controlling the if is true, the statement associated with that if block is going to be executed, and the rest of the C# else-if ladder is bypassed. If none of the conditions are true, then the final else statement will be executed.

![image_53.png](image_53.png)

#### Example to understand Ladder If-Else Statements in C# Language:
```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int i = 20;
            if (i == 10)
            {
                Console.WriteLine("i is 10");
            }
            else if (i == 15)
            {
                Console.WriteLine("i is 15");
            }
            else if (i == 20)
            {
                Console.WriteLine("i is 20");
            }
            else
            {
                Console.WriteLine("i is not present");
            }

            Console.ReadKey();
        }
    }
}
```

### Switch Statements in C#
The switch is a keyword in the C# language, and by using this switch keyword we can create selection statements with multiple blocks. And the Multiple blocks can be constructed by using the case keyword.

Switch case statements in C# are a substitute for long if else statements that compare a variable or expression to several values.

#### When do we need to go for a switch statement?
When there are several options and we have to choose only one option from the available options depending on a single condition then we need to go for a switch statement. Depending on the selected option a particular task can be performed.

#### Syntax of Switch Statements in C# Language:
In C#, the Switch statement is a multiway branch statement. It provides an efficient way to transfer the execution to different parts of a code based on the value of the expression. The switch expression is of integer type such as int, byte, or short, or of an enumeration type, or of character type, or of string type. The expression is checked for different cases and the match case will be executed. The following is the syntax to use switch case statement in C# language.

![image_54.png](image_54.png)

##### Example to understand Switch Statement in C# Language:
```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int x = 2;
            switch (x)
            {
                case 1:
                    Console.WriteLine("Choice is 1");
                    break;
                case 2:
                    Console.WriteLine("Choice is 2");
                    break;
                case 3:
                    Console.WriteLine("Choice is 3");
                    break;
                default:
                    Console.WriteLine("Choice other than 1, 2 and 3");
                    break;
            }
            Console.ReadKey();
        }
    }
}
```
Output: 
```Plain Text
Choice is 2
```

After the end of each case block, it is necessary to insert a break statement. If we are not inserting the break statement, then we will get a compilation error. But you can combine multiple case blocks with a single break statement if and only if the previous case statement does not have any code block. For a better understanding, please have a look at the below example.

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            string str = "C#";
            switch (str)
            {
                case "C#":      
                case "Java":
                case "C":
                    Console.WriteLine("It’s a Programming Langauge");
                    break;

                case "MSSQL":
                case "MySQL":
                case "Oracle":
                    Console.WriteLine("It’s a Database");
                    break;

                case "MVC":
                case "WEB API":
                    Console.WriteLine("It’s a Framework");
                    break;

                default:
                    Console.WriteLine("Invalid Input");
                    break;
            }
            Console.ReadKey();
        }
    }
}
```
Output: 
```Plain Text
It’s a Programming Language
```

##### Is default block Mandatory in a Switch Statement?
No, the default block in the switch statement is not mandatory. If you are putting the default block and if any of the case statement is not fulfilled, then only the default block is going to be executed. For a better understanding, please have a look at the below example where we don’t have the default block.

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            string str = "C#2";
            Console.WriteLine("Switch Statement Started");
            switch (str)
            {
                case "C#":      
                case "Java":
                case "C":
                    Console.WriteLine("It's a Programming Language");
                    break;

                case "MSSQL":
                case "MySQL":
                case "Oracle":
                    Console.WriteLine("It's a Database");
                    break;

                case "MVC":
                case "WEB API":
                    Console.WriteLine("It's a Framework");
                    break;
            }
            Console.WriteLine("Switch Statement Ended");
            Console.ReadKey();
        }
    }
}
```

Output:
```Plain Text
Switch Statement Started
Switch Statement Ended
```

### Nested Switch Statement in C#:
```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Ask the user to enter a number between 1 and 3
            Console.Write("Enter a Number Between 1 and 3:");
            int number = Convert.ToInt32(Console.ReadLine());

            //outer Switch Statement
            switch (number)
            {
                case 1:
                    Console.WriteLine("You Entered One");
                    //Ask the user to enter the character R, B, or G
                    Console.Write("Enter Color Code (R/G/B): ");
                    char color = Convert.ToChar(Console.ReadLine());

                    //Inner Switch Statement
                    switch (Char.ToUpper(color))
                    {
                        case 'R':
                            Console.WriteLine("You have Selected Red Color");
                            break;
                        case 'G':
                            Console.WriteLine("You have Selected Green Color");
                            break;
                        case 'B':
                            Console.WriteLine("You have Selected Blue Color");
                            break;
                        default:
                            Console.WriteLine($"You Have Enter Invalid Color Code: {Char.ToUpper(color)}");
                            break;
                    }
                    break;

                case 2:
                    Console.WriteLine("You Entered Two");
                    break;

                case 3:
                    Console.WriteLine("You Entered Three");
                    break;
                default:
                    Console.WriteLine("Invalid Number");
                    break;
            }

            Console.ReadLine();
        }
    }
}
```
output:
```Plain Text
Enter a Number Between 1 and 3:1
You Entered One
Enter Color Code (R/G/B): b
You have Selected Blue Color
```

## Iteration Statements
### Loops in C#

Loops are also called repeating statements or iterative statements. Loops play an important role in programming. The Looping Statements are also called Iteration Statements. So, we can use the word Looping and Iteration and the meanings are the same.

Example to print numbers from 1 to 10 without using the loop in C#
Till now what we learned using those concepts If I write a program to print 1 to 10 it looks something like this.

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("1");
            Console.WriteLine("2");
            Console.WriteLine("3");
            Console.WriteLine("4");
            Console.WriteLine("5");
            Console.WriteLine("6");
            Console.WriteLine("7");
            Console.WriteLine("8");
            Console.WriteLine("9");
            Console.WriteLine("10");

            Console.ReadKey();
        }
    }
}
```

Output:
```Plain Text
1
2
3
4
5
6
7
8
9
10
```
> Note: Even though we are able to print the numbers from 1 to 10, the code doesn’t look good as the same instruction is written multiple times also what is it if want to print from 1 to 1000? Or from 1 to 100000? So, without loops, the code not even looks understandable and efficient.

### What is looping?
Looping in programming languages is a feature that facilitates the execution of a set of instructions repeatedly while some condition evaluates to true.

The process of repeatedly executing a statement or group of statements until the condition is satisfied is called looping. In this case, when the condition becomes false the execution of the loops terminates. The way it repeats the execution of the statements or instructions will form a circle that’s why iteration statements are called loops.

### Types of Loops in C#

1. For loop
2. For Each Loop
3. While loop
4. Do while loop

![image_55.png](image_55.png)

As the input is processed then it checks for the condition, if the condition is true then again it goes back and processing will do and then again check for the condition, if the condition is true then again goes back, and so on.

This will be repeated. So, this processing part will go on repeating as long as that condition is true and once the conditions become false it will come out from here and print the output.

#### While Loop in C#

A while loop is used for executing a statement repeatedly until `a given condition returns false`. Here, statements may be a single statement or a block of statements. `The loop iterates while the condition is true`. If you see the syntax and flow chart parallelly, then you will get more clarity of the while loop.

##### While Loop Syntax in C# 

![image_56.png](image_56.png)

Flow Chart of While Loop in C#

![image_57.png](image_57.png)

**Example to understand While loop**

In the below example, 
- The variable x is initialized with value 1 
- Then it has been tested for the condition. 
- If the condition returns `true` then the statements inside the body of the while loop are `executed` else control comes out of the loop. 
- The value of x is incremented using the ++ operator.
- Then it has been tested again for the loop condition.

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int x = 1;
            while (x <= 5)
            {
                Console.WriteLine("Value of x:" + x);
                x++;
            }
            Console.ReadKey();
        }
    }
}
```

Output:
```Plain Text
Value of x:1
Value of x:2
Value of x:3
Value of x:4
Value of x:5
```

**Advanced Example to understand While loop**
Print the numbers in the following format up to a given number and that number is entered from the keyboard.
`2 4 6 8 ……………………..` up to that given number

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int i, n;
            Console.Write("Enter a Number : ");
            n = Convert.ToInt32(Console.ReadLine());
            i = 2;
            while (i <= n)
            {
                Console.Write($"{i} ");
                i = i + 2;
            }

            Console.ReadKey();
        }
    }
}
```
Output:
```Plain Text
Enter a Number : 10
2 4 6 8 10
```

#### Nested While Loop in C#
Writing a while loop inside another while loop is called nested while loop or you can say defining one while loop inside another while loop is called nested while loop. This is the reason why nested loops are also called “loops inside the loop”.

###### Nested While Loop Syntax
![image_58.png](image_58.png)

> Note: In the nested while loop, the number of iterations will be equal to the number of iterations in the outer loop multiplied by the number of iterations in the inner loop. Nested while loops are mostly used for making various pattern programs in C# like number patterns or shape patterns.

###### Flow Chart of Nested While Loop

![image_59.png](image_59.png)

**Example to Print the Following Format using Nested While Loop in C# Language**
![image_60.png](image_60.png)

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.Write("ENTER  A NUMBER ");
            int n = Convert.ToInt32(Console.ReadLine());
            int i = 1;
            while (i <= n)
            {
                Console.WriteLine();
                int j = 1;
                while (j <= i)
                {
                    Console.Write(j + " ");
                    j++;
                }
                i++;
            }

            Console.ReadKey();
        }
    }
}
```

#### Do While Loop in C#
The do-while loop is a post-tested loop or exit-controlled loop i.e. first it will execute the loop body and then it will be going to test the condition. That means we need to use the do-while loop where we need to execute the loop body at least once. The do-while loop is mainly used in menu-driven programs where the termination condition depends upon the end-user.

##### Syntax to use Do While Loop in C#
![image_61.png](image_61.png)

**While and do-while are almost the same. So, what is the difference? Which one do we use?**

The difference between the do-while loop and the while loop in C# is that the do-while evaluates its test condition at the bottom of the loop whereas the while loop evaluates its test condition at the top. Therefore, the statements written inside the do-while block are executed at least once whereas we cannot give a guarantee that the statements written inside the while loop are going to be executed at least once.

> Note: When you want to execute the loop body at least once irrespective of the condition, then you need to use the do-while loop else you need to use the while loop.

##### Flow Chart of Do-While Loop
![image_62.png](image_62.png)

##### Example to understand do while loop in C#

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int number = 1;
            do
            {
                Console.Write($"{number} ");
                number++;
            } while (number <= 5);

            Console.ReadKey();
        }
    }
}
```
Output: 1 2 3 4 5

#### Nested Do-While Loop in C#

Using a do-while loop inside another do-while loop is called nested do-while loop. The syntax to use the nested do-while loop in C# language is given below.

![image_63.png](image_63.png)

**Example to Understand Nested do-while Loop in C#**

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            do
            {
                Console.WriteLine("I am from outer do-while loop");
                do
                {
                    Console.WriteLine("I am from inner do-while loop");
                }
                while (1 > 10);
            }
            while (2 > 10);

            Console.ReadKey();
        }
    }
}
```

Output:
```Plain Text
I am from outer do-while loop
I am from inner do-while loop
```

#### For Loop in C#
For loop is one of the most commonly used loops in the C# language. If we know the number of times, we want to execute some set of statements or instructions, then we should use for loop. For loop is known as a Counter loop. Whenever counting is involved for repetition, then we need to use for loop.

##### For Loop Flowchart
The following diagram shows the flowchart of the for loop.

![image_64.png](image_64.png)

##### Syntax to use For Loop in C#
The for loop allows the execution of instructions for a specific amount of time. It has four stages.

* Loop initialization
* Condition evaluation
* Execution of instruction
* Increment/Decrement

Now let’s have a look at the for loop syntax:

![image_65.png](image_65.png)

**Explanation of the for-loop syntax:**
* **Loop Initialization**: Loop `initialization happens only once` while executing the for loop, which means that the initialization part of for loop only executes once. Here, initialization means we need to initialize the counter variable.
* **Condition Evaluation**: Conditions in for loop are executed for each iteration and if the condition is true, it executes the C# instruction and if the condition is false then it comes out of the loop.
* **Execution of Instruction**: Once the condition is evaluated, and if the condition is true, then the control comes to the loop body i.e. the loop body is going to be executed.
* **Increment/Decrement**: After executing the loop body, the increment/decrement part of the for loop will be executed, and once it executes the increment decrement part i.e. once it increments and decrements the counter variable, again it will go to the condition evaluation stage.

**Example to Print Numbers From 1 to n Using For Loop**

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.Write("Enter one Integer Number:");
            int number = Convert.ToInt32(Console.ReadLine());
            for (int counter = 1; counter <= number; counter++)
            {
                Console.WriteLine(counter);
            }
            Console.ReadKey();
        }
    }
}
```
```Plain Text
Output: 
1
2
3
4
5
``` 

You will get the same output as the previous example.

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.Write("Enter one Integer Number:");
            int number = Convert.ToInt32(Console.ReadLine());
            int counter = 1; //Initialization
            for (;counter <= number;)
            {
                Console.WriteLine(counter);
                counter++; //Updation
            }
            Console.ReadKey();
        }
    }
}
```

> Can we run for loop without condition in C#?
Yes, we can run a for loop without condition. And, it will be an infinite loop. Because if we don’t mention any termination condition in for loop, the for loop is not going to end.

#### Nested for Loop in C#:

When we created one for loop inside the body of another for loop, then it is said to be nested for loop in C# language. The syntax to use nested for loop is given below.

![image_66.png](image_66.png)

> The point that you need to remember is when the inner for loop condition failed, then it will terminate the inner for loop only. And when the outer for loop condition failed, then it will terminate the outer for loop.

**Example to Understand Nested For Loop in C#:**
In the below example, we have created a nested for loop. The outer for loop is going to be executed 5 times and for each iteration of the outer for loop, the inner for loop is going to execute 10 times.

```C#
using System;
namespace ControlFlowDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            for (int i = 1; i <= 5; i++) //Outer For Loop
            {
                Console.WriteLine($"Outer For Loop : {i}");
                for (int j = 1; j <= 10; j++) //Inner For Loop
                {
                    Console.Write($" {j}");
                }
                Console.WriteLine();
            }

            Console.ReadKey();
        }
    }
}
```
Output:
```Plain Text
Outer For Loop : 1
 1 2 3 4 5 6 7 8 9 10
Outer For Loop : 2
 1 2 3 4 5 6 7 8 9 10
Outer For Loop : 3
 1 2 3 4 5 6 7 8 9 10
Outer For Loop : 4
 1 2 3 4 5 6 7 8 9 10
Outer For Loop : 5
 1 2 3 4 5 6 7 8 9 10
```

## Jumping Statements
The Jump Statements in C# are used to transfer control from one point or location or statement to another point or location or statement in the program due to some specified condition while executing the program.

The Jump Statements in C# Language are used to modify the behavior of conditional (if, else, switch) and iterative (for, while, and do-while) statements. The Jump Statements allow us to exit a loop, and start the next iteration, or explicitly transfer the program control to a specified location in your program. C# supports the following jump statements:

* **break**
* **continue**
* **goto**
* **return** (In the Function section we will discuss the return statement)
* **throw** (In the Exception Handling section we will discuss the throw statement)

#### Break Statement in C#
In C#, the break is a keyword. By using the break statement, we can terminate either the loop body or the switch body. The most important point that you need to keep in mind is the use of a break statement is optional but if you want to use then the break statement should be placed either within the loop body or switch body.

##### Flowchart of Break Statement:

![image_67.png](image_67.png)

When it encounters the break statement inside a loop body or switch body, then immediately it terminates the loop and switch execution and executes the statements which are present after the loop body or switch body. But if the break statement is not executed, then the statements which are present after the break statement will be executed and then it will continue its execution with the next iteration of the loop.

##### How does the break statement work in C# 
![image_68.png](image_68.png)

If you notice the above code, we have written the if conditional statement inside the loop body, and within the if condition block, we have written the break statement. So, when the loop executes, in each iteration, the if condition will be checked and if the condition is false, then it will execute the statements which are present after the if block and continue with the next iteration. Now, what happens when the if condition is true? Once the if condition is evaluated to true, then the if block will be executed, and once the break statement within the if block is executed, it immediately terminates the loop, and the statements which are present after the loop block will be executed.

##### Example to Understand Break Statement in C#

In the below example, we have provided the condition for the loop to be executed 10 times i.e. starting from I value 1 to 10. But our requirement is when the I value becomes 5, we need to terminate the loop. In this case, we need to write if condition inside the loop body and check whether the current I value is equal to 5 or not. If it is not equal to 5, then continue the execution of the for loop and execute the next iteration. But if the I value is 5, then the, if condition will return true, and, in that case, the break statement will be executed and once the break statement is executed, it will immediately terminate the loop body.

```C#
using System;
namespace JumpStatementDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            for (int i = 1; i <= 10; i++)
            {
                Console.WriteLine($"I : {i}");
                if (i == 5)
                {
                    break;
                }
            }
            Console.WriteLine("Out of for-loop");

            Console.ReadKey();
        }
    }
}
```

Output:
```Plain Text
I : 1
I : 2
I : 3
I : 4
I : 5
Out of for-loop
```

#### Continue Statement in C#

In C#, continue is a keyword. By using the continue keyword, we can skip the statement execution from the loop body. Like the break statement, the use of the continue statement is also optional but if you want to use then you can use it only within the loop body.

> The `BREAK statement terminates the loop`, whereas the `CONTINUE statement skips only the current loop iteration`, and allows the next loop iteration to proceed. The continue statement is almost always used with the if…else statement.

![image_69.png](image_69.png)

The continue statement in C# is very similar to the break statement, except that instead of terminating the loop, it will skip the current iteration and continue with the next iteration. That means the continue statement skips the rest of the body of the loop and immediately checks the loop’s condition.

##### How Does the Continue Statement Work in C#
![image_70.png](image_70.png)

If you notice the above code, we have written the if conditional statement inside the loop body, and within the if condition block, we have written the continue statement. So, when the loop executes, in each iteration, the if condition will be checked and if the condition is false, then it will execute the statements which are present after the if block and continue with the next iteration. Now, what happens when the if condition is true? Once the if condition is evaluated to true, then the if block will be executed, `and once the continue statement within the if block is executed, it will skip the execution of the statements which are present after the continue statement and continue with the execution of the next iteration` of the loop.

**Example to Understand Continue Statement in C#**

````C#
using System;
namespace JumpStatementDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            for (int i = 1; i <= 5; i++)
            {
                if (i == 3)
                {
                    continue;
                }
                Console.WriteLine($"I : {i}");
            }
            
            Console.ReadKey();
        }
    }
}
````

Output:

```Plain Text
I : 1
I : 2
I : 3
I : 4
I : 5
```
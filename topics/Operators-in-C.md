# Operators in C#

Operators in C# are symbols that are used to perform operations on operands.
For example, consider the expression `2 + 3 = 5`, here `2` and `3` are `operands`, and `+` and `=` are called `operators`. So, the Operators in C# are used to manipulate the variables and values in a program.

## Types of Operators in C#:

1. Arithmetic Operators
2. Relational Operators
3. Logical Operators
4. Bitwise Operators
5. Assignment Operators
6. Unary Operators or
7. Ternary Operator or Conditional Operator

In C#, the Operators can also be categorized based on the Number of Operands:

![image_43.png](image_43.png)

1. **Unary Operator**: The Operator that requires one operand (variable or value) to perform the operation is called Unary Operator.
2. **Binary Operator**: Then Operator that requires two operands (variables or values) to perform the operation is called Binary Operator.
3. **Ternary Operator**: The Operator that requires three operands (variables or values) to perform the operation is called Ternary Operator. The Ternary Operator is also called Conditional Operator.

### Arithmetic Operators in C#

The Arithmetic Operators in C# are used to perform arithmetic/mathematical operations like addition, subtraction, multiplication, division, etc. on operands. The following Operators are falling into this category.

#### Addition Operator (+):

```C#
int a = 10;
int b = 5;
int c = a + b; //15, Here, it will add the a and b operand values i.e. 10 + 5
```

#### Subtraction Operator (-):

```C#
int a = 10;
int b = 5;
int c = a - b; //5, Here, it will subtract b from a i.e. 10 – 5
```

#### Multiplication Operator (*):

```C#
int a = 10;
int b = 5;
int c = a * b; //50, Here, it will multiply a with b i.e. 10 * 5
```

#### Division Operator (/):

```C#
int a = 10;
int b = 5;
int c = a / b; //2, Here, it will divide 10 / 5
```

#### Modulus Operator (%):
The % (Modulos) operator returns the remainder when the first operand is divided by the second. As this operator works with two operands, so, this % (Modulos) operator belongs to the category of the binary operator

```C#
int a = 10;
int b = 5;
int c=a % b; //0, Here, it will divide 10 / 5 and it will return the remainder which is 0 in this case
```

##### Example to Understand Arithmetic Operators in C#:

````C#
using System;
namespace OperatorsDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            int Result;
            int Num1 = 20, Num2 = 10;

            // Addition Operation
            Result = (Num1 + Num2);
            Console.WriteLine($"Addition Operator: {Result}" );

            // Subtraction Operation
            Result = (Num1 - Num2);
            Console.WriteLine($"Subtraction Operator: {Result}");

            // Multiplication Operation
            Result = (Num1 * Num2);
            Console.WriteLine($"Multiplication Operator: {Result}");

            // Division Operation
            Result = (Num1 / Num2);
            Console.WriteLine($"Division Operator: {Result}");

            // Modulo Operation
            Result = (Num1 % Num2);
            Console.WriteLine($"Modulo Operator: {Result}");

            Console.ReadKey();
        }
    }
}
````

### Assignment Operators in C#:
The Assignment Operators in C# are used to assign a value to a variable. The left-hand side operand of the assignment operator is a variable and the right-hand side operand of the assignment operator can be a value or an expression that must return some value and that value is going to assign to the left-hand side variable.

#### Simple Assignment (=):

```C#
int a=10;
int b=20;
```

#### Add Assignment (+=):
This operator is the combination of + and = operators. It is used to add the left-hand side operand value with the right-hand side operand value and then assign the result to the left-hand side variable.

```C#
int a=5;
int b=6;
a += b; //a=a+b; That means (a += b) can be written as (a = a + b)
```

#### Subtract Assignment (-=):
This operator is the combination of – and = operators. It is used to subtract the right-hand side operand value from the left-hand side operand value and then assign the result to the left-hand side variable.

```C#
int a=10;
int b=5;
a -= b; //a=a-b; That means (a -= b) can be written as (a = a – b)
```

#### Multiply Assignment (*=):
This operator is the combination of * and = operators. It is used to multiply the left-hand side operand value with the right-hand side operand value and then assign the result to the left-hand side variable. 

```C#
int a=10;
int b=5;
a *= b; //a=a*b; That means (a *= b) can be written as (a = a * b)
```

#### Division Assignment (/=):
This operator is the combination of / and = operators. It is used to divide the left-hand side operand value with the right-hand side operand value and then assign the result to the left-hand side variable.

```C#
int a=10;
int b=5;
a /= b; //a=a/b; That means (a /= b) can be written as (a = a / b)
```

#### Modulus Assignment (%=):
This operator is the combination of % and = operators. It is used to divide the left-hand side operand value with the right-hand side operand value and then assigns the remainder of this division to the left-hand side variable.

```C#
int a=10;
int b=5;
a %= b; //a=a%b; That means (a %= b) can be written as (a = a % b)
```

##### Example to Understand Assignment Operators in C#:

```C#
using System;
namespace OperatorsDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            // Initialize variable x using Simple Assignment Operator "="
            int x = 15;

            x += 10;  //It means x = x + 10 i.e. 15 + 10 = 25
            Console.WriteLine($"Add Assignment Operator: {x}");

            // initialize variable x again
            x = 20;
            x -= 5;  //It means x = x - 5 i.e. 20 - 5 = 15
            Console.WriteLine($"Subtract Assignment Operator: {x}");

            // initialize variable x again
            x = 15;
            x *= 5; //It means x = x * 5  i.e. 15 * 5 = 75
            Console.WriteLine($"Multiply Assignment Operator: {x}");

            // initialize variable x again
            x = 25;
            x /= 5; //It means x = x / 5 i.e. 25 / 5 = 5
            Console.WriteLine($"Division Assignment Operator: {x}");

            // initialize variable x again
            x = 25;
            x %= 5; //It means x = x % 5 i.e. 25 % 5 = 0
            Console.WriteLine($"Modulo Assignment Operator: {x}");

            Console.ReadKey();
        }
    }
}
```

### Relational Operators in C#:

The Relational Operators in C# are also known as Comparison Operators. It determines the relationship between two operands and returns the Boolean results, i.e. true or false after the comparison. The Different Types of Relational Operators supported by C# are as follows.

#### Equal to (==):
This Operator is used to return true if the left-hand side operand value is equal to the right-hand side operand value. For example, 5==3 is evaluated to be false. So, this Equal to (==) operator will check whether the two given operand values are equal or not. If equal returns true else returns false.

#### Not Equal to (!=):
This Operator is used to return true if the left-hand side operand value is not equal to the right-hand side operand value. For example, 5!=3 is evaluated to be true. So, this Not Equal to (!=) operator will check whether the two given operand values are equal or not. If equal returns false else returns true

#### Less than (<):
This Operator is used to return true if the left-hand side operand value is less than the right-hand side operand value. For example, 5<3 is evaluated to be false. So, this Less than (<) operator will check whether the first operand value is less than the second operand value or not.

#### Less than or equal to (<=):
This Operator is used to return true if the left-hand side operand value is less than or equal to the right-hand side operand value. For example, 5<=5 is evaluated to be true. So. this Less than or equal to (<=) operator will check whether the first operand value is less than or equal to the second operand value. If so returns true else returns false.

#### Greater than (>):
This Operator is used to return true if the left-hand side operand value is greater than the right-hand side operand value. For example, 5>3 is evaluated to be true. So, this Greater than (>) operator will check whether the first operand value is greater than the second operand value. If so, returns true else return false.

#### Greater than or Equal to (>=):
This Operator is used to return true if the left-hand side operand value is greater than or equal to the right-hand side operand value. For example, 5>=5 is evaluated to be true. So, this Greater than or Equal to (>=) operator will check whether the first operand value is greater than or equal to the second operand value. If so, returns true else returns false.

##### Example to Understand Relational Operators in C#:

```C#
using System;
namespace OperatorsDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            bool Result;
            int Num1 = 5, Num2 = 10;

            // Equal to Operator
            Result = (Num1 == Num2);
            Console.WriteLine("Equal (=) to Operator: " + Result);

            // Greater than Operator
            Result = (Num1 > Num2);
            Console.WriteLine("Greater (<) than Operator: " + Result);

            // Less than Operator
            Result = (Num1 < Num2);
            Console.WriteLine("Less than (>) Operator: " + Result);

            // Greater than Equal to Operator
            Result = (Num1 >= Num2);
            Console.WriteLine("Greater than or Equal to (>=) Operator: " + Result);

            // Less than Equal to Operator
            Result = (Num1 <= Num2);
            Console.WriteLine("Lesser than or Equal to (<=) Operator: " + Result);

            // Not Equal To Operator
            Result = (Num1 != Num2);
            Console.WriteLine("Not Equal to (!=) Operator: " + Result);

            Console.ReadKey();
        }
    }
}
```

Output:

```Plain Text
Equal (=) to Operator: False
Greater (<) than Operator: False
Less than (>) Operator: True
Greater than or Equal to (>=) Operator: False
Lesser than or Equal to (<=) Operator: True
Not Equal to (!=) Operator: True
```

### Logical Operators in C#:
The Logical Operators are mainly used in conditional statements and loops for evaluating a condition. These operators are going to work with boolean expressions. The different types of Logical Operators supported in C# are as follows:

#### Logical OR (||):
This operator is used to return true if either of the Boolean expressions is true. For example, false || true is evaluated to be true. That means the Logical OR (||) operator returns true when one (or both) of the conditions in the expression is satisfied. Otherwise, it will return false. For example, a || b returns true if either a or b is true. Also, it returns true when both a and b are true.

##### Logical AND (&&):
This operator is used to return true if all the Boolean Expressions are true. For example, false && true is evaluated to be false. That means the Logical AND (&&) operator returns true when both the conditions in the expression are satisfied. Otherwise, it will return false. For example, a && b return true only when both a and b are true.

#### Logical NOT (!):
This operator is used to return true if the condition in the expression is not satisfied. Otherwise, it will return false. For example, !a returns true if a is false.

##### Example to Understand Logical Operators in C#:

```C#
using System;
namespace OperatorsDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            bool x = true, y = false, z;

            //Logical AND operator
            z = x && y;
            Console.WriteLine("Logical AND Operator (&&) : " + z);

            //Logical OR operator
            z = x || y;
            Console.WriteLine("Logical OR Operator (||) : " + z);

            //Logical NOT operator
            z = !x;
            Console.WriteLine("Logical NOT Operator (!) : " + z);

            Console.ReadKey();
        }
    }
}
```

Output:
```Plain Text
Logical AND Operator (&&) : False
Logical OR Operator (||) : True
Logical NOT Operator (!) : False
```

#### Unary/Modify Operators in C#:
The Unary Operators in C# need only one operand. They are used to increment or decrement a value. There are two types of Unary Operators. They are as follows:

* **Increment operators (++)**: Example: (++x, x++)
* **Decrement operators (–)**: Example: (--x, x--)

![image_44.png](image_44.png)
> Increment Operator means to `increment` the value of the variable by `1` and Decrement Operator means to `decrement` the value of the variable by `1`.

##### Increment Operator (++) in C# Language:
The Increment Operator (++) is a unary operator. It operates on a single operand only. Again, it is classified into two types:

1. Post-Increment Operator
2. Pre-Increment Operator
##### Post Increment Operators:
The Post Increment Operators are the operators that are used as a suffix to its variable. It is placed after the variable. For example, a++ will also increase the value of the variable a by 1.

Syntax: Variable++;
Example: x++;

##### Pre-Increment Operators:
The Pre-Increment Operators are the operators which are used as a prefix to its variable. It is placed before the variable. For example, ++a will increase the value of the variable a by 1.

Syntax: ++Variable;
Example: ++x;

##### Decrement Operators in C# Language:
The Decrement Operator (–) is a unary operator. It takes one value at a time. It is again classified into two types. They are as follows:

* Post Decrement Operator
* Pre-Decrement Operator
##### Post Decrement Operators:
The Post Decrement Operators are the operators that are used as a suffix to its variable. It is placed after the variable. For example, a– will also decrease the value of the variable a by 1.

Syntax: Variable--;
Example: x--;

##### Pre-Decrement Operators:
The Pre-Decrement Operators are the operators that are a prefix to its variable. It is placed before the variable. For example, –a will decrease the value of the variable a by 1.

Syntax: --Variable;
Example: --x;

##### Example to Understand Increment Operators in C# Language:

```C#
using System;
namespace OperatorsDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            // Post-Increment
            int x = 10;
            // Result1 is assigned 10 only,
            // x is not updated yet
            int Result1 = x++;
            //x becomes 11 now
            Console.WriteLine("x is {0} and Result1 is {1}", x, Result1);

            // Pre-Increment 
            int y = 10;
            int Result2 = ++y;
            //y and Result2 have same values = 11
            Console.WriteLine("y is {0} and Result2 is {1}", y, Result2);

            Console.ReadKey();
        }
    }
}
```
output:
```Plain Text
x is 11 and Result1 is 10
y is 11 and Result2 is 11
```

##### Example to understand Decrement Operators in C# Language:
```C#
using System;
namespace OperatorsDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            // Post-Decrement
            int x = 10;
            // Result1 is assigned 10 only,
            // x is not yet updated
            int Result1 = x--;
            //x becomes 9 now
            Console.WriteLine("x is {0} and Result1 is {1}", x, Result1);

            // Pre-Decrement 
            int y = 10;
            int Result2 = --y;
            //y and Result2 have same values i.e. 9
            Console.WriteLine("y is {0} and Result2 is {1}", y, Result2);

            Console.ReadKey();
        }
    }
}
```
output: 
```Plain Text
x is 9 and Result1 is 10
y is 9 and Result2 is 9
```

# Array

The array is defined as a collection of similar data elements. If you have some sets of integers, and some sets of floats, you can group them under one name as an array. So, in simple words, **`we can define an array as a collection of similar types of values that are stored in a contiguous memory location under a single name`**.

```C#
int[] employeeno = { 1, 2, 3, 4, 5};
```

How does using [] this work in real memory?

![image_149.png](image_149.png)

See using this [] after the data type, you are informing the compiler that the variable is an array and allocating a block of memory as specified by the array.

### Types of Arrays in C#:
C# supports 2 types of arrays. They are as follows:

* Single dimensional array
* Multi-dimensional array

In the Single dimensional array, the data is arranged in the form of a row whereas in the multi-dimensional arrays, the data is arranged in the form of rows and columns.

### How to declare an Array

General Syntax: `<data type>[] VariableName = new <data type>[size of the array];`

Example: `int[] A = new int[5];`

Here we have created an array with the name A and with a size of 5. So, you can store 5 values with the same name A. How it looks in the memory? It will allocate memory for 5 integers. These all 5 are types of int. For that memory, indexing will start from 0 onwards as shown in the below image.

![image_150.png](image_150.png)

### Assigning Values to Array

Thus, the first way of assigning values to the elements of an array is by doing so at the time of its declaration i.e. int[] n={1,2,3}; And the second method is declaring the array first and then assigning values to its elements as shown below.

![image_151.png](image_151.png)

### Creating and Initializing an Array at the Same Statement

```C#
using System;
namespace ArayDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating and Initializing an Array
            //Here, the size will be decided based on the number of elements
            //In this case size will be 3
            int[] Numbers = { 10, 20, 30 };

            //Accessing the Array Elements separately
            Console.WriteLine("Accessing the Array Elements separately");
            Console.WriteLine($"Numbers[0] = {Numbers[0]}");
            Console.WriteLine($"Numbers[1] = {Numbers[1]}");
            Console.WriteLine($"Numbers[2] = {Numbers[2]}");

            //Accessing the Array Elements using for Loop
            Console.WriteLine("\nAccessing the Array Elements using For Loop");
            for (int i = 0; i <= Numbers.Length - 1; i++)
            {
                Console.WriteLine($"Numbers[{i}] = {Numbers[i]}");
            }

            //Accessing the Array Elements using foreach Loop
            Console.WriteLine("\nAccessing the Array Elements using ForEach Loop");
            foreach (int Number in Numbers)
            {
                Console.WriteLine($"{Number}");
            }
            Console.ReadKey();
        }
    }
}
```
Output:

![image_152.png](image_152.png)

### Creating and Initializing an Array Separately

```C#
using System;
namespace ArayDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating an Integer Array with size 3
            int[] Numbers = new int[3];

            //Accessing the Array Elements Before Initialization
            Console.WriteLine("Accessing the Array Elements Before Initialization");
            for (int i = 0; i <= Numbers.Length - 1; i++)
            {
                Console.WriteLine($"Numbers[{i}] = {Numbers[i]}");
            }

            //Initializing the Array Elements using the Index Position
            Numbers[0] = 10;
            Numbers[1] = 20;
            Numbers[2] = 30;

            //Accessing the Array Elements After Initialization
            Console.WriteLine("\nAccessing the Array Elements After Initialization");
            for (int i = 0; i <= Numbers.Length - 1; i++)
            {
                Console.WriteLine($"Numbers[{i}] = {Numbers[i]}");
            }
            
            Console.ReadKey();
        }
    }
}
```
Output:

![image_153.png](image_153.png)

### Array Class in C#
The Array class in C# is a predefined class that is defined inside the System namespaces. This class is working as the base class for all the Arrays in C#. The Array class provides a set of members (methods and properties) to work with the `arrays such as creating, manipulating, searching, reversing, and sorting` the elements of an array, etc. The definition of the Array class in C# is gen below.

The Array Class in C# is not a part of the **`System.Collections`** namespace. It is a part of the System namespace. But still, we considered it as a collection because it Implement the **`IList interface`**.

1. **Sort(<array>)**: Sorting the array elements
2. **Reverse (<array>)**: Reversing the array elements
3. **Copy (src, dest, n)**: Copying some of the elements or all elements from the old array to the new array
4. **GetLength(int)**: A 32-bit integer that represents the number of elements in the specified dimension.
5. **Length**: It Returns the total number of elements in all the dimensions of the Array; zero if there are no elements in the array.

### Example to Understand Array Class Methods and Properties in C#

```C#
using System;
namespace ArayDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating and Initializing an Array of Integers
            //Size of the Array is 10
            int[] Numbers = { 17, 23, 4, 59, 27, 36, 96, 9, 1, 3 };

            //Printing the Array Elements using a for Loop
            Console.WriteLine("Original Array Elements :");
            for (int i = 0; i < Numbers.Length; i++)
            {
                Console.Write(Numbers[i] + " ");
            }
            Console.WriteLine();

            //Sorting the Array Elements by using the Sort method of Array Class
            Array.Sort(Numbers);
            //Printing the Array Elements After Sorting using a foreach loop
            Console.WriteLine("\nArray Elements After Sorting :");
            foreach (int i in Numbers)
            {
                Console.Write(i + " ");
            }
            Console.WriteLine();

            //Reversing the array elements by using the Reverse method of Array Class
            Array.Reverse(Numbers);
            //Printing the Array Elements in Reverse Order
            Console.WriteLine("\nArray Elements in the Reverse Order :");
            foreach (int i in Numbers)
            {
                Console.Write(i + " ");
            }
            Console.WriteLine();

            //Creating a New Array
            int[] NewNumbers = new int[10];
            //Copying Some of the Elements from Old array to new array
            //We declare the array with size 10 and we copy only 5 elements. 
            //So the rest 5 elements will take the default value. In this array, it will take 0
            Array.Copy(Numbers, NewNumbers, 5);

            //Printing the Array Elements using for Each Loop
            Console.WriteLine("\nNew Array Elements :");
            foreach (int i in NewNumbers)
            {
                Console.Write(i + " ");
            }

            Console.WriteLine();
            Console.WriteLine($"\nNew Array Length using Length Property :{NewNumbers.Length}");
            Console.WriteLine($"New Array Length using GetLength Method :{NewNumbers.GetLength(0)}");
            Console.ReadKey();
        }
    }
}
```
Output:

![image_154.png](image_154.png)

## 2Dimensional Array in C#:

Let us understand how to initialize a 2D Array with an example. Please have a look at the following statement which shows the declaration and initialization of a 2D Array.

`int[,] A = {{2, 5, 9},{6, 9, 15}};`

This is the declaration + initialization of a 2Dimensional array in C#. Here 2,5,9 is the 1st row and 6,9,15 is the 2nd row. This is how they will be filled and we can access any element with the help of two indexes that is row number and column number. Now, the other way of initializing it is,

````C#
int[,] A = new int[2,3]
{
    {2, 5, 9},{6, 9, 15}
};
````
### Accessing the Elements of the 2D array

For accessing all the elements of the rectangle 2D Array in C#, we require a nested for loop, one for loop for accessing the rows, and another for loop for accessing the columns.

![image_155.png](image_155.png)

````C#
using System;
namespace TwoDimensionalArayDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating a 2D Array with 4 Rows and 5 Columns
            int[,] RectangleArray = new int[4, 5];
            int a = 0;

            //Printing the values of 2D array using foreach loop
            //It will print the default values as we are not assigning
            //any values to the array
            foreach (int i in RectangleArray)
            {
                Console.Write(i + " ");
            }
            Console.WriteLine("\n");

            //Assigning values to the 2D array by using nested for loop
            //arr.GetLength(0): Returns the size of the Row
            //arr.GetLength(0): Returns the size of the Column
            for (int i = 0; i < RectangleArray.GetLength(0); i++)
            {
                for (int j = 0; j < RectangleArray.GetLength(1); j++)
                {
                    a += 5;
                    RectangleArray[i, j] = a;
                }
            }

            //Printing the values of array by using nested for loop
            //arr.GetLength(0): Returns the size of the Row
            //arr.GetLength(0): Returns the size of the Column
            for (int i = 0; i < RectangleArray.GetLength(0); i++)
            {
                for (int j = 0; j < RectangleArray.GetLength(1); j++)
                {
                    Console.Write(RectangleArray[i, j] + " ");
                }
            }
            Console.ReadKey();
        }
    }
}
````

Output:

![image_156.png](image_156.png)

#### Advantages of using an Array

1. It is used to `store similar types of multiple data items using a single name`.
2. We can use `arrays to implement other data structures such as linked lists, trees, graphs, stacks, queues`, etc.
3. The `two-dimensional arrays in C# are used to represent matrices.`
4. The` Arrays in C# are strongly typed`. That means they are used to store similar types of multiple data items using a single name.

#### Limitations of Array in C#:
1. `The array size is fixed.` Once the array is created we can never increase the size of the array. If we want then we can do it manually by creating a new array and copying the old array elements into the new array or by using the Array class Resize method which will do the same thing means to create a new array and copy the old array elements into the new array and then destroy the old array.
2. We can never `insert an element into the middle of an array`
3. `Deleting or removing elements from the middle of the array`.
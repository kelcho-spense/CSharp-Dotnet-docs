# ArrayList

The ArrayList in C# is a non-generic collection class that works like an array but provides the facilities such as `dynamic resizing`, `adding`, and `deleting elements` from the middle of a collection. 

## Properties of ArrayList Class in C#:
1. The `Elements can be added and removed` from the Array List collection at any point in time.
2. The `ArrayList is not guaranteed to be sorted`.
3. The `capacity of an ArrayList is the number of elements the ArrayList can hold`.
4. `Elements in this collection can be accessed using an integer index`. Indexes in this collection are zero-based.
5. It allows duplicate elements.

### How to Add Elements into ArrayList in C#?
The ArrayList non-generic class provides the `Add()` method which we can use to add elements to the array list or even we can use the object initializer syntax to add elements in an ArrayList. The most important point is that we can add multiple different types of elements in an ArrayList even though it is also possible to add duplicate elements.

```C#
using System;
using System.Collections;
namespace Csharp8Features
{
    public class ArrayListDemo
    {
        public static void Main()
        {
            //Adding elements to ArrayList using Add() method
            ArrayList arrayList1 = new ArrayList();
            arrayList1.Add(101); //Adding Integer Value
            arrayList1.Add("James"); //Adding String Value
            arrayList1.Add("James"); //Adding Duplicate Value
            arrayList1.Add(" "); //Adding Empty
            arrayList1.Add(true); //Adding Boolean
            arrayList1.Add(4.5); //Adding double
            arrayList1.Add(null); //Adding null

            foreach (var item in arrayList1)
            {
                Console.WriteLine(item);
            }

            //Adding Elements to ArrayList using object initializer syntax
            var arrayList2 = new ArrayList()
            {
                102, "Smith", "Smith", true, 15.6
            };

            foreach (var item in arrayList2)
            {
                Console.WriteLine(item);
            }
            Console.ReadKey();
        }
    }
}
```
Output:

![image_162.png](image_162.png)

### How to Access an ArrayList in C#?
If you go to the definition of ArrayList, then you will see that the ArrayList class implements the IList interface as shown in the below image. As it implements the IList interface, so we can access the elements using an indexer, in the same way as an array. The index starts from zero and increases by one for each subsequent element. So, when a collection class implements the IList interface, then we can access the elements of that collection by using the integer indexes.

![image_163.png](image_163.png)

```C#
using System;
using System.Collections;
namespace Csharp8Features
{
    public class ArrayListDemo
    {
        public static void Main()
        {
            //Adding elements to ArrayList using Add() method
            ArrayList arrayList1 = new ArrayList();
            arrayList1.Add(101); //Adding Integer Value
            arrayList1.Add("James"); //Adding String Value
            arrayList1.Add(true); //Adding Boolean
            arrayList1.Add(4.5); //Adding double

            //Accessing individual elements from ArrayList using Indexer
            int firstElement = (int)arrayList1[0]; //returns 101
            string secondElement = (string)arrayList1[1]; //returns "James"
            //int secondElement = (int) arrayList1[1]; //Error: cannot cover string to int
            Console.WriteLine($"First Element: {firstElement}, Second Element: {secondElement}");

            //Using var keyword without explicit casting
            var firsItem = arrayList1[0]; //returns 101
            var secondItem = arrayList1[1]; //returns "James"
            //var fifthElement = arrayList1[5]; //Error: Index out of range
            Console.WriteLine($"First Item: {firsItem}, Second Item: {secondItem}");

            //update elements
            arrayList1[0] = "Smith";
            arrayList1[1] = 1010;
            //arrayList1[5] = 500; //Error: Index out of range

            foreach (var item in arrayList1)
            {
                Console.Write($"{item} ");
            }
            Console.ReadKey();
        }
    }
} 
```
Output:

![image_164.png](image_164.png)

### How to Iterate an ArrayList in C#?
If you go to the definition of ArrayList, then you will also see that the `ArrayList non-generic collection` class implements the ICollection interface as shown in the below image. And we know the ICollection interface supports iteration of the collection types. So, we can either use the foreach loop and for loop to iterate a collection of type ArrayList.

![image_165.png](image_165.png)

The **`Count property of ArrayList returns the total number of elements present in an ArrayList`**. Let us understand this with an example.

```C#
using System;
using System.Collections;
namespace Csharp8Features
{
    public class ArrayListDemo
    {
        public static void Main()
        {
            //Adding elements to ArrayList using Add() method
            ArrayList arrayList1 = new ArrayList();
            arrayList1.Add(101); //Adding Integer Value
            arrayList1.Add("James"); //Adding String Value
            arrayList1.Add(true); //Adding Boolean
            arrayList1.Add(4.5); //Adding double

            //Iterating through foreach loop
            Console.WriteLine("Using ForEach Loop");
            foreach (var item in arrayList1)
            {
                Console.Write($"{item} ");
            }

            //Iterating through for loop
            Console.WriteLine("\n\nUsing For Loop");
            for (int i = 0; i < arrayList1.Count; i++)
            {
                Console.Write($"{arrayList1[i]} ");
            } 
            Console.ReadKey();
        }
    }
}
```
Output:

![image_166.png](image_166.png)

### How to Insert an Element into a Specified Position in an ArrayList Collection 
The Add method will add the element at the end of the collection i.e. after the last element. But, if you want to add the element at a specified position, then you need to use the **`Insert()`** method of the ArrayList class which will insert an element into the collection at the specified index position. The syntax to use the Insert method is given below.

`void Insert(int index, object? value);`

```C#
using System;
using System.Collections;
namespace Csharp8Features
{
    public class ArrayListDemo
    {
        public static void Main()
        {
            ArrayList arrayList = new ArrayList()
            {
                    101,
                    "James",
                    true,
                    10.20
            };
            
            //Insert "First Element" at First Position i.e. Index 0
            arrayList.Insert(0, "First Element");

            //Insert "Third Element" at Third Position i.e. Index 2
            arrayList.Insert(2, "Third Element");

            //Iterating through foreach loop
            foreach (var item in arrayList)
            {
                Console.WriteLine($"{item}");
            }
            Console.ReadKey();
        }
    }
}
```
Output:

![image_167.png](image_167.png)

### How to Remove Elements from ArrayList

If you want to remove elements from ArrayList in C#, then you can use Remove(), RemoveAt(), or RemoveRange() methods of the ArrayList class in C#.

* **Remove(object? obj)**: This method is used to `remove the first occurrence of a specific object` from the System.Collections.ArrayList. The parameter obj specifies the Object to remove from the ArrayList. The value can be null.
* **RemoveAt(int index)**: This method is used to `remove the element at the specified index` of the ArrayList. The parameter index specifies the index position of the element to remove.
* **RemoveRange(int index, int count)**: This method is used to `remove a range of elements from the ArrayList`. The parameter index specifies the starting index position of the range of elements to remove and the parameter count specifies the number of elements to remove.

```C#
using System;
using System.Collections;
namespace Csharp8Features
{
    public class ArrayListDemo
    {
        public static void Main()
        {
            ArrayList arrayList = new ArrayList()
            {
                    "India",
                    "USA",
                    "UK",
                    "Nepal",
                    "HongKong",
                    "Srilanka",
                    "Japan",
                    "Britem",
                    "HongKong",
            };

            Console.WriteLine("Array List Elements");
            foreach (var item in arrayList)
            {
                Console.Write($"{item} ");
            }

            arrayList.Remove("HongKong"); //Removes first occurance of null
            Console.WriteLine("\n\nArray List Elements After Removing First Occurances of HongKong");
            foreach (var item in arrayList)
            {
                Console.Write($"{item} ");
            }

            arrayList.RemoveAt(3); //Removes element at index postion 3, it is 0 based index
            Console.WriteLine("\n\nArray List1 Elements After Removing Element from Index 3");
            foreach (var item in arrayList)
            {
                Console.Write($"{item} ");
            }

            arrayList.RemoveRange(0, 2);//Removes two elements starting from 1st item (0 index)
            Console.WriteLine("\n\nArray List Elements After Removing First Two Elements");
            foreach (var item in arrayList)
            {
                Console.Write($"{item} ");
            }
            Console.ReadKey();
        }
    }
}
```

Output:

![image_168.png](image_168.png)

### How to Remove all the elements from the ArrayList
```C#
using System;
using System.Collections;
namespace Csharp8Features
{
    public class ArrayListDemo
    {
        public static void Main()
        {
            ArrayList arrayList = new ArrayList()
            {
                    "India",
                    "USA",
                    "UK",
                    "Denmark",
                    "Nepal",
            };

            int totalItems = arrayList.Count;
            Console.WriteLine(string.Format($"Total Items: {totalItems}, Capacity: {arrayList.Capacity}"));
            //Remove all items from the Array list             
            arrayList.Clear();

            totalItems = arrayList.Count;
            Console.WriteLine(string.Format($"Total Items After Clear(): {totalItems}, Capacity: {arrayList.Capacity}"));
            Console.ReadKey();
        }
    }
}
```
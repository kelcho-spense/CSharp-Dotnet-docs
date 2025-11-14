# Generic Collections

Generic Collections are introduced as part of `C# 2.0`. You can consider this **`Generic Collection as an extension to the Non-Generic Collection Classes`** which we have already discussed in our previous articles such as `ArrayList`, `Hashtable`, `SortedList`, `Stack`, and `Queue`.

## Problems with Non-Generic Collections in C#
The `Non-Generic Collection Classes such as ArrayList, Hashtable, SortedList, Stack, and Queue are worked on the object data type`. That means the elements added to the collection are of an **`object type`**. As these `Non-Generic CollectionClasses worked on object data type, we can store any type of value that may lead to runtime exceptions due to type mismatch`. But with **`Generic Collections, now we are able to store a specific type of data (whether a primitive type or a reference type) which provides type safety by eliminating the type mismatch at run time`**. For a better understanding, please have a look at the below example.

```C#
using System;
using System.Collections;
namespace CollectionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            ArrayList Numbers = new ArrayList(3);
            Numbers.Add(100);
            Numbers.Add(200);
            Numbers.Add(300);

            //It is also possible to store string values
            Numbers.Add("Hi");

            foreach (int Number in Numbers)
            {
                Console.Write(Number + "  ");
            }
            Console.ReadKey();
        }
    }
}
```
Output:

![image_174.png](image_174.png)

## The Solution to Non-Generic Collection Problems
The above two problems of Non-Generic Collections are overcome by using the Generic Collections in C#. The .NET Framework has re-implemented all the existing `Non-Generic Collection classes such as ArrayList, Hashtable, SortedList, Stack, and Queue`., etc. in **`Generic Collections such as ArrayList<T>, Dictionary<TKey, TValue>, SortedList<TKey, TValue>, Stack<T>, and Queue<T>`**. 

> Here **T** is nothing but the **`type of values that we want to store in the collection`**. So, the most important point that you need to remember is while creating the objects of Generic Collection Classes, you need to explicitly provide the type of values that you are going to store in the collection.

**`A Generic Collection is Strongly Type-Safe. Which type of data do you want to store in generic type, this information you have to provide at compile time. It means you can only put one type of data into it.`** This eliminates type mismatches at runtime.

The Generic Collection Classes are implemented under the System.Collections.Generic namespace. The classes which are present in this namespace are as follows.

1. `Stack<T>`: It represents a variable size last-in-first-out (LIFO) collection of instances of the same specified type.
2. `Queue<T>`: It represents a first-in, first-out collection of objects.
3. `HashSet<T>`: It represents a set of values. It eliminates duplicate elements.
4. `SortedList<TKey, TValue>`: It represents a collection of key/value pairs that are sorted by key based on the associated System.Collections.Generic.IComparer implementation. It automatically adds the elements in ascending order of key by default.
5. `List<T>`: It represents a strongly typed list of objects that can be accessed by index. Provides methods to search, sort, and manipulate lists. It grows automatically as you add elements to it.
6. `Dictionary<TKey, Tvalue>`: It represents a collection of keys and values.
7. `SortedSet<T>`: It represents a collection of objects that are maintained in sorted order.
8. `SortedDictionary<TKey, TValue>`: It represents a collection of key/value pairs that are sorted on the key.
9. `LinkedList<T>`: It represents a doubly linked list.

> **`Here the <T> refers to the type of values we want to store under them`**

Examples:

![image_175.png](image_175.png)

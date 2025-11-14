# Stack&lt;T&gt;

The `Stack<T> `class in C# (in the `System.Collections.Generic` namespace) stores elements in LIFO order — Last In, First Out.
You add items using `Push()` and remove the most recent one with `Pop()`.

```C#
using System;
using System.Collections.Generic;

namespace GenericStackDemo
{
    class Program
    {
        static void Main()
        {
            // Creating a Generic Stack of string type
            Stack<string> countries = new Stack<string>();

            // Pushing elements into the Stack
            countries.Push("India");
            countries.Push("USA");
            countries.Push("Japan");
            countries.Push("UK");

            Console.WriteLine("Stack Elements After Push Operations:");
            foreach (var country in countries)
            {
                Console.WriteLine(country);
            }

            // Peek() - returns the top element without removing it
            Console.WriteLine($"\nTop element (Peek): {countries.Peek()}");

            // Pop() - removes and returns the top element
            Console.WriteLine($"\nPopped Element: {countries.Pop()}");

            Console.WriteLine("\nStack Elements After Pop:");
            foreach (var country in countries)
            {
                Console.WriteLine(country);
            }

            // Contains() - checks if a specific element exists
            Console.WriteLine($"\nStack contains 'India'? {countries.Contains("India")}");

            // Count property - returns the number of elements
            Console.WriteLine($"\nNumber of Elements in Stack: {countries.Count}");

            // ToArray() - converts Stack to an array
            string[] stackArray = countries.ToArray();
            Console.WriteLine("\nStack Elements Converted to Array:");
            foreach (var item in stackArray)
            {
                Console.WriteLine(item);
            }

            // Clear() - removes all elements
            countries.Clear();
            Console.WriteLine($"\nStack Cleared. Number of Elements Now: {countries.Count}");

            Console.ReadKey();
        }
    }
}

```

| Method             | Description                                  |
| ------------------ | -------------------------------------------- |
| `Push(T item)`     | Adds an item to the top of the stack.        |
| `Pop()`            | Removes and returns the top item.            |
| `Peek()`           | Returns the top item without removing it.    |
| `Contains(T item)` | Checks if an item exists in the stack.       |
| `Count`            | Returns the number of elements in the stack. |
| `ToArray()`        | Copies elements into a new array.            |
| `Clear()`          | Removes all items from the stack.            |
| `foreach`          | Iterates through the stack elements.         |

# List&lt;T&gt;

The `List<T>` class (from `System.Collections.Generic`) is a strongly typed, dynamic collection that stores elements in a sequential manner.
It allows duplicates, maintains insertion order, and can grow or shrink dynamically at runtime.

It is the generic version of `ArrayList`, ensuring type safety and preventing type mismatches.

```C#
using System;
using System.Collections.Generic;

namespace GenericListDemo
{
    public class Program
    {
        public static void Main(string[] args)
        {
            // Creating a generic list of integers
            List<int> numbers = new List<int>();

            // Adding elements using Add()
            numbers.Add(11);
            numbers.Add(22);
            numbers.Add(55);
            numbers.Add(65);
            numbers.Add(10);

            Console.WriteLine("List after Add operations:");
            DisplayList(numbers);

            // Adding multiple elements at once using AddRange()
            numbers.AddRange(new List<int> { 99, 77, 88 });
            Console.WriteLine("\nList after AddRange():");
            DisplayList(numbers);

            // Inserting an element at a specific position using Insert()
            numbers.Insert(2, 100);
            Console.WriteLine("\nList after Insert(2, 100):");
            DisplayList(numbers);

            // Removing an element by value using Remove()
            numbers.Remove(55);
            Console.WriteLine("\nList after Remove(55):");
            DisplayList(numbers);

            // Removing an element by index using RemoveAt()
            numbers.RemoveAt(0);
            Console.WriteLine("\nList after RemoveAt(0):");
            DisplayList(numbers);

            // Contains() - checks if an element exists
            Console.WriteLine($"\nList contains 65? {numbers.Contains(65)}");

            // IndexOf() - gets the index of an element
            Console.WriteLine($"Index of 65: {numbers.IndexOf(65)}");

            // Count property - total number of elements
            Console.WriteLine($"\nTotal number of elements: {numbers.Count}");

            // Sort() - sorts elements in ascending order
            numbers.Sort();
            Console.WriteLine("\nList after Sort():");
            DisplayList(numbers);

            // Reverse() - reverses the order of elements
            numbers.Reverse();
            Console.WriteLine("\nList after Reverse():");
            DisplayList(numbers);

            // Clear() - removes all elements
            numbers.Clear();
            Console.WriteLine($"\nList cleared. Count = {numbers.Count}");

            Console.ReadKey();
        }

        // Helper method to display list elements
        public static void DisplayList(List<int> list)
        {
            foreach (var item in list)
            {
                Console.Write(item + " ");
            }
            Console.WriteLine();
        }
    }
}
```

| Method / Property           | Description                                      |
| --------------------------- | ------------------------------------------------ |
| `Add(T item)`               | Adds an element to the end of the list.          |
| `AddRange(IEnumerable<T>)`  | Adds multiple elements at once.                  |
| `Insert(int index, T item)` | Inserts an element at a specified position.      |
| `Remove(T item)`            | Removes the first matching element.              |
| `RemoveAt(int index)`       | Removes an element at a specific index.          |
| `Contains(T item)`          | Checks if an element exists.                     |
| `IndexOf(T item)`           | Returns the index of the first matching element. |
| `Sort()`                    | Sorts the list in ascending order.               |
| `Reverse()`                 | Reverses the list order.                         |
| `Count`                     | Returns the number of elements.                  |
| `Clear()`                   | Removes all elements.                            |
| `foreach`                   | Iterates over the list elements.                 |

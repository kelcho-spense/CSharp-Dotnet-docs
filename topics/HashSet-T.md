# HashSet&lt;T&gt;

The `HashSet<T>` class in C# (from the `System.Collections.Generic` namespace) represents a collection of unique elements — meaning duplicates are not allowed.
Unlike `SortedSet<T>`, it does not maintain order, but provides fast lookups and set operations like union, intersection, and difference.
```C#
using System;
using System.Collections.Generic;

namespace GenericHashSetDemo
{
    public class Program
    {
        public static void Main(string[] args)
        {
            // Creating a HashSet of integers
            HashSet<int> numbers = new HashSet<int>();

            // Adding elements using Add()
            numbers.Add(11);
            numbers.Add(22);
            numbers.Add(55);
            numbers.Add(65);

            // Trying to add duplicate element (ignored automatically)
            numbers.Add(55);

            Console.WriteLine("Elements in HashSet:");
            foreach (var num in numbers)
            {
                Console.Write(num + " ");
            }

            // Count property
            Console.WriteLine($"\n\nTotal Elements: {numbers.Count}");

            // Contains() - checks if a specific element exists
            Console.WriteLine($"Contains 22? {numbers.Contains(22)}");

            // Remove() - removes an element
            numbers.Remove(11);
            Console.WriteLine("\nAfter Removing 11:");
            foreach (var num in numbers)
            {
                Console.Write(num + " ");
            }

            // Working with another HashSet
            HashSet<int> otherNumbers = new HashSet<int>() { 33, 44, 55, 66 };

            // UnionWith() - adds all unique elements from another set
            numbers.UnionWith(otherNumbers);
            Console.WriteLine("\n\nAfter UnionWith(otherNumbers):");
            foreach (var num in numbers)
            {
                Console.Write(num + " ");
            }

            // IntersectWith() - keeps only common elements
            numbers.IntersectWith(otherNumbers);
            Console.WriteLine("\n\nAfter IntersectWith(otherNumbers):");
            foreach (var num in numbers)
            {
                Console.Write(num + " ");
            }

            // ExceptWith() - removes elements found in another set
            numbers.ExceptWith(new HashSet<int>() { 44 });
            Console.WriteLine("\n\nAfter ExceptWith({44}):");
            foreach (var num in numbers)
            {
                Console.Write(num + " ");
            }

            // Relationship checks
            HashSet<int> subset = new HashSet<int>() { 55 };
            Console.WriteLine($"\n\nIs Subset of otherNumbers? {subset.IsSubsetOf(otherNumbers)}");
            Console.WriteLine($"Is Superset of otherNumbers? {numbers.IsSupersetOf(otherNumbers)}");

            // Clear() - removes all elements
            numbers.Clear();
            Console.WriteLine($"\nAfter Clear(): Count = {numbers.Count}");

            Console.ReadKey();
        }
    }
}

```
> If you notice, we have added 55 elements two times. Now, run the application and you will see that it `removes the duplicate element and shows 55 only once as shown` in the below image.

output: 

![image_176.png](image_176.png)

| Method / Property               | Description                                       |
| ------------------------------- | ------------------------------------------------- |
| `Add(T item)`                   | Adds an element to the set (duplicates ignored).  |
| `Remove(T item)`                | Removes a specific element from the set.          |
| `Contains(T item)`              | Checks whether an element exists.                 |
| `Count`                         | Returns the number of elements in the set.        |
| `UnionWith(IEnumerable<T>)`     | Adds all unique elements from another set.        |
| `IntersectWith(IEnumerable<T>)` | Keeps only common elements between sets.          |
| `ExceptWith(IEnumerable<T>)`    | Removes elements found in another set.            |
| `IsSubsetOf(IEnumerable<T>)`    | Checks if the current set is a subset of another. |
| `IsSupersetOf(IEnumerable<T>)`  | Checks if the current set contains another set.   |
| `Clear()`                       | Removes all elements from the set.                |
| `foreach`                       | Iterates through elements (unordered).            |

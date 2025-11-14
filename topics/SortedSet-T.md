# SortedSet&lt;T&gt;

The `SortedSet<T>` class in C# (in the `System.Collections.Generic` namespace) represents a collection of unique elements that are automatically sorted in ascending order by default.

* Duplicate elements are ignored.
* The sorting order can be customized using an `IComparer<T>` if needed.
* It’s ideal when you want fast lookups, no duplicates, and sorted order.

```C#
using System;
using System.Collections.Generic;

namespace GenericSortedSetDemo
{
    public class Program
    {
        public static void Main(string[] args)
        {
            // Creating a SortedSet of integers
            SortedSet<int> numbers = new SortedSet<int>();

            // Adding elements using Add() method
            numbers.Add(55);
            numbers.Add(22);
            numbers.Add(88);
            numbers.Add(11);
            numbers.Add(77);
            numbers.Add(66);

            // Trying to add a duplicate element (ignored automatically)
            numbers.Add(55);

            Console.WriteLine("Elements in SortedSet (Ascending Order):");
            foreach (var num in numbers)
            {
                Console.Write(num + " ");
            }

            // Count property
            Console.WriteLine($"\n\nTotal Elements: {numbers.Count}");

            // Contains() - checks if an element exists
            Console.WriteLine($"Contains 77? {numbers.Contains(77)}");

            // Remove() - deletes an element
            numbers.Remove(22);
            Console.WriteLine("\nAfter Removing 22:");
            foreach (var num in numbers)
            {
                Console.Write(num + " ");
            }

            // Min and Max properties
            Console.WriteLine($"\n\nMinimum Element: {numbers.Min}");
            Console.WriteLine($"Maximum Element: {numbers.Max}");

            // Working with another SortedSet
            SortedSet<int> otherNumbers = new SortedSet<int>() { 33, 44, 55, 66, 77 };

            // UnionWith() - combines unique elements from both sets
            numbers.UnionWith(otherNumbers);
            Console.WriteLine("\nAfter UnionWith(otherNumbers):");
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
            numbers.ExceptWith(new SortedSet<int>() { 55, 77 });
            Console.WriteLine("\n\nAfter ExceptWith({55, 77}):");
            foreach (var num in numbers)
            {
                Console.Write(num + " ");
            }

            // Clear() - removes all elements
            numbers.Clear();
            Console.WriteLine($"\n\nAfter Clear(): Count = {numbers.Count}");

            Console.ReadKey();
        }
    }
}

```
> As you notice in the above-sorted set, we have added 55 elements two times. Now, run the application and you will see that it removes the duplicate element and shows 55 only once as well as it will sort the elements in ascending order as shown in the below image.

output: 

![image_177.png](image_177.png)

| Method / Property               | Description                                       |
| ------------------------------- | ------------------------------------------------- |
| `Add(T item)`                   | Adds an element to the set (ignores duplicates).  |
| `Remove(T item)`                | Removes a specified element.                      |
| `Contains(T item)`              | Checks whether an element exists.                 |
| `Count`                         | Returns the number of elements in the set.        |
| `Min` / `Max`                   | Returns the smallest and largest element.         |
| `UnionWith(IEnumerable<T>)`     | Adds all elements from another set (unique only). |
| `IntersectWith(IEnumerable<T>)` | Retains only elements found in both sets.         |
| `ExceptWith(IEnumerable<T>)`    | Removes elements found in another set.            |
| `Clear()`                       | Removes all elements from the set.                |
| `foreach`                       | Iterates through the elements in ascending order. |

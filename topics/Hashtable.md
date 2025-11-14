# Hashtable

The Hashtable in C# is **`a Non-Generic Collection that stores the element in the form of “Key-Value Pairs”`**. The data in the Hashtable are organized based on the hash code of the key. The key in the HashTable is defined by us and more importantly, that key can be of any data type. Once we created the Hashtable collection, `then we can access the elements by using the keys`.

## Hashtable Characteristics in C#
1. The Hashtable Collection Class in C# stores the elements in the form of key-value pairs.
2. Hashtable Class belongs to System.Collection namespace i.e. it is a Non-Generic Collection class.
3. It implements the IDictionary interface.
4. The Keys can be of any data type but they must be unique and not null.
5. The Hashtable accepts both null and duplicate values.
6. We can access the values by using the associated key.
7. The capacity of a Hashtable is the number of elements that a Hashtable can hold.
8. A hashtable is a non-generic collection, so we can store elements of the same type as well as of different types.

One more point that you need to remember is that the performance of the hashtable is less as compared to the ArrayList because of this key conversion (converting the key to an integer hashcode).

![image_169.png](image_169.png)

### How to Add Elements into a Hashtable Collection in C#?
Now, if you want to add elements i.e. a key/value pair into the hashtable, then you need to use Add() method of the Hashtable class.

Add(object key, object? value): The Add(object key, object? value) method is used to add an element with the specified key and value into the Hashtable. Here, the parameter key specifies the key of the element to add and the parameter value specifies the value of the element to add. The value can be null.

For Example

```C#
hashtable.Add(“EId”, 1001);
```

```C#
hashtable.Add(“Name”, “James”);
```

Even it is also possible to create a Hashtable using collection-initializer syntax as follows:
```C#
var cities = new Hashtable(){
    {“UK”, “London, Manchester, Birmingham”},
    {“USA”, “Chicago, New York, Washington”},
    {“India”, “Mumbai, Delhi, BBSR”}
};
```

### How to access a Non-Generic Hashtable Collection
ou can access the individual value of the Hashtable in C# by using the keys. In this case, we need to pass the key as a parameter to find the respective value. If the specified key is not present, then the compiler will throw an exception. The syntax is given below.
```C#
hashtable[“EId”]
hashtable[“Name”]
```

### Create a Hashtable and Add Elements
```C#
using System;
using System.Collections;
namespace HasntableExample
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating Hashtable collection with default constructor
            Hashtable hashtable = new Hashtable();

            //Adding elements to the Hash table using key value pair
            hashtable.Add("EId", 1001); //Here key is Eid and value is 1001
            hashtable.Add("Name", "James"); //Here key is Name and value is James
            hashtable.Add("Salary", 3500); //Here key is Salary and value is 3500
            hashtable.Add("Location", "Mumbai"); //Here key is Location and value is Mumbai
            hashtable.Add("EmailID", "a@a.com"); //Here key is EmailID and value is a@a.com

            //Printing the keys and their values using foreach loop
            Console.WriteLine("Printing Hashtable using Foreach Loop");
            foreach (object obj in hashtable.Keys)
            {
                Console.WriteLine(obj + " : " + hashtable[obj]);
            }
            //Or
            //foreach (DictionaryEntry de in hashtable)
            //{
            //    Console.WriteLine($"Key: {de.Key}, Value: {de.Value}");
            //}

            Console.WriteLine("\nPrinting Hashtable using Keys");
            //I want to print the Location by using the Location key
            Console.WriteLine("Location : " + hashtable["Location"]);

            //I want to print the Email ID by using the EmailID key
            Console.WriteLine("Emaild ID : " + hashtable["EmailID"]);

            Console.ReadKey();
        }
    }
}
```
Output:

![image_170.png](image_170.png)

### Add Elements to a Hashtable using Collection Initializer
```C#
using System;
using System.Collections;

namespace HasntableExample
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating a Hashtable using collection-initializer syntax
            Hashtable citiesHashtable = new Hashtable(){
                            {"UK", "London, Manchester, Birmingham"}, //Key:UK, Value:London, Manchester, Birmingham
                            {"USA", "Chicago, New York, Washington"}, //Key:USA, Value:Chicago, New York, Washington
                            {"India", "Mumbai, New Delhi, Pune"}      //Key:India, Value:Mumbai, New Delhi, Pune
                        };

            Console.WriteLine("Creating a Hashtable Using Collection-Initializer");
            foreach (DictionaryEntry city in citiesHashtable)
            {
                Console.WriteLine($"Key: {city.Key}, Value: {city.Value}");
            }

            Console.ReadKey();
        }
    }
}
```
Output:

![image_171.png](image_171.png)

### How to Check the Availability of a key/value Pair in a Hashtable
f you want to check whether the key/value pair exists or not in the Hashtable, then you can use the following methods of the Hashtable class.

1. **Contains(object key)**: The Contains(object key) method of the Hashtable is used to check whether the Hashtable contains a specific key. The parameter key to locating in the Hashtable object. It returns true if the Hashtable contains an element with the specified key; otherwise, false. If the key is null, then it will throw System.ArgumentNullException.
2. **ContainsKey(object key)**: The ContainsKey(object key) method of the Hashtable is used to check if a given key is present in the Hashtable or not. The parameter key to locating in the Hashtable object. If the given key is present in the collection then it will return true else it will return false. If the key is null, then it will throw System.ArgumentNullException.
3. **ContainsValue(object value)**: The ContainsValue(object value) Method of the Hashtable class is used to check if a value is present in the Hashtable or not. The parameter value to locate in the hashtable object. If the given value is present in the collection then it will return true else it will return false.

```C#
using System;
using System.Collections;

namespace HasntableExample
{
    class Program
    {
        static void Main(string[] args)
        {
            //Creating Hashtable collection with default constructor
            Hashtable hashtable = new Hashtable();

            //Adding elements to the Hash table using key value pair
            hashtable.Add("EId", 1001); //Here key is Eid and value is 1001
            hashtable.Add("Name", "James"); //Here key is Name and value is James
            hashtable.Add("Job", "Developer");
            hashtable.Add("Salary", 3500);
            hashtable.Add("Location", "Mumbai");
            hashtable.Add("Dept", "IT");
            hashtable.Add("EmailID", "a@a.com");

            //Checking the key using the Contains methid
            Console.WriteLine("Is EmailID Key Exists : " + hashtable.Contains("EmailID"));
            Console.WriteLine("Is Department Key Exists : " + hashtable.Contains("Department"));

            //Checking the key using the ContainsKey methid
            Console.WriteLine("Is EmailID Key Exists : " + hashtable.ContainsKey("EmailID"));
            Console.WriteLine("Is Department Key Exists : " + hashtable.ContainsKey("Department"));

            //Checking the value using the ContainsValue method
            Console.WriteLine("Is Mumbai value Exists : " + hashtable.ContainsValue("Mumbai"));
            Console.WriteLine("Is Technology value Exists : " + hashtable.ContainsValue("Technology"));

            Console.ReadKey();
        }
    }
}
```
Output:

![image_172.png](image_172.png)

### How to Remove Elements from a Non-Generic Hashtable Collection
If you want to remove an element from the Hashtable, then you can use the following Remove method of the C# Hashtable class.

- `Remove(object key)`: This method is used to remove the element with the specified key from the Hashtable. Here, the parameter key specifies the element to remove.
- `Clear()`: This method is used to remove all elements from the Hashtable

```C#
using System;
using System.Collections;

namespace HasntableExample
{
    class Program
    {
        static void Main(string[] args)
        {
            Hashtable employee = new Hashtable
            {
                { "EId", 1001 },
                { "Name", "James" },
                { "Salary", 3500 },
                { "Location", "Mumbai" },
                { "EmailID", "a@a.com" }
            };

            //Count Property returns the number of elements present in the collection
            Console.WriteLine($"Hashtable Total Elements: {employee.Count}");

            // Remove EId as this key exists
            employee.Remove("EId");
            Console.WriteLine($"After Removing EID Total Elements: {employee.Count}");

            //Following statement throws run-time exception: KeyNotFoundException
            //employee.Remove("City"); 

            //Check key before removing it
            if (employee.ContainsKey("City"))
            {
                employee.Remove("City");
            }
            else
            {
                Console.WriteLine("Hashtable doesnot contain City key");
            }

            //Removes all elements
            employee.Clear();
            Console.WriteLine($"After Clearing Hashtable Total Elements: {employee.Count}");

            Console.ReadKey();
        }
    }
}
```
Output:

![image_173.png](image_173.png)
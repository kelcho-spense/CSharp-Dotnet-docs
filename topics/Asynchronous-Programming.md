# Asynchronous Programming

## What is Concurrency?
Concurrency means doing several things at the same time. For example, if we have to do a million tasks, then instead of doing them sequentially one by one, we can do them simultaneously, thus reducing the duration of the program execution.

This concept of having **`a Set of Tasks`** and dividing them into several parts and those several parts can be `performed simultaneously` is called **`Parallelism`**.

## How we can Achieve Parallelism in Programming?
In Programming, `we can achieve Parallelism by using threads`. _A thread is a sequence of instructions that can be executed independently of other code_. Since they are independent within a process, so we can have several threads. And if our processor allows, then we can run several threads simultaneously. When we are executing multiple threads simultaneously, then it is called multithreading. So, Parallelism uses multiple threads to perform multiple tasks simultaneously. Therefore, parallelism uses multithreading and multithreading is a form of concurrency.

> **`Asynchronous Programming allows us to use threads efficiently and threads are prevented from being unnecessarily blocked.`**

## Introduction to Asynchronous Programming
Asynchronous Programming allows us to handle the threads of our processes in a more efficient way. The idea is to avoid blocking a thread while waiting for a response, either from an external system such as a Web service or from the computer’s file management system.

For example, if we have a web application, it will be able to serve more **HTTP requests** at the same time by using Asynchronous Programming. `This is because each HTTP request is handled by a thread`, **`and if we avoid blocking threads, then there will be more threads available to process HTTP requests`**.

To work with asynchronous programming in C# we use **async** and **await** keywords. The idea is that `we need to use the async keyword to mark a method as asynchronous` and `with await, we can wait for an asynchronous operation in such a way that the original thread is not blocked`.

The method which is marked with the `async` keyword must return a **`Task`** or **`Task<T>`**. `The idea of a Task is that it represents an asynchronous operation and does not return anything.` **`In the case of Task<T>, it is like a promise that in the future this method will return a value of the data type T`**.

### Async and Await
In modern C# code, in order to use asynchronous programming, we need to use async and await keywords. The idea is that if we have a method in which we want to use asynchronous programming, then we need to mark the method with the async keyword as shown in the below image.

![image_180.png](image_180.png)

For those asynchronous operations for which we do not want to block the execution thread i.e. the current thread, we can use the await operator as shown in the below image.

![image_181.png](image_181.png)

#### Example to Understand Async and Await
Please have a look at the below example. It’s a very simple example. Inside the main method, first, we print that main method started, then we call the SomeMethod. Inside the SomeMethod, first, we print that SomeMethod started and then the thread execution is sleep for 10. After 10 seconds, it will wake up and execute the other statement inside the SomeMethod method. Then it will come back to the main method, where we called SomeMethod. And finally, it will execute the last print statement inside the main method.

```C#
using System;
using System.Threading;
namespace AsynchronousProgramming
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Main Method Started......");

            SomeMethod();

            Console.WriteLine("Main Method End");
            Console.ReadKey();
        }

        public static void SomeMethod()
        {
            Console.WriteLine("Some Method Started......");

            Thread.Sleep(TimeSpan.FromSeconds(10));
            Console.WriteLine("\n");
            Console.WriteLine("Some Method End");
        }
    }
}
```

When you execute the above code, you will see that after printing SomeMethod Started……, the console window is frozen for 10 seconds. This is because here we are not using asynchronous programming. One thread i.e. the Main thread is responsible for executing the code And when we call Thread.Sleep method the current thread is blocked for 10 seconds. This is a bad user experience

![image_182.png](image_182.png)

Now, let us see how we can overcome this problem by using asynchronous programming.

## Task in C#
A task is basically a “promise” that the operation to be performed will not necessarily be completed immediately, but that it will be completed in the future.

## What is the difference between Task and Task<T> in C#?
`Task` and `Task<T>` in C# for the return data type of an asynchronous method, the difference is that the **`Task is for methods that do not return a value`** while the **`Task<T> is for methods that do return a value of type T where T can be of any data type`**, such as a string, an integer, and a class, etc. 

> We know from basic C# that a method that `does not return a value is marked with a void`. **`This is something to avoid in asynchronous methods`**. So, don’t use async void except for event handlers.

### Example to Understand Task in C#
In our previous example, we have written the following SomeMethod.

```C#
public async static void SomeMethod()
{
    Console.WriteLine("Some Method Started......");

    await Task.Delay(TimeSpan.FromSeconds(10));

    Console.WriteLine("\nSome Method End");
}
```

Now, what we will do is we will move the Task.Dealy to separate method and call that method inside the SomeMethod. So, let’s create a method with the name Wait as follows. Here, we mark the method as async so it is an asynchronous method that will not block the currently executing thread. And when calling this method it will wait for 10 seconds. And more importantly, here we use the return type as Task as this method is not going to return anything.

```C#
private static async Task Wait()
{
    await Task.Delay(TimeSpan.FromSeconds(10));
  Console.WriteLine("\n10 Seconds wait Completed\n");
}
```

**`In asynchronous programming when your method does not return anything, then instead of using void you can use Task`**. Now, from the SomeMethod we need to call the Wait method. If we call the Wait method like the below then we will get a warning.

```C#
public async static void SomeMethod()
{
    Console.WriteLine("Some Method Started......");

    Wait();

    Console.WriteLine("Some Method End");
}
```

Here, you can see green lines under the Wait method as shown in the below image.

![image_183.png](image_183.png)

**Why is that?**
This is because the Wait method returns a Task and because it does return a Task, then it means that this will be a promise. So, this warning of the Wait method informed us that, if we don’t use the await operator while calling the Wait method, the Wait method is not going to wait for this operation to finish, which means that once we call the Wait method, the next line of code inside the SomeMethod is going to be executed immediately.
Let us see that practically. The following is the complete example code.

```C#
using System;
using System.Threading.Tasks;

namespace AsynchronousProgramming
{
    class Program
    {
        public static void Main(string[] args)
        {
            Console.WriteLine("Main Method Started......");

            SomeMethod();

            Console.WriteLine("Main Method End");
            Console.ReadKey();
        }

        public async static void SomeMethod()
        {
            Console.WriteLine("Some Method Started......");
            await Wait();
            Console.WriteLine("Some Method End");
        }

        private static async Task Wait()
        {
            await Task.Delay(TimeSpan.FromSeconds(10));
            Console.WriteLine("\n10 Seconds wait Completed\n");
        }
    }
} 
```
Output:

![image_184.png](image_184.png)

Now, you can observe in the above output that once it calls the Wait method, then the SomeMethod will wait for the Wait method to complete its execution. You can see that before printing the last print statement of the SomeMethod, it prints the printing statement of the Wait method. Hence, it proves that when we use await operator then the current method execution waits until the called async method completes its execution. Once the async method, in our example Wait method, complete its example, then the calling method, in our example SomeMethod, will continue its execution i.e. it will execute the statement which is present after the async method call.

## How to Return a Value from a Task in C#?
The .NET Framework also provides a generic version of the Task class i.e. Task<T>. Using this Task<T> class we can return data or values from a task. In Task<T>, T represents the data type that you want to return as a result of the task. With Task<T>, we have the representation of an asynchronous method that is going to return something in the future. That something could be a string, a number, a class, etc.

### Example to Understand Task<T>
Let us understand this with an example. What are we going to do is,
we are going to communicate with a Web API that is deployed and is live, and we will try to retrieve the users that we receive from the Web API.



```c#
using System;
using System.Net.Http;
using System.Threading.Tasks;

namespace AsynchronousProgramming
{
    class Program
    {
        // Create a single HttpClient instance to reuse across calls (singleton pattern)
        private static readonly HttpClient _httpClient;

        // Static constructor to initialize the HttpClient
        static Program()
        {
            _httpClient = new HttpClient
            {
                BaseAddress = new Uri("https://jsonplaceholder.typicode.com/"),
                Timeout = TimeSpan.FromSeconds(30)
            };
            _httpClient.DefaultRequestHeaders.Accept.Clear();
            _httpClient.DefaultRequestHeaders.Accept.Add(
                new System.Net.Http.Headers.MediaTypeWithQualityHeaderValue("application/json"));
        }

        public static async Task Main(string[] args)
        {
            Console.WriteLine("Main Method Started......");

            try
            {
            // get all users
                Console.WriteLine("\nAll User info:");
                string users = await FetchUsersAsync();
                Console.WriteLine(users);
                
            // get a user with id 
            Console.Write("Enter the id to search users: ");
            string id = Console.ReadLine();   
            
            string userJson = await FetchUserAsync(id);
            Console.WriteLine("\nUser infor:");
            Console.WriteLine(userJson); 
                
                
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error: {ex.Message}");
            }

            Console.WriteLine("\nMain Method End");
            Console.ReadKey();
        }

        public static async Task<string> FetchUsersAsync()
        {
            HttpResponseMessage response = await _httpClient.GetAsync("users");
            response.EnsureSuccessStatusCode();

            string users = await response.Content.ReadAsStringAsync();
            return users;
        }

        public static async Task<string> FetchUserAsync(string id)
        {
            if (string.IsNullOrWhiteSpace(id))
                throw new ArgumentException("ID must be provided", nameof(id));

            HttpResponseMessage response = await _httpClient.GetAsync($"users/{id}");
            response.EnsureSuccessStatusCode();

            string user = await response.Content.ReadAsStringAsync();
            return user;
        }
    }
}
```

---
### Key Concepts in the Code:

#### 1. **HttpClient as a Singleton**

* **HttpClient** is used to make HTTP requests. The code creates a static instance of `HttpClient` (a singleton pattern) so that it can be reused across multiple requests. This is important because creating new `HttpClient` instances for each request can lead to resource exhaustion (e.g., socket exhaustion).

```c#
private static readonly HttpClient _httpClient;
static Program()
{
    _httpClient = new HttpClient
    {
        BaseAddress = new Uri("https://jsonplaceholder.typicode.com/"),
        Timeout = TimeSpan.FromSeconds(30)
    };
    _httpClient.DefaultRequestHeaders.Accept.Clear();
    _httpClient.DefaultRequestHeaders.Accept.Add(
        new System.Net.Http.Headers.MediaTypeWithQualityHeaderValue("application/json"));
}
```

* The static constructor ensures that the `HttpClient` instance is initialized once when the program starts. This is crucial for performance and resource management.

#### 2. **Asynchronous Methods: FetchUsersAsync and FetchUserAsync**

The methods `FetchUsersAsync` and `FetchUserAsync` are asynchronous methods. They are declared with the `async` keyword and return a `Task<string>`, which represents an operation that will eventually produce a string result.

**`FetchUsersAsync`**:
This method retrieves all users from the API asynchronously:

```c#
public static async Task<string> FetchUsersAsync()
{
    HttpResponseMessage response = await _httpClient.GetAsync("users");
    response.EnsureSuccessStatusCode();

    string users = await response.Content.ReadAsStringAsync();
    return users;
}
```

* `await _httpClient.GetAsync("users")`: This makes an HTTP GET request to retrieve the list of users. The `await` keyword means that this line will **not block** the thread while waiting for the HTTP request to complete. Instead, the control returns to the calling method (`Main`) and the program continues executing other code.
* After the request is complete, the program continues and checks whether the request was successful using `response.EnsureSuccessStatusCode()`. If the request fails, it throws an exception.
* `await response.Content.ReadAsStringAsync()`: This asynchronously reads the response content as a string.

**`FetchUserAsync`**:
This method retrieves information for a specific user by ID:

```c#
public static async Task<string> FetchUserAsync(string id)
{
    if (string.IsNullOrWhiteSpace(id))
        throw new ArgumentException("ID must be provided", nameof(id));

    HttpResponseMessage response = await _httpClient.GetAsync($"users/{id}");
    response.EnsureSuccessStatusCode();

    string user = await response.Content.ReadAsStringAsync();
    return user;
}
```

* It first checks if the provided `id` is valid. If the `id` is invalid (empty or null), it throws an exception.
* The same `await` pattern is used to make the HTTP GET request to fetch a user by ID. The program does not block while waiting for the response.
* After the request completes successfully, it reads the content and returns it.

#### 3. **Main Method (Entry Point)**

The `Main` method is marked as `async Task`, which allows it to contain asynchronous code using the `await` keyword.

```c#
public static async Task Main(string[] args)
{
    Console.WriteLine("Main Method Started......");

    try
    {
        Console.WriteLine("\nAll User info:");
        string users = await FetchUsersAsync(); // Fetch all users
        Console.WriteLine(users);

        Console.Write("Enter the id to search users: ");
        string id = Console.ReadLine();

        string userJson = await FetchUserAsync(id); // Fetch a specific user by ID
        Console.WriteLine("\nUser info:");
        Console.WriteLine(userJson);
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error: {ex.Message}");
    }

    Console.WriteLine("\nMain Method End");
    Console.ReadKey();
}
```

* **First Task**: The program first calls `FetchUsersAsync` using `await`, which fetches all users asynchronously. The program doesn't block while waiting for the response.
* **Second Task**: After displaying the users, it asks the user to input an ID and fetches that specific user using `FetchUserAsync` with the `await` keyword.
* The method does not block the main thread during these operations, meaning the application remains responsive, and the user can continue interacting with it (e.g., entering the user ID).

---
# Dependency Injection in ASP.NET Core Web API

Building modern applications requires writing clean, testable, and maintainable code. One of the most common challenges developers face is **managing dependencies**, which is the way one class relies on another to perform its work.

## Need for Dependency Injection in ASP.NET Core

In Traditional Applications, classes often create and manage their own dependencies using the new keyword. This leads to Tight Coupling between classes, making the code difficult to maintain, extend, and test. So, when components create their own dependencies (tight coupling), we get several pain points:

* **Tight Coupling**: Changing a concrete implementation (e.g., swapping an in‑memory store for a database) triggers edits across the codebase.
* **Difficult Testing**: Unit testing becomes more challenging as it is difficult to easily substitute dependencies with mocks.
* **Code Duplication**: Instantiating the same services in multiple places leads to redundancy.
* **Violation of SRP and OCP**: Your classes end up doing too much, and changes require touching multiple parts of the system.

## What is the Dependency Injection (DI) Design Pattern?

**Dependency Injection** is a **Design Pattern** that **Removes Dependency Creation** from a class and instead provides (injects) them externally.

* **Without DI**: Class A creates an instance of Class B.
* **With DI**: Class A receives an instance of Class B from outside (Container).

### Components involved in the Dependency Injection (DI) Design Pattern
There are four components involved in the Dependency Injection Design Pattern. For a better understanding, please have a look at the following diagram. Here, Service represents two things: the Interface and its implementation class.

![image_269.png](image_269.png)

#### Abstraction (Interface / Contract)

An abstraction is like a **contract** that defines what the service can do, but not how it does it. In C#, this is usually an **interface**. The client depends only on this abstraction, not on the actual service implementation. This way, the client doesn’t care which service it gets, as long as it follows the contract.

**Key Points:**

* Defines what needs to be done, not how.
* Usually written as an **interface** in code (e.g., `IProductService`).
* Makes the system **flexible** — you can easily swap implementations.
* Keeps the **client (user)** free from knowing service details.

#### Service (Dependency / Implementation)
A service is the class that actually does the work. It provides some functionality that other parts of the application need. It contains the logic that your application needs, like sending an email, saving data to a database, or processing a payment. Without the service, your program cannot complete its tasks.

**Key Points:**

* It is the **real worker** of your system.
* Contains the actual **business logic**.
* It is the **dependency** that other classes need.
* Examples: EmailService, ProductRepository, PaymentService.
* Can have **multiple versions** (e.g., EmailService using Gmail or Outlook).

#### Client (Consumer / Dependent Class)
A client is the class that **needs the service** to perform its job. It doesn’t do the actual work itself but depends on a service to get things done. The client depends on the service but does not care about how it is created. For example, an OrderController needs an IEmailService to send order confirmation emails.

**Key Points:**

* It is the **user of the service**.
* Relies on an abstraction (interface), not on the concrete service.
* It only **uses** the service; it does not **create** it.
* Example: A controller using IProductService for CRUD operations.
* Becomes **simpler and cleaner** because it doesn’t create dependencies on its own.

#### Injector (Dependency Injection Container)
The injector is like the **manager** who provides the right worker (service) to the client. In ASP.NET Core, this is the **built-in DI container**. It creates service objects, gives them to the classes that need them, and manages their lifetime (i.e., how long they live).

**Key Points:**

Also called the **DI container**.
**Registers** services with their matching interfaces.
**Resolves** dependencies when a client needs them.
**Injects** the correct implementation at runtime.
Manages **lifetimes**: Transient (new every time), Scoped (per request), Singleton (once for the whole app).

## Types of Services in ASP.NET Core: Custom vs Built-in Services
They are mainly of two types:

#### Built-in Services:

ASP.NET Core provides many built-in services, such as:

* **Logging** (ILogger<T>),
* **Configuration** (IConfiguration),
* **Options** (IOptions<T>, IOptionsMonitor<T>),
* **HTTP** (IHttpClientFactory),
* **Caching** (IMemoryCache, IDistributedCache),
* **Authentication/Authorization**, **Routing/MVC** services, etc.

#### Custom Services:

You can create your own services (e.g., `IStudentRepository`, `EmailService`) and register them in the DI container

* Email sending (IEmailService).
* Background job scheduler (IJobService).
* Notification manager (INotificationService).
* Third-party API integrations (e.g., SMS, payments).

## How to Register a Service with the ASP.NET Core DI Container?

We need to register a service with the ASP.NET Core Dependency Injection Container within the Main method of the Program class. All registrations happen in the Program.cs (composition root) using builder.Services. ASP.NET Core provides three main lifetimes:

* **Singleton**: One instance throughout the application. Syntax: `builder.Services.AddSingleton<IProductRepository, ProductRepository>();`
* **Scoped**: One instance per HTTP request. Syntax: `builder.Services.AddScoped<IProductRepository, ProductRepository>();`
* **Transient**: New instance every time it’s requested. Syntax: `builder.Services.AddTransient<IProductRepository, ProductRepository>();`

## Different Ways of Accessing Dependencies in ASP.NET Core Web API
There are 3 different approaches that we can use to inject the dependency object into a class in ASP.NET Core.

* **Constructor Injection** – The Recommended Approach
* Action Method Injection – For Occasional Dependencies
* Manual Service Resolution – Use Only When Necessary

#### Constructor Injection (The Recommended Approach)

Constructor Injection is the most commonly used and recommended method of injecting dependencies. The DI container automatically creates and passes dependencies to the class constructor at runtime. This ensures your object is fully initialized and ready to perform its intended tasks

![image_270.png](image_270.png)

**Real-Time Use Cases:**
* Business services like IProductService, IOrderService, ILogger<T>, or DbContext.
* When the service is needed by multiple action methods in the same controller.
* Example: A ProductsController needs IProductService for CRUD operations

#### Action-Method Injection (For Occasional Dependencies)

If a service is needed **only inside a specific action, one** or **a few** endpoints (not the whole controller), injecting it into the controller constructor would be unnecessary. Instead, use `[FromServices]` in the method signature, i.e., inject it at the action parameter `using [FromServices]`.

Syntax:

![image_271.png](image_271.png)

Real-Time Use Cases
* Dependencies that are **used only in one or two endpoints**
* A service used for **special cases, such as** sending an email notification, generating a report, or exporting data.
**Example**:
  * A **one-off service** for generating PDF reports (IReportService) used in only one endpoint.
  * A **temporary feature**, such as **ICaptchaValidator**, is used only in the registration API.

#### Manually Resolving Services (Use Only When Necessary)

You need to use `HttpContext.RequestServices.GetService<T>()` to manually resolve a service at runtime. This is useful in **middleware**, **filters**, or in situations where injection isn’t possible.

**Syntax:**

![image_272.png](image_272.png)
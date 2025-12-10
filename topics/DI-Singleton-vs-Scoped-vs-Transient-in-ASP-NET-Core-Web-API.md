# DI : Singleton vs Scoped vs Transient in ASP.NET Core Web API

## Singleton Service in ASP.NET Core
A Singleton Service is the simplest and most powerful way to share one instance across the entire Application Lifetime. That means:

* It is **created only once** (at application startup or on the first time it’s requested).
* It is **then reused across all HTTP requests, all controllers, and all users**.
* The same object instance lives until the application shuts down.

![image_273.png](image_273.png)

#### Why Singleton Service in ASP.NET Core Web API?
Singleton services are best suited when you want a single shared resource that does not depend on request-specific data. Some key benefits:

* **Global State** → If you want to keep something in memory and accessible to everyone (such as a tax configuration or a cache), a Singleton works perfectly.
* **Performance** → Since the service is created only once, you avoid the cost of creating a new instance repeatedly.
* **Memory Efficiency** → Objects that are expensive to create (e.g., database connection pools, API clients, machine learning models, etc.) don’t need to be duplicated for every request.
* **Consistency** → All requests see the same version of the service.

#### Use Case of Singleton Service in ASP.NET Core:

Some common real-world scenarios where Singleton makes sense:

* **Caching** → Store frequently used data (e.g., product categories, configuration, lookup tables) in memory for fast access.
* **Configuration Providers** → A central service that loads configuration settings (e.g., appsettings.json, environment variables) and provides them to other services.
* **Logging** → A logging service can be shared across the entire app (all requests log to the same logger).
* **External API Clients** → If your app needs to call external APIs (e.g., payment gateway, weather API), you don’t want to create a new HTTP client every time — instead, use IHttpClientFactory or a Singleton service.

## Scoped Service in ASP.NET Core
A **Scoped service** is created **once per HTTP request**.

* Within a single request, the **same instance is reused** everywhere it’s injected.
* But as soon as a new HTTP request starts, ASP.NET Core creates a **new instance** for that request.
* Once the request ends, the instance is **disposed** (cleaned up by the DI container).

So, it lives only as long as the HTTP request scope. For a better understanding, please refer to the following diagram.

![image_274.png](image_274.png)

#### Why Scoped Service in ASP.NET Core Web API?
Scoped services are useful when you need to **maintain state per request**, but you don’t want that state to leak across different users or requests.

**Examples:**

* A shopping cart that exists only while a request is being processed.
* A UserContext service that holds info about the currently logged-in user.
* A Unit of Work service where one request = one transaction.

Scoped ensures isolation → each request gets its own copy, but consistency inside that request.

## Transient Service in ASP.NET Core
A **Transient service** is the **shortest-lived service** in ASP.NET Core’s Dependency Injection system.

* **A new instance is created every time** it is requested from the DI container.
* Even within the same request, if you inject it twice, you will get **two completely separate objects**.
* As a result, the service **doesn’t hold any shared state** across or within requests.
* Once used, it’s disposed (garbage collected) quickly, no reuse at all.

This makes transient services **ideal for lightweight, stateless tasks** that don’t rely on user data, shared context, or long-lived memory. For a better understanding, please refer to the following diagram.

![image_275.png](image_275.png)

#### When to Use Transient Service in ASP.NET Core:
Best for services that are stateless and lightweight:

* **Calculation helpers** → e.g., discount calculation, price formatting, currency conversion.
* **Utility classes** → string formatters, mappers, validators.
* **Random generators** / ID generators → if you specifically want a different value each time.
* **Stateless API wrappers** → if they don’t need to cache or persist anything.
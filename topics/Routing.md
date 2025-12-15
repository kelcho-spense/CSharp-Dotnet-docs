# Routing

![image_280.png](image_280.png)

In an ASP.NET Core Web API application, **Routing** is a fundamental concept that connects the **incoming HTTP request** from a client to the correct **controller action method** within the server application. Let us understand routing in ASP.NET Core Web API with an example. Please have a look at the following image.

On the left-hand side, we have multiple clients, such as:

* A **Web Browser** (e.g., Chrome, Edge)
* **Postman** or **Fiddler** (API testing tools)
* **Swagger UI** (auto-generated API explorer)
* A **Mobile App** or a **Desktop Application** consuming your API

## What Is Routing in ASP.NET Core and How Does It Work

In ASP.NET Core Web API, **Routing** is the process that maps incoming HTTP requests to the corresponding **Controller Actions**. Think of it as a **Traffic Director**. 

In earlier versions of ASP.NET Core, developers had to call two middleware components: explicitly

* `UseRouting()` → to match the request to an endpoint
* `UseEndpoints()` → to execute the matched endpoint

However, starting from **.NET 6 and later**, these are automatically configured when we use endpoint mapping methods such as: **app.MapControllers()**;

So, when we call the **app.MapControllers()**, the framework internally:

* Adds the **Routing Middleware** (UseRouting()) to match incoming requests to controller actions.
* Adds the **Endpoint Middleware** (UseEndpoints()) to invoke the matched controller action.

### How Routing Works in ASP.NET Core
To understand how Routing works in ASP.NET Core, please have a look at the following diagram. Here, in the image below, the Routing Engine represents both Routing Middleware and Endpoint Middleware.

![image_281.png](image_281.png)

#### Step 1: Incoming HTTP Request

When a client (such as a browser, Postman, Swagger UI, or mobile app) sends a request to the web server, it includes:

* **HTTP Method**: The type of action requested (GET, POST, PUT, DELETE, PATCH).
* **URL**: The target endpoint (e.g., https://example.com/api/customers/5).
* **Headers**: Metadata like Content-Type, Authorization, or Accept.
* **Query String (optional)**: Additional parameters sent as part of the URL (e.g., ?search=abc).
* **Request Body (optional)**: Data sent with **POST**, **PUT**, or **PATCH** requests (e.g., JSON payload for creating or updating a resource).

Once received, the request flows through the ASP.NET Core **Middleware Pipeline**. One of the essential middleware components in this pipeline is the **Routing Middleware**, which starts the process of matching the request to an endpoint.

#### Step 2: URL Parsing (Handled by Routing Middleware)

When the Routing Middleware is invoked, it first parses the incoming URL into its components. For example, consider the request:

* `https://example.com/api/customers/5?search=abc`
This URL is broken down into:

1. **Scheme**: https (communication protocol)
2. **Host**: `example.com` (domain)
3. **Path**: `/api/customers/5` (endpoint path)
4. **Query String**: Additional parameters if provided (e.g., `?search=abc`)

The **Routing Middleware** uses this path and HTTP method to find a matching endpoint among those registered in the application during startup.

#### Step 3: Finding a Matching Endpoint
After the URL is parsed, the Routing Middleware (UseRouting) begins searching for a matching endpoint among all endpoints registered during application startup.
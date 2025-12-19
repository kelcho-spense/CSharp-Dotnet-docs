# ASP.NET Core Web API – Basics

## Default ASP.NET Core Web API Files and Folders

When you create a new ASP.NET Core Web API project in Visual Studio 2022 0r 2026 with .NET 10.0, 
a predefined set of files and folders is automatically generated. This structure serves as the foundation of the application, ensuring developers have a well-organized starting point.

![image_251.png](image_251.png)

### Connected Services
The Connected Services node in Solution Explorer allows developers to easily integrate external services into their applications without manually writing commonly used code for connectivity, authentication, or configuration

It provides integration for services such as:

* **Azure Services** – Azure Storage, Azure App Services, Azure Key Vault, etc.
* **Third-party REST APIs** – For example, Google APIs, Stripe for payment processing, or Twilio for SMS.
* **Microsoft Office 365 APIs** – Accessing Office 365 resources like emails, calendars, or SharePoint.

Benefits of Connected Services:

* **Automatic Configuration** – Adds the necessary configuration files and entries to appsettings.json.
* **Client Code Generation** – Creates client libraries that allow developers to call external APIs as strongly typed methods instead of crafting raw HTTP requests.
* **Dependency Management** – Installs required NuGet packages automatically.

> Example: If you want to use Azure Key Vault for securely storing API keys or database connection strings, you can add it via Connected Services, and Visual Studio will handle package installation, configuration, and secret retrieval setup.

### Dependencies:
The Dependencies folder shows all **Packages, Analyzers, SDKs,** and **Frameworks** your project depends on. It ensures that everything needed to build and run your Web API is properly referenced. This makes it easy to see exactly what external components are powering your application. When expanded, it typically contains three key categories: **Analyzers**, **Frameworks**, and **Packages**, as shown in the below image.

![image_252.png](image_252.png)

### Analyzers

Analyzers are **Static Code Analysis Tools** that run inside Visual Studio and during build time, it is used to improve code quality. They help with:

* **Code Quality Checks**: Detecting potential bugs, unused variables, or unsafe patterns.
* **Style Enforcement**: Ensuring coding conventions (e.g., naming conventions, spacing rules, formatting, etc.) are followed.
* **Performance Suggestions**: Recommend better coding practices for speed and memory efficiency.

### Frameworks
The Frameworks section lists the core libraries and runtime components that your project depends on. By default, two major framework groups are included. They are as follows:

#### Microsoft.NETCore.App:
This Includes the .NET runtime, Base Class Libraries (BCL), Garbage Collector, JIT compiler, and other core functionality required to run .NET applications.

* **Base Class Libraries (BCL)**:
  * System.IO → File and stream handling.
  * System.Collections → Collections like lists and dictionaries.
  * System.Threading → Multithreading and parallel programming.
* **Core Runtime Libraries**: Provides the **Garbage Collector** (GC), **JIT Compiler**, and runtime services necessary for executing .NET Core applications.

#### Microsoft.AspNetCore.App:
Includes all the **ASP.NET Core libraries** required for web development. It provides:

* **Web Server Implementations** → Kestrel for cross-platform hosting and IIS integration for Windows hosting.
* **ASP.NET Core MVC Framework** → For building APIs and web apps using the MVC pattern.
* **Razor Engine** → Used for processing Razor pages and views in dynamic web content.
* **Routing and Model Binding** → Enables mapping of HTTP requests to controllers and automatic data binding.
* **Authentication and Authorization Libraries** → Provides support for securing APIs with JWT, OAuth, Identity, etc.

#### Packages
The **Packages Section** lists external **NuGet packages** that are added to extend the project’s functionality. They are downloaded from NuGet.org and are automatically restored whenever you build the project on a new machine. Packages can be removed if no longer required, keeping the project lightweight.

Common examples include:

* **Swashbuckle.AspNetCore** – Integrates Swagger UI for interactive API documentation and testing.
* **Entity Framework Core (EF Core)** – Provides ORM (Object Relational Mapping) support for database access.
* **Serilog / NLog** – Advanced logging frameworks for structured and centralized logging.
* **IdentityServer** – A library for implementing authentication, authorization, and token management.

#### Properties:
The **Properties folder** in an ASP.NET Core project contains files that define how the application behaves during **development and debugging** inside Visual Studio or when running via the .NET CLI. By default, it includes only one file: launchSettings.json.

This **`file is not used in production`**. Instead, it is only relevant during local development, helping Visual Studio and the dotnet run command determine:

* Which URLs the application should listen on.
* Whether it runs on HTTP, HTTPS, or IIS Express.
* Which environment variables should be set (for example, ASPNETCORE_ENVIRONMENT=Development).
* Whether a browser should open automatically and which URL it should open.

#### launchSettings.json
The launchSettings.json file contains launch profiles that specify how the application should be started. Each profile is like a separate startup configuration, allowing you to run the same project in different modes (HTTP, HTTPS, or IIS Express).

* It’s a configuration file used locally (on a developer’s machine) by tools like Visual Studio, dotnet CLI (`dotnet run`) or other IDEs to decide how to launch the app. 
* It is **not** used by the application itself once published or deployed. Production hosting ignores it; production configuration should come from other sources (e.g. `appsettings.json`, real environment variables). 


```JSON
{
  "profiles": {
    "http": {
      "commandName": "Project",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "dotnetRunMessages": true,
      "applicationUrl": "http://localhost:5021"
    },
    "https": {
      "commandName": "Project",
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "dotnetRunMessages": true,
      "applicationUrl": "https://localhost:7081;http://localhost:5021"
    },
    "Container (Dockerfile)": {
      "commandName": "Docker",
      "launchUrl": "{Scheme}://{ServiceHost}:{ServicePort}",
      "environmentVariables": {
        "ASPNETCORE_HTTPS_PORTS": "8081",
        "ASPNETCORE_HTTP_PORTS": "8080"
      },
      "publishAllPorts": true,
      "useSSL": true
    }
  },
  "$schema": "https://json.schemastore.org/launchsettings.json"
}
```


---

## Structure of Your JSON & What It Means

Your JSON defines three “profiles” under `"profiles"` — each profile describes a different way to launch the application. These are useful when you want different behaviors or settings depending on context (e.g. plain HTTP, HTTPS, Docker container).

Here are the profiles:

| **Profile Name**           | **Purpose / Meaning**                                                                                                                                                                                                                                                                                                                                                                              |
| -------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `"http"`                   | Runs the app over plain HTTP (on localhost), using the project directly (likely via Kestrel server). `environmentVariables` sets `ASPNETCORE_ENVIRONMENT = "Development"`, meaning the app runs in “development” mode locally. `applicationUrl` says it will listen on `http://localhost:5021`.                                                                                                    |
| `"https"`                  | Runs the app over HTTPS (and also HTTP fallback) in development mode. The `applicationUrl` is `https://localhost:7081;http://localhost:5021`, meaning the app will listen on both those addresses. This is for testing secure HTTPS connections locally.                                                                                                                                           |
| `"Container (Dockerfile)"` | A profile used when running the app inside a container (e.g. Docker). `commandName: "Docker"` signals tooling to spin up a container rather than directly run the project. The profile defines environment variables (e.g. ports for HTTP and HTTPS inside container), and `publishAllPorts: true` indicates that ports should be exposed/published. `useSSL: true` suggests HTTPS should be used. |

Other settings:

* `"commandName"` — defines how to start the app. For `"Project"` → run the compiled project directly (e.g. with Kestrel). For `"Docker"` → run via Docker container. 
* `"environmentVariables"` — these are variables set for the process when launched via that profile. Commonly used to set the runtime environment (e.g. `"ASPNETCORE_ENVIRONMENT": "Development"`). 
* `"applicationUrl"` — the URL(s) (and ports) the application will bind to when run under this profile. Multiple URLs (semicolon-separated) mean the app listens on multiple addresses (e.g. both HTTP and HTTPS). 
* `$schema` — a reference to the JSON schema for launchSettings.json (for IDE validations). Doesn’t affect runtime logic.

---

####  What It’s Good For

* Easily switch between different launch modes (http, https, container) depending on what you’re testing.
* Automatically set environment variables (like `"ASPNETCORE_ENVIRONMENT"`) so your app can behave differently (for example loading development settings, showing detailed errors, enabling debugging). 
* Define which ports/URLs the app uses during development without hard-coding them in code — makes it easier for each developer on the team to run locally without conflicts.
* If using Docker/containers, define container-specific run settings (ports, SSL, publishing) for easy container runs.

---

### Controllers Folder:

The Controllers folder is the core of any ASP.NET Core Web API application. It contains the controller classes, which are responsible for:

* Accepting **incoming HTTP requests** from clients (e.g., browser, Postman, mobile app).
* Executing the necessary **business logic** (either directly or via service/repository layers).
* Returning the appropriate **HTTP response** back to the client (e.g., JSON data, status codes).

Controllers act as the **bridge between clients (such as browsers, mobile apps, or API consumers)** and our application’s business logic.

* Each **controller class** typically represents a **resource** or a **set of related endpoints**. For example, you might have a ProductsController to handle product-related APIs or a UsersController for user management.
* Controllers use **routing attributes** (like `[Route]`, `[HttpGet]`, `[HttpPost]`) to map specific HTTP requests to action methods.
* Each **action method** inside a controller corresponds to an operation that can be performed on the resource, such as:

  * **GET** → Retrieve data
  * **POST** → Insert new data
  * **PUT** → Update existing data
  * **DELETE** → Remove data

#### Example: WeatherForecastController

By default, when we create a new ASP.NET Core Web API project, Visual Studio adds a **sample controller** named WeatherForecastController. This controller demonstrates the basic structure and functionality of a Web API endpoint.

```C#
using Microsoft.AspNetCore.Mvc;
namespace MyFirstWebAPIProject.Controllers
{
    [ApiController]
    [Route("[controller]")]
    public class WeatherForecastController : ControllerBase
    {
        private static readonly string[] Summaries = new[]
        {
            "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching"
        };

        private readonly ILogger<WeatherForecastController> _logger;

        public WeatherForecastController(ILogger<WeatherForecastController> logger)
        {
            _logger = logger;
        }

        [HttpGet(Name = "GetWeatherForecast")]
        public IEnumerable<WeatherForecast> Get()
        {
            return Enumerable.Range(1, 5).Select(index => new WeatherForecast
            {
                Date = DateOnly.FromDateTime(DateTime.Now.AddDays(index)),
                TemperatureC = Random.Shared.Next(-20, 55),
                Summary = Summaries[Random.Shared.Next(Summaries.Length)]
            })
            .ToArray();
        }
    }
}
```

### appsettings.json file:

The **appsettings.json** file acts as the **primary configuration source** for an ASP.NET Core application. It contains structured key-value pairs in **JSON format** and defines how the application behaves at runtime. 

* **Database connection strings** (SQL Server, PostgreSQL, etc.).
* **API keys** for third-party services.
* **Logging configuration** (log levels for different parts of the app).
* **Custom configuration values** are defined by the developer.

ASP.NET Core also supports **hierarchical configuration** and **environment-specific overrides**. For example:

* `appsettings.json` → Base/default settings.
* `appsettings.Development.json` → Overrides for development environment.
* `appsettings.Production.json` → Overrides for production deployment.

ASP.NET Core automatically loads the correct configuration based on the **`ASPNETCORE_ENVIRONMENT`** variable.
This layered approach enables developers to have **distinct settings for each environment** while maintaining a consistent codebase.

### Program.cs Class File:

The **Program.cs** file is the **entry point** of every ASP.NET Core application. When the application starts, this is the file that the .NET runtime looks for and executes. It defines the **host configuration, service registration**, and the **HTTP request pipeline (also known as the middleware pipeline)**. Together, these determine how your API behaves when handling client requests and responses.

In .NET 6 and later, Microsoft introduced a **minimal hosting model**, which simplified the structure of Program.cs by merging Startup.cs into it. This is why you see all configurations (services + middleware) handled in one place.

#### Key Responsibilities of the Program.cs
The Program.cs file performs four major tasks:

* **Build and configure the ASP.NET Core host** → Sets up the hosting environment (Kestrel web server, configuration, logging, etc.).
* **Register services** → Adds services such as Controllers, Swagger, Authentication, EF Core, and custom services to the dependency injection (DI) container.
* **Define the middleware pipeline** → Configures request/response processing (e.g., HTTPS redirection, authentication, routing, error handling) and also the order in which the middleware components handle requests.
* **Start the application** → Runs the app (Calls app.Run()) so it can begin listening for HTTP requests.

**Default `Program.cs` File**

```C#
var builder = WebApplication.CreateBuilder(args);

// Add services to the container.

builder.Services.AddControllers();
// Learn more about configuring OpenAPI at https://aka.ms/aspnet/openapi
builder.Services.AddOpenApi();

var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.MapOpenApi();
}

app.UseHttpsRedirection();

app.UseAuthorization();

app.MapControllers();

app.Run();

```

#### Service Registration (Dependency Injection)
Service registration is the process of adding reusable components (services) to the **Dependency Injection** (DI) container. ASP.NET Core uses built-in DI by default, which makes it easier to manage dependencies. Services are registered inside the builder.Services.

- `AddControllers()`

  - Registers **MVC controllers** with the DI container.
  - Enables support for Web API endpoints defined in controller classes.
- `AddOpenApi()`
  - Registers services needed to generate openAPI docs.
  - Maps an endpoint for viewing the OpenAPI document in JSON format with the `MapOpenApi()` extension method on the app.

### Middleware Configuration

The app object defines the **Middleware Pipeline**, a sequence of components that process every incoming HTTP request and outgoing response. Each piece of middleware can inspect, modify, or short-circuit the request/response before passing it to the next component. The order of middleware calls is very important.

* `if (app.Environment.IsDevelopment())`
  * Checks if the current environment is Development.
* `app.UseAuthorization()`
  * Adds authorization middleware to enforce security policies.
  * Works in conjunction with authentication middleware to restrict access to endpoints.
* `app.MapControllers()`
  * Maps controller routes to the request pipeline.
  * Without this line, controller endpoints will not be accessible.
* `app.Run()`
  * Starts the application and begins listening for incoming HTTP requests.

### `MyProjectName.http` file

The **.http file** in an ASP.NET Core Web API project is a **built-in testing file** that allows you to send HTTP requests directly from within **Visual Studio** or **Visual Studio Code**. Instead of relying on external tools like **Postman or Fiddler**, or command-line tools such as **cURL**, you can quickly write, send, and debug API requests directly inside your IDE.

This file is part of the Visual Studio HTTP Client feature and is automatically generated when you create a new Web API project.

When you open the MyProjectName.http file for the first time, you will typically see

```Bash
@MyFirstWebAPIProject_HostAddress = http://localhost:5021

GET {{@MyFirstWebAPIProject_HostAddress}}/weatherforecast/
Accept: application/json

###
```

* **@Blogging_Web_API_HostAddress** → A variable holding the base URL of your API (localhost + port).
* **GET {{MyFirstWebAPIProject_HostAddress}}/weatherforecast/** → Defines a sample GET request to the WeatherForecast endpoint.
* **Accept: application/json** → Adds a request header that tells the server to return the response in JSON format.

**How to Use**
To test an API request using the .http file:

* **Run the project** → Start your API using the HTTP launch profile in Visual Studio.
* **Ensure the port matches** → The port number in MyFirstWebAPIProject.http must match the one in launchSettings.json. If your app runs on http://localhost:5021, make sure the variable points to the same URL.
* **Send request inside IDE** → Open the .http file and hover over the request. Visual Studio will show a “Send Request” link above it.
* **View the response** → After clicking “Send Request,” the response (status code, headers, body) will appear in a new results pane or side window.

![image_253.png](image_253.png)

Once you click on the Send Request link, the responses are typically shown in a pane next to the request or in a separate window, allowing you to view the status code, response body, and headers, as shown in the image below.

![image_254.png](image_254.png)

The **.http** file is a developer-friendly tool built into Visual Studio that allows you to **send and test API requests directly inside your IDE**. It simplifies debugging, supports multiple HTTP methods, and provides quick access to responses without relying on external tools.
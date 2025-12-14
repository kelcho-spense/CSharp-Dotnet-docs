# Swagger API in ASP.NET Core Web API

![image_278.png](image_278.png)

Swagger, often implemented using the **Swashbuckle** package, is a powerful tool used for documenting RESTful APIs developed with ASP.NET Core Web API. It provides a way to help plan, design, and document RESTful APIs. Swagger in ASP.NET Core Web API helps developers create, document, and consume RESTful APIs.

### Key Features of Swagger in ASP.NET Core Web API:

* **API Documentation**: Swagger automatically generates interactive API documentation, known as Swagger UI, based on your ASP.NET Core Web API Project. This documentation includes details about endpoints, parameters, request and response schemas, and even allows users to try out API calls directly from the documentation interface. The documentation can be dynamically updated as changes occur in the API.
* **Standardization**: Swagger uses the OpenAPI Specification (OAS), which is a widely adopted industry standard for documenting RESTful APIs.
* **Testing** and **Debugging**: Through Swagger UI, developers can send requests to the API, view the responses, and effectively test API functionality in a user-friendly web interface without the need for a separate tool or writing additional code. This real-time interaction helps in debugging and improving the API during the development phase.
* **API Versioning**: Swagger supports easy API versioning, which is important for evolving an API without breaking the existing client integrations. It provides a clear and structured way to communicate changes and deprecations to API consumers.

### How to Install the Swagger Package in ASP.NET Core Web API:

> Please not this tutorial is for Swashbuckle.AspNetCore v10 and above. Below this there are breaking changes.

Integrating Swagger into the ASP.NET Core Web API project is straightforward using the `Swashbuckle.AspNetCore` NuGet package. Once we install the package and register the required Swagger services and middleware components, Swagger automatically generates the API documentation, reducing the effort required to maintain separate documentation files.

Swashbuckle.AspNetCore consists of multiple components that can be used together or individually depending on your needs.

At its core, 
* [Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger) : there's an OpenAPI generator
* [Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen) : middleware to expose OpenAPI (Swagger) documentation as JSON endpoints.
* [Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI) : a packaged version of the swagger-ui. 

These three packages can be installed with the `Swashbuckle.AspNetCore` "metapackage" 

> Version 10.0 of Swashbuckle.AspNetCore introduces breaking changes due to upgrading our dependency on [Microsoft.OpenApi](https://www.nuget.org/packages/Microsoft.OpenApi) to version 2.x.x to add support for generating OpenAPI 3.1 documents.

#### Installation

install below packages

```bash
Microsoft.AspNetCore.OpenApi

Swashbuckle.AspNetCore
```

![image_276.png](image_276.png)

#### Configure Swagger Services:

Register AddSwaggerGen() service

```c#
builder.Services.AddSwaggerGen(options =>
{
    // Define API metadata (title, version, description, contact) shown in Swagger UI
    options.SwaggerDoc("v1", new() 
    { 
        Title = "Shopping Cart API", 
        Version = "v1", 
        Description = "API for managing a shopping cart with discounts, tax, and delivery fee calculations.",
        Contact = new()
        {
            Name = "API Support",
            Email = "support@shoppingcart.com",
            Url = new Uri("https://www.shoppingcart.com/support")
        }
    });
});
```

Enable Swagger only in Development environment for security, prevents exposing API structure in production.

```C#
if (app.Environment.IsDevelopment())
{
    // Enable middleware to serve generated Swagger as JSON endpoint
    app.UseSwagger();
    // Enable middleware to serve Swagger UI (HTML, JS, CSS, etc.)
    // Provides interactive documentation at /swagger
    app.UseSwaggerUI(options =>
    {
        // Configure the JSON endpoint for Swagger UI to consume
        options.SwaggerEndpoint("/swagger/v1/swagger.json", "Shopping Cart API V1");
    });
}
```

Incase you have issues  where your using productName and ProductName interchangeably, price and Price  please add **.AddJsonOptions()** with `PropertyNameCaseInsensitive = true` as shown below 

```C#
using ShoppingCartAPI.Services;
using ShoppingCartAPI.Services.Interfaces;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.
builder.Services.AddControllers()
    .AddJsonOptions(options =>
    {
        options.JsonSerializerOptions.PropertyNameCaseInsensitive = true;
    });

```
#### Run the API:
Now, we need to Build and Run the project. Then, access the Swagger UI by navigating to the URL: `http://localhost:<port>/swagger`, as shown in the below image.

![image_279.png](image_279.png)

#### Customizing Swagger in ASP.NET Core Web API

Customizing Swagger in an ASP.NET Core Web API project can be essential in various scenarios to enhance documentation, usability, and security and meet the specific requirements of your API consumers. This can also include custom information, such as API descriptions and contact information, licenses, versions, etc.
So, modify the AddSwaggerGen service as follows. As you can see here, we have provided more information about our APIs, such as the **Title**, **Version**, **Description**, **Terms of Service**, **Contact**, and **License**.

```C#
builder.Services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("V10", new OpenApiInfo
    {
        Title = "My Custom API",
        Version = "V1",
        Description = "A Brief Description of My APIs",
        TermsOfService = new Uri("https://shoppingcart.net/privacy-policy/"),
        Contact = new OpenApiContact
        {
            Name = "Support",
            Email = "support@shoppingcart.net",
            Url = new Uri("https://shoppingcart.net/contact/")
        },
        License = new OpenApiLicense
        {
            Name = "Use Under XYZ",
            Url = new Uri("https:shoppingcart.net/about-us/")
        }
    });
});
```

By default, Swagger API looks for version V1, but we have changed the version to version V10 here. So, we also need to configure the same into the Swagger Middleware component. So, next, modify the UseSwaggerUI as follows:

```C#
app.UseSwaggerUI(c =>
{
    c.SwaggerEndpoint("/swagger/V10/swagger.json", "My API V10");
});
```
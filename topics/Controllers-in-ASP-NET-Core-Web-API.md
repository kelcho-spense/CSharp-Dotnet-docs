# Controllers in ASP.NET Core Web API

In **ASP.NET Core Web API**, controllers are **special classes** that handle HTTP requests sent by clients (such as browsers, mobile apps, Postman, or other services) and return HTTP responses. Think of them as the “**Traffic Police**” or **Gatekeepers** of your API. They listen to requests at specific endpoints (URLs), process them with the aid of business logic, and return meaningful responses in standard web formats, such as JSON or XML.

In MVC terms:

* **Model** = Data (Entities, DTOs, Domain Models).
* **View** = Not used in Web APIs (unlike MVC web apps).
* **Controller** = The central unit that receives the request, coordinates with services and models, and sends back a response.

## Key Roles of a Controller
Let us understand the key Roles and Responsibilities of a Controller in an ASP.NET Core Web API Application.

### Request Handling
The controller listens at a specific URL route (e.g., /api/products) and decides which **Action Method** (a public method inside the controller) should handle the request.
Each action method is decorated with attributes such as `[HttpGet]`, `[HttpPost]`, `[HttpPut]`, `[HttpDelete]` that map to HTTP verbs.

**Example**: Here, `GET /api/products/5` will be handled by the GetProduct method.

```C#
[ApiController]
[Route("api/[controller]")]
public class ProductsController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult GetProduct(int id) 
    {
        // logic to fetch product
        return Ok(new { Id = id, Name = "Laptop" });
    }
}
```
> Analogy: Think of a call center where incoming calls (requests) are routed to the correct department (action method).

### Data Processing

Controllers **shouldn’t hold heavy business logic themselves**. Instead, they delegate:

* To **services** for applying business rules (e.g., price discounts, tax calculation).
* To **repositories** or EF Core for data persistence (CRUD on a database).
* To **validators** for input checking (e.g., ensuring an email format is valid).

They handle input validation, model binding, and error checking before passing data to the services. For example:

```C#
[HttpPost]
public IActionResult CreateProduct(ProductDto dto)
{
    if (!ModelState.IsValid)
        return BadRequest(ModelState);

    var product = _productService.Create(dto);
    return CreatedAtAction(nameof(GetProduct), new { id = product.Id }, product);
}
```

> Analogy: The controller is like a coordinator. It doesn’t do the heavy work but ensures the right team (services, database) gets involved.

### Response Generation
After the data is processed, the controller decides **what response to send back**. Responses may include:

* **Response Payload** → Data in JSON/XML format (default is JSON in Web API).
* **Status codes** → To indicate success or failure (200 OK, 201 Created, 400 Bad Request, 404 Not Found, 500 Internal Server Error, etc.).
* **Error messages** → Clear explanation if something went wrong.

Example:

```C#
[HttpDelete("{id}")]
public IActionResult DeleteProduct(int id)
{
    bool deleted = _productService.Delete(id);
    if (!deleted)
        return NotFound(new { Message = "Product not found" });

    return NoContent(); // 204
}
```

> Analogy: Like a waiter in a restaurant, after checking with the chef (services/database), the waiter (controller) delivers the order (response) back to the customer (client).

### Adding Controller Class in ASP.NET Core Web API

When creating a controller in an ASP.NET Core Web API project, adhere to the following conventions and best practices:

#### Naming Convention:
ASP.NET Core uses **Conventions** for recognizing controllers. By convention, ASP.NET Core recognizes controller classes whose names end with the **“Controller”** suffix.

* **Must end with Controller suffix** → This is how ASP.NET Core identifies them during routing.
* Examples:
  * EmployeeController → Manages employees.
  * StudentController → Manages students.
  * ProductController → Manages products.
  
If you name your class Employee instead of EmployeeController, ASP.NET Core won’t recognize it as a controller.

> Best Practice: Choose meaningful names based on the resource you’re exposing, not on verbs.
> * Correct Example: `EmployeeController`
> * Wrong Example: `ManageEmployeeController` or `DoEmployeeStuffController`

### Base Class

Controllers in Web API projects usually inherit from ControllerBase.

* **ControllerBase** → Provides features needed for **APIs** (No views, just JSON/XML responses).
* **Controller** → Used in **MVC applications** where you need both API and Razor Views (it inherits from ControllerBase and adds view-related features like View(), PartialView(), etc.).

In Web API, stick with ControllerBase unless you are mixing APIs with Razor views.

```C#
[ApiController]
[Route("api/[controller]")]
public class EmployeeController : ControllerBase
{
    [HttpGet]
    public IActionResult GetEmployees()
    {
        return Ok(new[] { "John", "Jane", "Alice" });
    }
}
```

#### Steps to Add a Controller:

Let’s add a controller named Employee. To do so, right-click on the Controllers folder and then select Add => Controller from the context menu, which will open the following Add New Scaffold Item window. From this window, please choose API, then API Controller – Empty, and click on the Add button, as shown in the image below.

![image_255.png](image_255.png)

##### Understanding the Different Options:

###### API Controller – Empty

This option provides a blank controller with no predefined actions, allowing you to build everything from scratch. It’s ideal for learning, experimenting, and situations where you need maximum flexibility to design custom endpoints and logic without code.

* Contains only class declaration with [ApiController] and [Route].
* No predefined CRUD methods are available; you must add all endpoints manually.
* Best suited for beginners and projects that require full customization.
* Helps in understanding the fundamentals of Routing, HTTP Verbs, and Responses.

> Analogy: Like moving into an empty room where you have complete freedom to decide the layout, furniture, and design.

###### API Controller with Read/Write Actions

This option scaffolds a controller with predefined CRUD (Create, Read, Update, Delete) methods, making it easy to set up a RESTful API quickly. It’s ideal for projects where you want a head start and don’t want to create standard CRUD endpoints manually.

* Automatically generates methods for Get, Get by ID, Post, Put, and Delete.
* Suitable for quick prototypes or standard APIs that do not require complex logic.
* Reduces repetitive coding and provides a working structure instantly.
* Can be extended later by adding business logic or custom validations.

> Analogy: Like moving into a house with basic furniture already in place, you can start living immediately, but you can still customize it later.

###### API Controller with Actions Using Entity Framework

This option extends CRUD scaffolding by connecting directly to a database using Entity Framework Core. It’s perfect when you already have models and a DbContext and want to generate database-ready API endpoints quickly.

* Prebuilt methods that query and update data directly from the database.
* Integrates Entity Framework Core for database interactions.
* Useful for rapid prototyping of database-backed applications.
* Reduces duplicate code for common database operations.

##### API with Read/Write Endpoints

This option is similar to the Read/Write Actions template but provides more control over endpoint naming and structure. It’s meant for developers who need flexibility in designing endpoints while still benefiting from predefined CRUD scaffolding.

Generates standard CRUD endpoints but allows custom route names.
Useful when API design requires adherence to specific naming conventions.
Balances automation with customization for endpoint structure.
Suitable for teams following unique API standards.

##### API Controller with Read/Write Endpoints Using Entity Framework

This option combines the benefits of Entity Framework integration with customizable endpoints, providing a flexible solution. It gives you ready-to-use database CRUD operations while allowing you to rename or extend endpoints beyond the defaults.

* Direct database integration using Entity Framework Core.
* Allows renaming and customizing routes for more flexibility.
* Ideal for production-ready apps requiring both EF and non-standard endpoint names.
* Saves time by scaffolding EF code while supporting customization.
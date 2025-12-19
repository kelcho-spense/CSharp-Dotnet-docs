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

#### Incoming HTTP Request

When a client (such as a browser, Postman, Swagger UI, or mobile app) sends a request to the web server, it includes:

* **HTTP Method**: The type of action requested (GET, POST, PUT, DELETE, PATCH).
* **URL**: The target endpoint (e.g., https://example.com/api/customers/5).
* **Headers**: Metadata like Content-Type, Authorization, or Accept.
* **Query String (optional)**: Additional parameters sent as part of the URL (e.g., ?search=abc).
* **Request Body (optional)**: Data sent with **POST**, **PUT**, or **PATCH** requests (e.g., JSON payload for creating or updating a resource).

Once received, the request flows through the ASP.NET Core **Middleware Pipeline**. One of the essential middleware components in this pipeline is the **Routing Middleware**, which starts the process of matching the request to an endpoint.

#### URL Parsing (Handled by Routing Middleware)

When the Routing Middleware is invoked, it first parses the incoming URL into its components. For example, consider the request:

* `https://example.com/api/customers/5?search=abc`
This URL is broken down into:

1. **Scheme**: https (communication protocol)
2. **Host**: `example.com` (domain)
3. **Path**: `/api/customers/5` (endpoint path)
4. **Query String**: Additional parameters if provided (e.g., `?search=abc`)

The **Routing Middleware** uses this path and HTTP method to find a matching endpoint among those registered in the application during startup.

#### Finding a Matching Endpoint
After the URL is parsed, the Routing Middleware (UseRouting) begins searching for a matching endpoint among all endpoints registered during application startup.

#### Endpoint Found or Not?
At this stage, the Routing Middleware has either identified a matching endpoint or found none. The outcome determines the next step in the request pipeline.

##### Scenario 1: No Matching Endpoint (HTTP 404 Error)

* If no endpoint matches the request, the HttpContext remains without an assigned endpoint.
* The request continues through the remaining middleware, but since **Endpoint Middleware** has nothing to execute, the framework finally returns an HTTP 404 Not Found response to the client.
* This indicates that the requested URL or resource does not exist in the application.

##### Scenario 2: Matching Endpoint Found (Process the Request)
- When a valid endpoint is found, the **Routing Middleware** attaches the endpoint and extracted **Route Values** to the HttpContext.

#### Policy Middlewares Use the Selected Endpoint (Optional)
Once the Routing Middleware (UseRouting) has successfully matched the incoming request to a specific endpoint, ASP.NET Core allows several Policy Middlewares to act before the endpoint is executed.

These middlewares enforce cross-cutting concerns such as **Authorization**, **CORS**, and R**ate Limiting**, using the metadata already attached to the selected endpoint. These are often referred to as Policy Middlewares, because they apply policies defined in the endpoint’s metadata.

**Common Policy Middlewares Include:**

`UseCors()`

* Applies Cross-Origin Resource Sharing rules.
* Checks whether the current request’s origin, headers, and method are allowed by the configured CORS policy.
* Can be configured globally or per-endpoint using `[EnableCors]` or `[DisableCors]` attributes.

`UseAuthentication()`

* Authenticates the user based on **tokens, cookies, or credentials**.
* Populates the `HttpContext.User` property with the identity and claims if authentication succeeds.
* **Runs before authorization**, because authorization depends on the authenticated identity.

`UseAuthorization()`

* Uses endpoint metadata (like `[Authorize]` attributes or **authorization policies**) to decide whether **the current request is allowed to proceed**.
* If the user **is not authorized, it short-circuits the pipeline with a 403 Forbidden or 401 Unauthorized** response.
* If authorized, **the request continues to the next middleware**.

`UseRateLimiter()`

* Enforces rate-limiting policies attached to endpoints.
* Controls **how many requests a client can make within a defined time window**.

#### Route Parameters and Query Strings

ASP.NET Core provides two main ways to receive such data:

1. **Route Data**
2. **Query Strings**

Both mechanisms help your API capture values directly from the request URL, but they serve different purposes.

* **Route Data** is used when the value is **an essential part of the resource path**, for example, `/api/employees/5` directly identifies the employee with ID 5.
* **Query Strings**, on the other hand, are used when the values are **optional filters or modifiers**, for example, `/api/employees?department=HR&sortBy=name` retrieves employees in the HR department sorted by name.

In short:

* Use **Route Data** for mandatory and identity-based parameters (like **IDs**).
* Use **Query Strings** for **optional** or **filtering parameters** (like **search**, **filter**, or **sort options**).

#### Fetching Employee by ID using Route Data

Suppose we want to fetch one employee’s details by their ID. The **ID value** can naturally be part of the URL, since it uniquely identifies a resource. So, we need to define one parameter to take the ID value within the method signature, as shown in the image below

![image_288.png](image_288.png)

In ASP.NET Core Web API, if you want to pass anything as part of the URL Path (Route data), you need to use curly braces **{}**. Inside the curly braces, provide the same name as the method parameter.

In our example, the GetEmployeeById method takes the id parameter, so we need to pass the id within the curly braces of the Route or HttpGet attribute, as shown in the image below. Here, we are using HttpGet, but you can also use the Route Attribute.

![image_289.png](image_289.png)

So, please modify the EmployeeController class as shown below.

```C#
using Microsoft.AspNetCore.Mvc;
using RoutingInASPNETCoreWebAPI.Data;
using RoutingInASPNETCoreWebAPI.Models;

namespace RoutingInASPNETCoreWebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class EmployeeController : ControllerBase
    {
        // GET api/Employee/1

        [HttpGet("{id}")]
        public ActionResult<Employee> GetEmployeeById(int id)
        {
            var employee = EmployeeData.Employees.FirstOrDefault(e => e.Id == id);
            if (employee == null)
                return NotFound($"Employee with ID {id} not found.");

            return Ok(employee);
        }
    }
}
```

* **Route Attribute**: [Route(“api/[controller]”)] → Base route (api/Employee) for all endpoints under EmployeeController.
* **Route Parameter**: [HttpGet(“{id}”)] → The {id} in the route represents a dynamic parameter.
* **Model Binding**: ASP.NET Core automatically binds the {id} value from the URL to the id parameter in the method.
* **Action Method**: GetEmployeeById retrieves the employee with the specified ID from the in-memory data.

#### Handling Multiple Route Parameters in ASP.NET Core Web API:

In some cases, you may need to pass **multiple route parameters**, for instance, fetching employees by both Gender and City. Here, we want the Gender to be Male or Female and the city name to be specified. So, we will create an action method that takes two parameters, both of which are string types, as shown in the image below.

![image_290.png](image_290.png)

Now we want to access the above GetEmployeesByGenderAndCity action method using the URL: `api/Employee/Gender/Male/City/Los Angeles`

Here, Male and Los Angeles are the dynamic values. So, we need to decorate the GetEmployeesByGenderAndCity method with the Route or HttpGet Attribute, as shown in the image below. Here, we are passing the gender and city parameters in curly braces.

![image_291.png](image_291.png)

Please modify the EmployeeController class as shown below.

```C#
using Microsoft.AspNetCore.Mvc;
using RoutingInASPNETCoreWebAPI.Data;
using RoutingInASPNETCoreWebAPI.Models;

namespace RoutingInASPNETCoreWebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class EmployeeController : ControllerBase
    {
        // GET api/Employee/Gender/Male/City/Los Angeles

        [HttpGet("Gender/{gender}/City/{city}")]
        public ActionResult<IEnumerable<Employee>> GetEmployeesByGenderAndCity(string gender, string city)
        {
            var filteredEmployees = EmployeeData.Employees
                .Where(e => e.Gender.Equals(gender, StringComparison.OrdinalIgnoreCase) &&
                            e.City.Equals(city, StringComparison.OrdinalIgnoreCase))
                .ToList();

            if (!filteredEmployees.Any())
                return NotFound($"No employees found with Gender '{gender}' in City '{city}'.");

            return Ok(filteredEmployees);
        }
    }
}
```

* **Route Template**: “Gender/{gender}/City/{city}” defines two dynamic route parameters: gender and city.
* **Action Method**: Filters employees based on the provided gender and city.
* **Case-insensitive Comparison**: Ensures the search is user-friendly regardless of input casing.

#### What Is a Query String?

Query Strings are key-value pairs appended to the URL after a question mark (`?`). Multiple query parameters are separated by `&`. They typically provide optional criteria or additional information and are best suited for:

* Filtering
* Searching
* Sorting
* Paging


Query Strings don’t identify the resource itself but rather **modify the result** returned for that resource. You can specify as many optional query parameters as you want, in any order.

Let us understand the query string with an example. Now, we want to search employees by **department** without making it part of the route; we can use a **query string parameter**.

Here, we have created a method named SearchEmployee with one parameter called Department. Further notice, we have not included that parameter in the Route Attribute.

```C#
using Microsoft.AspNetCore.Mvc;
using RoutingInASPNETCoreWebAPI.Data;
using RoutingInASPNETCoreWebAPI.Models;

namespace RoutingInASPNETCoreWebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class EmployeeController : ControllerBase
    {
        // GET api/Employee/Search?Department=HR
        [HttpGet("Search")]
        public ActionResult<IEnumerable<Employee>> SearchEmployees([FromQuery] string department)
        {
            var filteredEmployees = EmployeeData.Employees
                .Where(e => e.Department.Equals(department, StringComparison.OrdinalIgnoreCase))
                .ToList();

            if (!filteredEmployees.Any())
                return NotFound($"No employees found in Department '{department}'.");

            return Ok(filteredEmployees);
        }
    }
}
```

* **Route Attribute: [HttpGet(“Search”)]** sets the endpoint to **api/Employee/Search**.
* **Query Parameter**: [FromQuery] tells ASP.NET Core to bind the department from the query string. Using the [FromQuery] attribute here is optional, as by default, value type parameters are bound from the Query string.
* **Action Method**: Filters employees based on the provided department. If no employee matches, it returns a 404 with a descriptive message.

#### Multiple Query String Parameters in ASP.NET Core Web API

In real-world scenarios, search functionalities often require multiple optional parameters to filter data. Let’s say we want to filter employees by **City**, **Gender**, and **Department**. In that case, our action method should accept three parameters. So, modify the Employee Controller as follows. If you want to make a query string parameter optional, you need to use ? or initialize the parameter with a default value.

```C#
using Microsoft.AspNetCore.Mvc;
using RoutingInASPNETCoreWebAPI.Data;
using RoutingInASPNETCoreWebAPI.Models;

namespace RoutingInASPNETCoreWebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class EmployeeController : ControllerBase
    {
        // GET api/Employee/Search?Gender=Male&Department=IT&City=Los Angeles
        [HttpGet("Search")]
        public ActionResult<IEnumerable<Employee>> SearchEmployees([FromQuery] string? gender, [FromQuery] string? department, [FromQuery] string? city)
        {
            var filteredEmployees = EmployeeData.Employees.AsQueryable();

            if (!string.IsNullOrEmpty(gender))
                filteredEmployees = filteredEmployees.Where(e => e.Gender.Equals(gender, StringComparison.OrdinalIgnoreCase));

            if (!string.IsNullOrEmpty(department))
                filteredEmployees = filteredEmployees.Where(e => e.Department.Equals(department, StringComparison.OrdinalIgnoreCase));

            if (!string.IsNullOrEmpty(city))
                filteredEmployees = filteredEmployees.Where(e => e.City.Equals(city, StringComparison.OrdinalIgnoreCase));

            var result = filteredEmployees.ToList();

            if (!result.Any())
                return NotFound("No employees match the provided search criteria.");

            return Ok(result);
        }
    }
}
```
* **Optional Parameters**: The **?** in string? makes the parameter optional.
* **Dynamic Filtering**: Filters are applied only when values are provided.
* **Parameter Order**: The order of query parameters in the URL does not matter.

##### Testing the Endpoint

**Filter by Gender Only:**
Let’s say we want to filter the employees by Gender only. Then, you can only pass the Gender query string parameter in the URL (**/api/Employee/Search?Gender=Male**), as shown in the image below.

![image_292.png](image_292.png)

**Filter by Gender and Department:**
Now, if you want to search employees by **Gender** and **Department**, then you need to pass two query string parameters in the URL (**/api/Employee/Search?Gender=Male&Department=IT**), as shown in the image below.

![image_293.png](image_293.png)

#### Combining Route Parameters and Query Strings in ASP.NET Core Web API

ASP.NET Core Web API allows combining both **Route Parameters** and **Query Strings** for maximum flexibility within the same endpoint. Let’s consider an example where we have an API endpoint to get employee details with the following criteria:

* **Route Parameter (mandatory)**: Gender (e.g., Male, Female) identifies a subset of employees.
* **Query Strings (optional)**: Department and City provide additional filtering within the specified gender.

So, modify the **Employee Controller** as follows:

```C#
using Microsoft.AspNetCore.Mvc;
using RoutingInASPNETCoreWebAPI.Data;
using RoutingInASPNETCoreWebAPI.Models;

namespace RoutingInASPNETCoreWebAPI.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class EmployeeController : ControllerBase
    {
        // GET api/Employee/Gender/Male?Department=IT&City=Los Angeles
        [HttpGet("Gender/{gender}")]
        public ActionResult<IEnumerable<Employee>> GetEmployeesByGender([FromRoute] string gender, [FromQuery] string? department, [FromQuery] string? city)
        {
            var filteredEmployees = EmployeeData.Employees
                .Where(e => e.Gender.Equals(gender, StringComparison.OrdinalIgnoreCase));

            if (!string.IsNullOrEmpty(department))
                filteredEmployees = filteredEmployees.Where(e => e.Department.Equals(department, StringComparison.OrdinalIgnoreCase));

            if (!string.IsNullOrEmpty(city))
                filteredEmployees = filteredEmployees.Where(e => e.City.Equals(city, StringComparison.OrdinalIgnoreCase));

            var result = filteredEmployees.ToList();

            if (!result.Any())
                return NotFound("No employees match the provided search criteria.");

            return Ok(result);
        }
    }
}
```

* **Route Parameter**: {**gender**} is part of the route path, identifying a subset of employees.
* **Query Strings**: **department** and **city** provide additional optional filtering.

Now, with the above changes in place, run the application and search for employees by Gender as Route Data, Department, and City as query strings in the URL (**/api/Employee/Gender/Male?Department=IT&City=Los Angeles**). You should get the following result.

![image_294.png](image_294.png)
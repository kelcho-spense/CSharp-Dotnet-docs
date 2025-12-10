# Basics of Web APIs


## Why do we need Web APIs?

In today’s multi-device world, Web APIs are the backbone of modern applications, powering websites, mobile apps, and even smart devices. Let us understand the need for Web APIs with some realistic examples.

#### Step 1 – Initial Setup (Only Website + Database)

Suppose you have an idea to develop and launch a product. For this, you need to develop a website and launch this product. Then what will you do? The simplest setup would be:

* A **Website** built using **ASP.NET MVC**, **ASP.NET Core**, **PHP**, **JSP**, or any other **server-side technology** that is available in the market.
* Of course, a **Database** such as **SQL Server**, **MySQL**, or **Oracle** to store all your product’s business data.

With this setup, your website becomes a dynamic, fully functional system that can create, read, update, and delete data from the database.

![image_243.png](image_243.png)

#### Step 2 – Business Grows (Website + Android + iOS Apps)

Over time, your business grows. Now, besides the website, you also want:

* An **Android Mobile App**
* An **iOS Mobile App**

This means:

* You now have **three different applications** (Website, Android, iOS).
* But you still have **only one database** that stores all your data.
* So, we have three different applications and one database.

Now, all these three applications have to interact with the database, as shown in the below image.

![image_244.png](image_244.png)

#### Step 3 – Problems With Direct Database Access

If all these three applications directly interact with the database, we have some problems. Let us understand the problems first, and then we will see how to overcome the above problems:

* **Duplicate Logic Across Applications**: Each application must implement the same business logic independently. This causes Code Duplication.
* **Time-Consuming and Error-Prone**: Writing the same logic in three places increases the chances of mistakes. If a piece of logic changes, you need to update it everywhere. This will add more errors to your application
* **Front-End Framework Limitations**: Some frameworks (like Angular or React) cannot directly talk to a database, because they are client-side technologies. They need a middle layer.
* **Hard to Maintain**: If you need to update or fix business logic, you must do so in multiple applications, which is a complex and inefficient process.

> In short, **direct database communication is messy, unsafe, and hard to scale**. There are also other problems that we face in this structure. Let us see how to overcome the above problems, or, in other words, why we need Web APIs.

## The Need for Web APIs

As you can see in the image below, we have three applications on the left-hand side, and on the right-hand side, we have the database.

![image_245.png](image_245.png)

We aim to establish communication between all three applications and the database to manage the data. So, what will we do? We will add a **Web API layer** between the **frontend applications** (**website**, **Android**, **iOS**) and the **backend database**.

* Instead of each application directly accessing the database, they all communicate with the **Web API**.
* The **Web API** contains all the **business logic** and manages all communication with the database.
* This makes the **Web API act as a mediator** between the frontend and the backend.

As a result, the database becomes secure, logic is centralized, and applications become easier to maintain and extend.

![image_246.png](image_246.png)

> Note: The Website, Android, and iOS applications do not have direct access to the database. They only need to communicate with the Web API Project, and it is the Web API project’s responsibility to interact with the database. The entire business logic will be written in the Web API project only. So, Web API acts as a mediator between the Front-End and Back-End.

## What is Rest?

**REST (Representational State Transfer)** is not a technology or a framework, but an **architectural style** for designing **Distributed Systems**. Its primary purpose is to define how **clients** (like browsers, mobile apps, or other applications) and **servers** (back-end services) should communicate over a network.

The beauty of REST lies in its **Platform Independence**:

* The client could be written in Java, .NET, Angular, React, or any other technology.
* The server could be built using Node.js, ASP.NET Core, PHP, Python, or Java.
* As long as they both follow REST principles and communicate via **HTTP**, they can exchange data seamlessly

REST treats **Everything as a resource**: books, users, orders, products, etc. Each resource is uniquely identified by a **URI (Uniform Resource Identifier)**, and standard **HTTP methods** (GET, POST, PUT, DELETE, PATCH, etc.) are used to perform operations (CRUD – Create, Read, Update, Delete) on these resources.

### Example – Book Management System API:

* GET /books → Retrieve all books.
* GET /books/{id} → Retrieve details of a specific book.
* POST /books → Add a new book.
* PUT /books/{id} → Update details of a book.
* DELETE /books/{id} → Delete a book.

This clear and uniform way of accessing resources is what makes REST widely adopted in modern web APIs.

### What are the REST Principles?

REST defines **six key principles (constraints)** that make a system truly RESTful. Let us proceed and understand the REST constraints or principles.

![image_247.png](image_247.png)

#### Client-Server Architecture:

REST enforces a clear separation of responsibilities:

* **Client** → Handles user interface, presentation, and experience.
* **Server** → Handles business logic, processing, and database/storage.

They communicate using **a standard interface (HTTP)**.

Why it matters:

* Client and server can be developed, deployed, and scaled **independently**.
* UI changes (say, redesigning a mobile app) don’t affect the back-end logic.
* Server upgrades (such as database migrations) don’t break clients.

> Example: In Instagram, your mobile app (client) fetches profile data via the GET /users/{id} endpoint. The server retrieves the data and returns it, regardless of whether the request originated from iOS, Android, or a web browser.

#### Stateless:
**The stateless constraint specifies that client-server communication must be stateless between requests.** Each request from the client-server must be **independent**. The server does **not store any session data** between requests. Every request from the client should carry **all necessary information** (authentication, parameters, etc.) for the server to process it.

**Why it matters:**

* Any server in a cluster can handle any request (better scalability).
* Simplifies server design (no need to track client state).
* Improves reliability (no “session lost” issues).
* **Each request from the clients can be treated independently by the server.**

> Example: When a client calls GET /orders/789, it sends an Authorization token in headers. The server validates the token, fetches the order, and responds. The server doesn’t need to remember past interactions.

#### Cacheable:
**In real-time applications, some data provided by the server is not changed frequently, such as the list of Countries, States, Cities, and products**. REST allows clients (and intermediate proxies) **to cache responses, improving performance and reducing** server load. The server informs the client whether a response is cacheable and for how long, using headers such as Cache-Control or Expires.

**Why it matters:**

* Faster responses for frequently accessed data.
* Reduces redundant server calls.
* Optimizes bandwidth usage.
* **Frequently requested resources can be served from a cache instead of re-fetching from the server**.
* **Reduces the number of round-trip requests to the server**.

**Example:**

* A request GET /countries may rarely change. The server responds with:
* Cache-Control: max-age=86400
* This means the client can reuse the response for 24 hours without needing to request it again from the server.

#### Uniform Interface:
The most critical principle: all resources are accessed in a **consistent and standardized way**. This makes APIs predictable and easier to use.

It has four sub-constraints:

* **Resource Identification** → A URI uniquely identifies each resource. Example: /users/1001 identifies a specific user.
* **Manipulation via Representations** → Clients modify resources by sending their representation (JSON/XML). Example: PUT /users/1001 with JSON body updates a user.
* **Self-Descriptive Messages** → Requests and responses carry enough information (HTTP method, headers, content type) so clients/servers understand how to process them.
* **HATEOAS (Hypermedia as the Engine of Application State)** → Responses guide clients with links to available next actions.

Example: A response for a book might include links like:

```json
{
  "Id": 101,
  "Title": "REST Basics",
  "Links": [
    {"rel": "update", "href": "/books/101", "method": "PUT"},
    {"rel": "delete", "href": "/books/101", "method": "DELETE"}
  ]
}
```

**Why it matters:**

* Clients know exactly how to interact with the API.
* Reduces coupling between client and server.
* Easier for new clients to consume APIs.

**Examples:**

* GET https://example.com/books retrieves a list of books.
* GET https://example.com /books/123 retrieves a book with ID 123.
* POST https://example.com/books creates a new book.
* PUT https://example.com/books/123 updates the book with ID 123.
* DELETE https://example.com/books/123 deletes the book with the specified ID.

#### Content Negotiation:
Clients and servers can negotiate the media type used for representing a resource. The client typically specifies its preferred format via the Accept header, and the server responds in one of the requested formats if supported.

**Why It Matters:**

* Allows different clients (mobile apps, browsers, etc.) to request the format they can best handle (e.g., JSON, XML, HTML).
* Servers can support multiple formats without changing the resource identifier.

Example: If a client requests an article by calling GET /articles/456 and includes Accept: application/json, the server can respond with JSON (e.g., Content-Type: application/json). If the client had requested Accept: application/xml, the server could respond with XML if it supports it.

#### Layered System:
REST allows APIs to be built as a **layered architecture**. Each layer has its own responsibility (e.g., security, caching, load balancing), and clients are unaware of the number of layers that exist.

**Why it matters:**

* Scalability → Easy to add caching servers, gateways, or load balancers.
* Security → Middle layers can handle authentication or rate limiting.
* Flexibility → Layers can develop independently.

**Example:**
In an e-commerce app:

1. **Layer 1**: Web Server handles client requests.
2. **Layer 2**: Application Layer applies business logic.
3. **Layer 3**: Database stores data.

Clients don’t know about internal layering; they just see a single API endpoint.

#### Code on Demand (Optional)
A REST API may optionally send **executable code** to clients (e.g., JavaScript). This allows extending client functionality without additional requests. **This is an optional constraint used to add flexibility for the client**.

Why it matters:

* Reduces server load.
* Allows clients to perform operations locally.

Example: A server sends a JavaScript snippet with sorting logic for a product list. The client runs it locally, avoiding repeated server requests for sorting.

## HTTP (HyperText Transport Protocol)

The Hypertext Transfer Protocol (HTTP) is the foundation of communication on the World Wide Web, enabling interaction between clients and servers. It defines how requests and responses are exchanged, ensuring that resources such as web pages, images, stylesheets, and APIs are delivered correctly and efficiently.

## What is HTTP?

HTTP (Hypertext Transfer Protocol) is the backbone of data communication on the web. It defines the rules for how clients (like browsers or apps) send requests to servers and how servers respond with data. This protocol ensures that when you access a website, the content (HTML, images, CSS, scripts) is transmitted properly between your device and the server hosting the site. HTTP works as a request–response protocol where the client initiates communication and the server fulfills it.

**Important Points**

* HTTP stands for **Hypertext Transfer Protocol**.
* It is the foundation of communication on the World Wide Web.
* Defines how messages are formatted and transmitted.
* Enables browsers, apps, and tools to send **requests** and receive **responses**.
* Operates in a **client–server model**.

**Example**

When you type **https://www.example.com** into your browser and press Enter:

* The browser sends an **HTTP Request** to the server hosting **example.com**.
* The server replies with an **HTTP Response** containing the **HTML page**.
* The browser then displays that page to you.

### How Client and Server Communicate

Communication between a browser (client) and a web server occurs through HTTP, in the form of requests and responses, within a **client–server architecture**.

![image_248.png](image_248.png)

Important Points

* **Client**: Browser, mobile app, or tool (like Postman, Swagger, Fiddler).
* **Server**: Hosts the web application or API.
* **Request**: Data sent from client to server (e.g., asking for a webpage or JSON data).
* **Response**: Data sent from the server to the client (e.g., HTML page, JSON).
* **Protocol**: Communication is possible only through HTTP/HTTPS.

### HTTP Request Components

An **HTTP Request** is the message sent by the client (browser, mobile app, Postman, Swagger, etc.) to the server asking for a resource or action. Every request must follow a specific structure so that the server can understand what the client needs. It consists of a **request line**, **headers**, an optional **body**, and sometimes **query parameters** in the URL. Together, these components define **what resource is being requested, how it should be processed**, and **any additional data or context needed**.

![image_249.png](image_249.png)

#### Request Line:

The **Request Line** is the first line of an HTTP request and specifies to the server the exact action the client wants to perform, the resource it is targeting, and the HTTP version it supports. It always contains three parts:

HTTP Method: This represents the type of operation the client wants to perform on the server. For example:

* **GET** → Retrieve a resource without making changes (e.g., viewing a webpage).
* **POST** → Send new data to the server for processing (e.g., submitting a login form).
* **PUT** → Update an existing resource completely.
* **DELETE** → Remove a resource from the server.
* Other methods include **PATCH** (Partial Update), **HEAD** (request headers only), and **OPTIONS** (check supported methods).

#### Request Headers:
After the request line, the client usually sends **one or more headers**. These headers serve as additional instructions, providing more detailed descriptions of the request. They are written in **key-value** format (e.g., Host: www.example.com) and each appears on a separate line. Common request headers include:

* **Host**: Specifies the domain name of the target server (Host: www.example.com).
* **User-Agent**: Identifies the client software making the request (e.g., browser, mobile app, Postman, etc.).
* **Accept**: Lists the content types the client is willing to accept from the server (e.g., HTML, JSON, XML).
* **Content-Type**: When the request includes a body (like a POST, PUT, or PATCH request), this header indicates the format of the request body. Example: application/json, application/xml, etc.
* **Authorization**: Carries credentials, such as API keys or tokens, for authentication.
* **Cookie**: Includes any cookies previously stored by the browser to maintain sessions. This is used for state management.
* **Cache-Control**: Provides caching rules for both client and server.

**Example:**

* Host: www.example.com
* User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36
* Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8

#### Request Body (Optional):
The request body is the part of an HTTP request that carries data from the client to the server.

* The body of an HTTP request is **optional** and is generally used in methods such as **POST, PUT**, and **PATCH**, where the client needs to send data to the server for processing.
* The data can also be:
    * Form submissions (e.g., login details).
    * File uploads (images, PDFs).
    * JSON or XML payloads for APIs.

**Example Body (JSON in a POST Request):**

For instance, in a POST request, the body would contain the JSON data as follows.

```JSON
{
  "Username": "ExampleUser",
  "Password": "ExamplePass"
}
```

#### Query Parameters:
Query parameters are part of the URL itself. They are used to send extra details to the server in the form of key-value pairs.

* They start with a **question mark (?)**.
* They are sent in the form of a key-value pair.
* Multiple parameters are separated by a**n ampersand (&)**.
* These parameters are beneficial for **search**, **filtering**, **sorting**, or **pagination** operations on the server.

**Example URL with Query Parameters:**
_https://example.com/search?q=example&sort=asc_

**q=example**: The query parameter q has the value example.
**sort=asc**: The query parameter sort has the value asc.

### HTTP Response Components

An HTTP response is the message sent from the **Server** back to the **Client** after processing a request. The response informs the client whether the request was successful or not and may also include the requested content or an error message. An HTTP response is composed of a **Status Line**, **Response Headers**, and optionally, a **Response Body**. This response structure is crucial for the client to understand what action the server took and what data is being returned. For a better understanding, please have a look at the following diagram:

![image_250.png](image_250.png)

**Response Line:**

The **Response Line** (also known as the **Status Line**) is the first line of an HTTP response. It provides the client with essential information about the response that the server is sending back. It is always made up of three parts:

**HTTP Version**: Tells the client which version of HTTP the server is using. Common versions include **HTTP/1.1** (still widely used), **HTTP/2** (faster with multiplexing), and **HTTP/3** (the latest, offering improved performance and security).

**Status Code**: A **three-digit number** that represents the outcome of the client’s request. The Status codes are grouped into categories:

1. 1xx (Informational): Request received, processing continues.
2. 2xx (Success): Request was successful (e.g., 200 OK, 201 Created).
3. 3xx (Redirection): Client must take additional steps (e.g., 301 Moved Permanently).
4. 4xx (Client Errors): The request has an error (e.g., 400 Bad Request, 404 Not Found).
5. 5xx (Server Errors): The server failed to process the request (e.g., 500 Internal Server Error).

**Status Text**: A short, human-readable description of the status code (e.g., OK for 200, Not Found for 404).

**_Example Response Line: HTTP/1.1 200 OK_**
This means:

* The server used HTTP version 1.1,
* The status code is 200 (success),
* The status text is OK.

**Response Headers:**

Each HTTP Response can have one or more Response Headers. The **Response Headers** come immediately after the response line and are composed of key-value pairs. These headers provide **additional information** about the response and the server. Each header is on its own line. Common response headers include:

* **Content-Type**: Specifies the type of data in the response body (e.g., text/html for webpages, application/json for APIs).
* **Content-Length**: Specifies the size of the response body in bytes (helps the client know how much data to expect).
* **Server**: Provides information about the server software that handled the request (e.g., Apache, Nginx, IIS, Kestrel, etc.).
* **Set-Cookie**: Used to send cookies from the server to the client browser. This is useful for session management and personalization.
* **Cache-Control**: Tells the client or proxy how caching should be handled (e.g., store for a day, don’t cache at all).
* **Date**: The timestamp when the server sent the response

**Example Response Headers:**

* Content-Type: text/html; charset=UTF-8
* Content-Length: 138
* Server: Apache/2.4.41 (Ubuntu)

**Response Body:**

The Response Body is the part of the HTTP response that carries the **actual data** being returned to the client. Whether or not a body is included depends on the request method and the status code. For example:

* A GET request typically includes a body containing the requested content.
* A 204 No Content response intentionally has no body.

The data in the response body can be in various formats, such as:

* HTML (for rendering web pages),
* JSON or XML (for APIs),
* Plain text,
* Images, files, or other media types.

The format and presence of the body depend on the Content-Type header sent by the server.

Example JSON Response Body:

````JSON
{
  "Id": 123,
  "Name": "John Doe",
  "Email": "john.doe@example.com"
}
````

### HTTP Verbs or HTTP Methods:

GET HTTP Method:
The GET method is the most commonly used HTTP method. It is designed to retrieve information from the server without changing anything on the server.

POST HTTP Method:
The POST method is used to send data to the server, usually resulting in the creation of a new resource or triggering a process. Unlike GET, it modifies the server state by adding or processing data.

**_Example:_**

POST /articles HTTP/1.1

Host: example.com

Content-Type: application/json
```JSON
{
  "Title": "New Article",
  "Content": "This is the content of the new article."
}
```

PUT HTTP Method:
The PUT method is used to completely update or replace an existing resource with the new data provided in the request body. If the resource does not exist, some servers may create it; however, the primary purpose is typically to perform a complete update or replacement of the existing resource.

**_Example:_**

PUT /articles/101 HTTP/1.1

Host: example.com

Content-Type: application/json

```JSON
{
  "Id": 101,
  "Title": "Updated Article",
  "Content": "This is the updated content of the article."
}
```

> Note: Ensure the ID in the URL path (/articles/101) matches the resource identifier in the body (“Id”: 101) if your API design requires it.

PATCH HTTP Method:
The PATCH method is used for Partial Updates to an existing resource. Unlike PUT, which replaces the entire resource, PATCH updates only the fields provided in the request body. This makes PATCH more efficient when only a subset of data needs to be changed.

**_Example:_**

PATCH /articles/1 HTTP/1.1

Host: example.com

Content-Type: application/json

```JSON
{
  "Content": "This is the updated content of the article."
} 
```

> Note: Commonly, you do not include the resource’s ID in the body if the ID is already specified in the URL, unless your API design specifically requires it.

DELETE HTTP Method:
The DELETE method is used to remove a resource from the server. Once deleted, attempting to access the same resource should ideally return a 404 Not Found error or indicate that it’s inactive.
Many applications use either soft delete (marking an item as inactive) or hard delete (permanently removing an item).

**_Example:_**

DELETE /articles/1 HTTP/1.1

Host: example.com

**Deletion Strategies**: In modern applications, we can implement Delete Functionality in two ways: Soft Delete and Hard Delete.

### HTTP Status Code Categories:
HTTP status codes are three-digit numbers sent by the server in an HTTP response. They inform the client whether the request was successful, requires further action, or has failed due to an error.

They are as follows: Here, XX will represent the actual number.

1. 1xx (Informational Responses): Request received; server is working on it.
2. 2xx (Successful Responses): Request understood and successfully processed.
3. 3xx (Redirection Responses): Further action is needed to complete the request.
4. 4xx (Client Error Responses): There was an issue with the client’s request.
5. 5xx (Server Error Responses): The server encountered an issue processing the request.

#### 1XX: Informational Response Status Codes

The 1xx category represents informational responses.

**Examples:**

* **100 Continue** → Server acknowledges headers and asks the client to send the request body (useful before sending large data).
* **102 Processing** → Server is still working on the request, but hasn’t finished (used in WebDAV and long tasks).

#### 2XX: Successful Response Status Codes

The 2xx category indicates that the client’s request was successfully received, understood, and processed.

**Examples:**
* **200 OK**: The standard response for successful HTTP requests. The requested resource (or data) is usually returned in the response body. For example, retrieving a web page or fetching data from an API.
* **201 Created**: The request was successful, and a new resource was created as a result. It is commonly used for POST requests. For example, creating a new user account or posting a new article.
* **202 Accepted**: The request has been accepted for processing, but it has not yet been completed. This is often used for asynchronous tasks. For example, initiating a background job or asynchronous task.
* **204 No Content**: The server successfully processed the request and is not returning any content. Often used after a successful **DELETE** or **PUT** operation when there is no need to return data. For example, deleting a resource or updating a record without returning the updated data.

#### 3XX: Redirection Response Status Codes
The **3xx category** indicates that the client must take further action to complete the request, typically by making another request to a different location. These codes are essential for **redirecting users** or handling resource movements, such as when websites change their domain names.

**Examples:**

* **301 Moved Permanently**: The resource has been permanently moved to a new URI provided in the Location header. The Ideal use case is redirecting from an old domain to a new one. Future requests should use the new URI.
* **302 Found**: The resource is temporarily located at a different URI. The client should use the new URI for this request, but future requests can still use the original URI. An ideal use case is temporary redirection during maintenance.

> Real-Life Example: If you type http://example.com, the server may respond with 301 Moved Permanently to redirect you to https://example.com.

#### 4XX: Client Error Response Status Codes
The **4xx category** indicates that the client made an error in the request. These errors are caused by incorrect input, unauthorized access, or asking for resources that don’t exist.

Examples:
**400 Bad Request**: The server cannot process the request due to malformed syntax or invalid data. For example, sending malformed JSON in a POST request.
**401 Unauthorized**: Authentication is required and has failed or has not been provided. For example, accessing a protected resource without valid credentials.
**403 Forbidden**: The server understands the request but refuses to authorize it. For example, accessing a resource without sufficient permissions.
**404 Not Found**: The requested resource could not be found on the server. For example, requesting a non-existent webpage or API endpoint.
**405 Method Not Allowed**: The HTTP method used in the request is not supported for the requested resource. For example, sending a PUT request to an endpoint that only supports GET and POST.

> Real-Life Example: If you mistype a page URL like /artcles instead of /articles, the server responds with 404 Not Found.

#### 5XX: Server Error Response Status Codes
The 5xx category indicates the request was valid, but the server failed to fulfil it due to an internal problem.

**Examples:**

* **500 Internal Server Error**: A generic error message indicating an unexpected condition that prevented the server from fulfilling the request. For example, an unhandled exception in server-side code.
* **502 Bad Gateway**: The server, while acting as a gateway or proxy, received an invalid response from the upstream server. For example, failures in communication between proxy servers.
* **503 Service Unavailable**: The server is currently unable to handle the request due to temporary overload or maintenance. For example, scheduled maintenance periods or unexpected traffic spikes. The client should try the request again later.
* **504 Gateway Timeout**: The server, while acting as a gateway or proxy, did not receive a timely response from the upstream server. For example, slow responses from a backend service can lead to a timeout. The client may need to retry the request later.

> Real-Life Example: During a high-traffic sale, a website may return a 503 Service Unavailable error because its servers are overloaded.
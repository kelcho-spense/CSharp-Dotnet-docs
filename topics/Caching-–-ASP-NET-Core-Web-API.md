# Caching – ASP.NET Core Web API

Caching is the process of storing frequently accessed data in a temporary storage (known as the cache) so that subsequent requests for that data can be served faster. Instead of recalculating or re-fetching data from the database or an external API every time, the application retrieves the data from the cache if available. This reduces unnecessary computation and network calls, improving the performance and responsiveness of applications and reducing the load on database servers.

ASP.NET Core provides built-in caching mechanisms, making integrating caching into Web API projects easy. ASP.NET Core supports various caching strategies, such as:

* **In-Memory Caching**: Stores data on the same server where the application is running.
* **Distributed Caching**: Enables caching across multiple servers using external cache stores like Redis or SQL Server.
* **Response Caching**: Caches entire HTTP responses, particularly useful for web APIs that return data that doesn’t change frequently.

## Why is Caching Important in ASP.NET Core Web API?

Caching is essential for improving the performance and scalability of ASP.NET Core Web APIs. It reduces the time required to serve client requests, decreases the load on backend services, and helps manage server resources more efficiently.

* **Performance** and Speed: By reducing round-trips to the database or external services, our API can respond to client requests more quickly.
* **Scalability**: When fewer resources are consumed, the server can handle more concurrent requests with the same hardware.
* **Cost Reduction**: If the database or third-party services charge per request or resource usage, especially in cloud-based environments where such operations may incur additional charges, caching can reduce costs by reducing the number of network calls.
* **Enhanced User Experience**: Faster response times lead to a better user experience, which is key for user retention and satisfaction.

#### Without Caching:

Each incoming request triggers the full processing pipeline. For example, a database query might be executed every time a user requests a particular resource, resulting in repeated I/O operations, increased load on the database server, and higher resource consumption, which can lead to slower performance under high traffic. That means every time a client requests data, the application must fetch it from the primary data source (e.g., a database or external API), perform any necessary computations, and then return the result. For a better understanding, please look at the following diagram:

![image_295.png](image_295.png)

**Without Caching**
* The client sends a request to the Web API.
* Web API receives the request and queries the database or an external service.
* The database processes the query and returns the data.
* Web API returns data to the client.
* Every request involves a call to the database or external service, even if the requested data is the same.

#### With Caching:

With Caching, when a request is made, the application first checks if the data is available in the cache. If the data is found (a cache hit), it is returned immediately, bypassing the need for a full data fetch. If the data is not in the cache (a cache miss), it is retrieved from the primary data source and stored in the cache for future requests. By serving cached data, the system reduces the number of calls to the backend systems. This leads to faster response times and more efficient resource usage. For a better understanding, please look at the following diagram:

![image_296.png](image_296.png)

**With Caching**
* The client sends a request to the Web API.
* Web API checks if the requested data is already in the cache.
  * If the data exists in the cache (cache hit), return it immediately from the cache.
  * If the data does not exist in the cache (cache miss), make a call to the database or external service, retrieve the data, and store it in the cache.
* Web API returns the data to the client.
* The data is quickly retrieved from the cache for subsequent requests, bypassing expensive operations.

With caching, the Web API only fetches data from the database or external service on the first request or when the cache expires/invalidates.

#### What is a Cache Hit and a Cache Miss?

* **Cache Hit**: When a requested item is found in the cache, it’s referred to as a cache hit. The application can quickly retrieve and return the data without performing additional processing. No external or database call is required, resulting in a very fast retrieval.
* **Cache Miss**: When a requested item is not found in the cache, it’s called a cache miss. In this case, the application must fetch the data from the original source (e.g., database or external API) and store it in the cache for future use.
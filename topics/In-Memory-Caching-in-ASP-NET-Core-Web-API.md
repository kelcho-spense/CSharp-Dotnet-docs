# In-Memory Caching in ASP.NET Core Web API

In-Memory Caching in ASP.NET Core is a mechanism for storing frequently accessed data in the server’s memory (RAM) to improve application performance and reduce database load. When a client makes a request that involves data retrieval, especially data that does not change often (e.g., master data like countries, states, cities), caching that data in memory helps the application to quickly serve subsequent requests without repeatedly querying the database.

For a better understanding of How In-Memory Caching Works in ASP.NET Core Web API Applications, please have a look at the following diagram:

![image_297.png](image_297.png)

#### Work Flow of In-Memory Caching in Web API:
* **Client Request**: The process starts when a client sends a request to the API.
* **API Endpoint**: The request is received by the API endpoint, which then processes the incoming request.
* **Cache Check (“Cache Exists?”)**: The API checks whether the requested data is available in the in-memory cache.
  * If the data exists, the API retrieves it from the cache.
  * If the data is not found in the cache, the API queries the data source. After fetching, it stores the result in the cache.
* **Return Data**: Finally, the data is returned to the client.




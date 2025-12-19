# Redis Cache in ASP.NET Core Web API

Distributed Caching is a caching mechanism where data is stored in an external cache system that can be accessed by multiple application servers simultaneously. That means Distributed caching (such as Redis, NCache, etc.) allows multiple application instances to share cached data. Unlike an in-memory cache that is local to one application instance, a distributed cache is typically hosted on a dedicated cache server, i.e., a distributed cache stores data in an external system. All application servers connect to this cache server (external system) to get/set cached data. 

## When Should We Use a Distributed Cache Instead of In-Memory Cache?
In-memory caching is fast but is limited to a single application instance. If the application is deployed across multiple servers or containers, each instance will have its own separate in-memory cache. This can lead to issues such as cache duplication, increased memory usage, and inconsistent data. In these cases, distributed caching is preferred because it provides a centralized cache that all application instances can access. Additionally, distributed caching is more reliable if an application instance goes down, as the cached data remains available in the external cache-store. So, we need to use a Distributed Cache Instead of In-Memory Cache in the following scenarios:

* **Multiple Server Instances**: If your application runs on multiple servers behind a load balancer, you need a cache that all servers can share. An in-memory cache would be unique to each server, causing inconsistent data or missing data if the request is routed to a different server.
* **Memory Constraints**: Maintaining large caches in each server’s memory can be expensive. Offloading the cache to an external system can be more cost-effective and memory-efficient on the app servers.
* **Persistence Across Restarts**: In-memory caches are lost whenever the application restarts. Distributed cache solutions can offer persistence and advanced features like replication, meaning your data remains available even if an app server goes down.
* **High Scalability**: In scenarios with multiple application servers, using an in-memory cache can lead to data inconsistency or duplication because each server holds its own cache. A distributed cache maintains a single source of cached data accessible to all servers.
* **High Availability**: With load balancing, a distributed cache ensures that a cache hit is possible regardless of which server handles the request.

### Distributed Caching Architecture:
Let us first understand the architecture of Distributed Caching, and then we will see how to implement Distributed caching using `Redis` cache in our ASP.NET Core Web API Application. In a distributed caching architecture, several components work together to manage, retrieve, and store cached data efficiently. These components include users, a load balancer, application servers, the distributed cache, and the database. To understand the architecture of Distributed Caching, please have a look at the following diagram.

![image_298.png](image_298.png)

### How Distributed Caching Works:
* **User Request**: Users send a request to the application, which first passes through the load balancer.
* **Load Balancer**: The load balancer routes the request to one of the application servers.
* **Cache Check**: The application server checks if the data is available in the distributed cache.
  * **Cache Hit**: If the requested data is found (cache hit) in the distributed cache, the data is retrieved quickly from the cache.
  * **Cache Miss**: If the data is not found in the cache, it is called a Cache Miss. In this case, the App Server queries the Database to retrieve the data.
* **Store Data in Cache**: After retrieving the data from the database, the app server stores it in the distributed cache. Subsequent requests for the same data can be served from the cache instead of the database.
* **Return Data**: The data, whether retrieved from the cache or database, is returned to the user.

### Running Redis in docker

To download the latest [Redis image](https://redis.io/downloads/), run the following command:

![image_300.png](image_300.png)

```bash
docker pull redis:latest

```

If you wish to pull a Specific Version of Redis:
````Bash
docker pull redis:8.0
````

Verify the image has been downloaded

```Bash
docker images
```

Once the image is downloaded, you can proceed to run the Redis container as you initially intended:

```Bash
docker run -d --name redis -p 6379:6379 redis:latest
```
This will run the Redis container in detached mode with the latest image.

Download [Redis Insight](https://redis.io/downloads/) app to view your redis cache db

![image_299.png](image_299.png)

## Project setup

Create a new ASP.NET Core Web API project named `RedisCachingDemo`. Once you create the project, we need to add the following Packages for Entity Framework Core to work with the SQL Server database and Redis cache. You can install these packages using the following commands in the Package Manager Console.

```Bash
Install-Package Microsoft.EntityFrameworkCore.SqlServer
Install-Package Microsoft.EntityFrameworkCore.Tools
Install-Package Microsoft.Extensions.Caching.StackExchangeRedis
```

> To connect and start caching data from .NET Core applications with Redis cache, we need `Microsoft.Extensions.Caching.StackExchangeRedis` package.

### Define the Database Model and DbContext

`Product.cs`

```C#
namespace RedisCachingDemo.Models
{
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Category { get; set; }
        public int Price { get; set; }
        public int Quantity { get; set; }
    }
}

```

### Create Db Context:
Next, we need to create the DbContent class. First, create a new folder named Data in the project root directory. Then, inside the `Data` folder, create a class file named `ApplicationDbContext.cs`

`ApplicationDbContext.cs`

```C#
using Microsoft.EntityFrameworkCore;
using RedisCachingDemo.Models;

namespace RedisCachingDemo.Data
{
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
        // Override this method to configure the model and seed initial data
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            base.OnModelCreating(modelBuilder);
            // Seed initial data for testing purposes.
            // Each Product instance must have a unique Id.
            var initialProducts = new List<Product>
            {
                new Product
                {
                    Id = 1,
                    Name = "Apple iPhone 14",
                    Category = "Electronics",
                    Price = 999,
                    Quantity = 50
                },
                new Product
                {
                    Id = 2,
                    Name = "Samsung Galaxy S22",
                    Category = "Electronics",
                    Price = 899,
                    Quantity = 40
                },
                new Product
                {
                    Id = 3,
                    Name = "Sony WH-1000XM4 Headphones",
                    Category = "Electronics",
                    Price = 349,
                    Quantity = 30
                },
                new Product
                {
                    Id = 4,
                    Name = "Nike Air Zoom Pegasus",
                    Category = "Footwear",
                    Price = 120,
                    Quantity = 100
                },
                new Product
                {
                    Id = 5,
                    Name = "Adidas Ultraboost",
                    Category = "Footwear",
                    Price = 180,
                    Quantity = 80
                },
                new Product
                {
                    Id = 6,
                    Name = "Organic Apples (1kg)",
                    Category = "Groceries",
                    Price = 4,
                    Quantity = 200
                },
                new Product
                {
                    Id = 7,
                    Name = "Organic Bananas (1 Dozen)",
                    Category = "Groceries",
                    Price = 3,
                    Quantity = 150
                }
            };
            // Instruct EF Core to seed this data into the 'Products' table
            modelBuilder.Entity<Product>().HasData(initialProducts);
        }
        // This DbSet maps to a Products table in the database
        public DbSet<Product> Products { get; set; }
    }
}

```

### Configure Connection String and Redis Cache Settings:
Next, please modify the `appsettings.json` file as follows. Here, the Entity Framework Core will create the database named `redis_caching_demo_db` if it has not already been created in the SQL server. Also, we have added a section specifying the Redis connection details (host and port) and an instance name (prefix) to avoid key collisions. Please remember that 6379 is the Port number on which our Redis Server is running.

```JSON
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "DefaultConnection": "Data Source=localhost;Initial Catalog=redis_caching_demo_db;Integrated Security=True;Pooling=False;Encrypt=True;Trust Server Certificate=True"
  },
  "RedisCacheOptions": {
    "Configuration": "localhost:6379",
    "InstanceName": "RedisCachingDemo"
  }
}

```

#### RedisCacheOptions
This is the section under which all Redis-related configurations are specified. We can name this section anything.

* **“Configuration”: “localhost:6379”**: This key `specifies the connection string for the Redis server`. Here, localhost is the host where the Redis server is running. localhost also **indicates that Redis is running on the same machine as your application**. If Redis were hosted elsewhere, we would specify the IP address or hostname of that server. `6379 is the port number on which the Redis server` listens for incoming connections.
* **“InstanceName”: “RedisCachingDemo”**: The InstanceName is a prefix used for all entries created by our application’s instance. This can be useful when multiple applications use the same Redis server, and we want to avoid key collisions and make it easier to identify which ones belong to which application. In this case, every key stored in Redis by this application will be prefixed with RedisCachingDemo.

### Configure the DbContext and Redis Cache in the Program Class:
Next, we need to configure our application to support Redis cache, and for this purpose, we need to register the **AddStackExchangeRedisCache** service. So, please modify the Program class as follows. The following code is self-explained, so please read the comment lines for a better understanding.

```C#
using Microsoft.EntityFrameworkCore;
using RedisCachingDemo.Data;
using StackExchange.Redis;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.

builder.Services.AddControllers();
// Learn more about configuring OpenAPI at https://aka.ms/aspnet/openapi
builder.Services.AddOpenApi();

builder.Services.AddControllers()
            .AddJsonOptions(options =>
            {
                // This will use the property names as defined in the C# model
                options.JsonSerializerOptions.PropertyNamingPolicy = null;
            });

// Configure DbContext to Use SQL Server Database
builder.Services.AddDbContext<ApplicationDbContext>(options => options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

// Register Redis distributed cache
builder.Services.AddStackExchangeRedisCache(options =>
{
    //This property is set to specify the connection string for Redis
    //The value is fetched from the application's configuration system, i.e., appsettings.json file
    options.Configuration = builder.Configuration["RedisCacheOptions:Configuration"];
    //This property helps in setting a logical name for the Redis cache instance. 
    //The value is also fetched from the appsettings.json file
    options.InstanceName = builder.Configuration["RedisCacheOptions:InstanceName"];
});
// Register the Redis connection multiplexer as a singleton service
// This allows the application to interact directly with Redis for advanced scenarios

// Establish a connection to the Redis server using the configuration from appsettings.json
builder.Services.AddSingleton<IConnectionMultiplexer>(sp => ConnectionMultiplexer.Connect(builder.Configuration["RedisCacheOptions:Configuration"]));

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

#### Create and Apply Database Migration:

```Bash
# Create migration
dotnet ef migrations add AddNewFeature

# Apply to database
dotnet ef database update
```

**EF Core will:**

* Create the database `redis_caching_demo_db` in our sql server. 
* Create all tables. 
* Insert the seed data defined in AppDbContext.
#### How to Use Redis for Cache in ASP.NET Core Web API:

Once the Redis server settings are configured in the Program class, we can inject the IDistributedCache service instance into our controllers or services to interact with Redis. The `IDistributedCache` interface provides `GetStringAsync`, `SetStringAsync`, and `RemoveAsync` methods in ASP.NET Core when Redis is used as the underlying cache provider.

##### **`SetStringAsync`**: 

This method stores a string value in the cache with an associated key and optional caching options (such as expiration policies). When we call SetStringAsync, it converts our string data into a byte array and stores it in Redis under the specified key.

![image_301.png](image_301.png)

**How It Works:**
  * We need to call **SetStringAsync(key, value, options)** to cache data.
  * The method serializes the string value (if required) and sends it to Redis to be stored.
  * The **DistributedCacheEntryOptions** allows us to set parameters like absolute or sliding expiration. This means the cached item can be automatically removed after a set duration or if it hasn’t been accessed for a specified period.
  * The cache is updated with the new value if the key already exists.

##### **`GetStringAsync`**: 

This method retrieves a cached value (stored as a string) associated with the provided key. We need to use this method to quickly check if the data is available in the cache before querying a slower data source like a database. After retrieval, we must deserialize the string to its original object type. The syntax is given below:

![image_302.png](image_302.png)

**How It Works:**
* When we call GetStringAsync(key), it sends an asynchronous request to the Redis server to fetch the value associated with the key.
* If the key exists, it returns the corresponding string value (typically serialized data like JSON).
* If the key is not found (a cache miss), it returns null.

##### **`RemoveAsync`**:

This method deletes the cache entry in Redis based on the specified key. We need to use this method to maintain cache consistency. For instance, if a product is updated or deleted, remove its cache entry so that the next read operation fetches fresh data. The syntax is given below:

![image_303.png](image_303.png)

**How It Works:**
* When you call RemoveAsync(key), it instructs Redis to delete the data associated with the provided key. The removal is performed asynchronously, allowing your application to continue processing while the cache entry is being deleted.
* This is useful for cache invalidation when the underlying data has changed (e.g., after a database update or delete operation).

### Using Redis Cache in ASP.NET Core Web API Controller:

To better understand, please create a new API Empty controller named **ProductsController** within the Controllers folder. The following code is self-explained, so please read the comment lines for a better understanding.

```C#
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Caching.Distributed;
using RedisCachingDemo.Data;
using RedisCachingDemo.Models;
using System.Text.Json;

// For more information on enabling Web API for empty projects, visit https://go.microsoft.com/fwlink/?LinkID=397860

namespace RedisCachingDemo.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class ProductsController : ControllerBase
    {
            private readonly ApplicationDbContext _context;
            private readonly IDistributedCache _cache;
            // Inject ApplicationDbContext and IDistributedCache via constructor.
            public ProductsController(ApplicationDbContext context, IDistributedCache cache)
            {
                _context = context;
                _cache = cache;
            }

            // GET: api/products/all
            [HttpGet("all")]
            public async Task<IActionResult> GetAll()
            {
                var cacheKey = "GET_ALL_PRODUCTS";
                List<Product> products;

                try
                {
                    // Attempt to retrieve the product list from Redis cache.
                    var cachedData = await _cache.GetStringAsync(cacheKey);
                    if (!string.IsNullOrEmpty(cachedData))
                    {
                        // Deserialize JSON string back to List<Product>.
                        products = JsonSerializer.Deserialize<List<Product>>(cachedData) ?? new List<Product>();
                    }
                    else
                    {
                        // Cache miss: fetch products from the database.
                        products = await _context.Products.AsNoTracking().ToListAsync();
                        if (products != null)
                        {
                            // Serialize the product list to a JSON string.
                            var serializedData = JsonSerializer.Serialize(products);
                            // Define cache options (using sliding expiration).
                            var cacheOptions = new DistributedCacheEntryOptions()
                                .SetSlidingExpiration(TimeSpan.FromMinutes(5));
                            // Store the serialized data in Redis.
                            await _cache.SetStringAsync(cacheKey, serializedData, cacheOptions);
                        }
                    }
                    return Ok(products);
                }
                catch (Exception ex)
                {
                    // Return a 500 response if any error occurs.
                    return StatusCode(500, new { message = "An error occurred while retrieving products.", details = ex.Message });
                }
            }

        // GET: api/products/Category?Category=Fruits
        [HttpGet("Category")]
        public async Task<IActionResult> GetProductByCategory(string Category)
        {
            // Cache key includes the category to ensure unique cache entries.
            var cacheKey = $"PRODUCTS_{Category}";
            List<Product> products;
            try
            {
                var cachedData = await _cache.GetStringAsync(cacheKey);
                if (!string.IsNullOrEmpty(cachedData))
                {
                    products = JsonSerializer.Deserialize<List<Product>>(cachedData) ?? new List<Product>();
                }
                else
                {
                    // Cache miss: fetch from the database by matching category.
                    products = await _context.Products
                        .Where(prd => prd.Category.ToLower() == Category.ToLower())
                        .AsNoTracking()
                        .ToListAsync();
                    if (products != null)
                    {
                        var serializedData = JsonSerializer.Serialize(products);
                        // Use absolute expiration so that the cache entry expires after 5 minutes.
                        var cacheOptions = new DistributedCacheEntryOptions()
                            .SetAbsoluteExpiration(TimeSpan.FromMinutes(5));
                        await _cache.SetStringAsync(cacheKey, serializedData, cacheOptions);
                    }
                }
                return Ok(products);
            }
            catch (Exception ex)
            {
                return StatusCode(500, new { message = "An error occurred while retrieving products.", details = ex.Message });
            }
        }
        // GET: api/products/{id}
        [HttpGet("{id}")]
        public async Task<IActionResult> GetProduct(int id)
        {
            // Cache key for a single product.
            var cacheKey = $"Product_{id}";
            Product? product;
            try
            {
                var cachedData = await _cache.GetStringAsync(cacheKey);
                if (!string.IsNullOrEmpty(cachedData))
                {
                    product = JsonSerializer.Deserialize<Product>(cachedData) ?? new Product();
                }
                else
                {
                    // Fetch from database if not present in cache.
                    product = await _context.Products.FindAsync(id);
                    if (product == null)
                        return NotFound($"Product with ID {id} not found.");
                    var serializedData = JsonSerializer.Serialize(product);
                    await _cache.SetStringAsync(cacheKey, serializedData, new DistributedCacheEntryOptions
                    {
                        SlidingExpiration = TimeSpan.FromMinutes(5)
                    });
                }
                return Ok(product);
            }
            catch (Exception ex)
            {
                return StatusCode(500, new { message = "An error occurred while retrieving the product.", details = ex.Message });
            }
        }
        // PUT: api/products/{id}
        [HttpPut("{id}")]
        public async Task<IActionResult> UpdateProduct(int id, [FromBody] Product updatedProduct)
        {
            if (id != updatedProduct.Id)
            {
                return BadRequest("Product ID mismatch.");
            }
            try
            {
                var existingProduct = await _context.Products.FindAsync(id);
                if (existingProduct == null)
                    return NotFound($"Product with ID {id} not found.");
                // Update product details in the database.
                _context.Entry(existingProduct).CurrentValues.SetValues(updatedProduct);
                await _context.SaveChangesAsync();
                // Update the cache for this product.
                var cacheKey = $"Product_{id}";
                var serializedData = JsonSerializer.Serialize(updatedProduct);
                await _cache.SetStringAsync(cacheKey, serializedData, new DistributedCacheEntryOptions
                {
                    SlidingExpiration = TimeSpan.FromMinutes(5)
                });
                return Ok();
            }
            catch (Exception ex)
            {
                return StatusCode(500, new { message = "An error occurred while updating the product.", details = ex.Message });
            }
        }
        // DELETE: api/products/{id}
        [HttpDelete("{id}")]
        public async Task<IActionResult> DeleteProduct(int id)
        {
            try
            {
                var product = await _context.Products.FindAsync(id);
                if (product == null)
                    return NotFound($"Product with ID {id} not found.");
                // Remove product from the database.
                _context.Products.Remove(product);
                await _context.SaveChangesAsync();
                // Remove product from the cache.
                var cacheKey = $"Product_{id}";
                await _cache.RemoveAsync(cacheKey);
                return Ok();
            }
            catch (Exception ex)
            {
                return StatusCode(500, new { message = "An error occurred while deleting the product.", details = ex.Message });
            }
        }
    }
}

```

#### Testing
Run the application and test that the Redis Cache is working as expected. Let’s try to access the API endpoint in Postman. The first request will take slightly longer to execute, but subsequent requests will improve the response time considerably. For me, it came down to just **7 milliseconds**, which is quite impressive.

![image_304.png](image_304.png)

check the redis Insight app to see the cached data 

![image_305.png](image_305.png)
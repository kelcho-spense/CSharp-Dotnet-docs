# Fluent API in LINQ

**LINQ** (Language Integrated Query) is a powerful feature in C# that allows querying data in a declarative manner. Fluent API refers to the ability to chain LINQ method calls together to express complex queries clearly. In this document, we will explain how to use LINQ’s Fluent API with a simple `Game` class example in a console application.

### Setup the Console Application

First, create a `.NET` console application. Define the `Game` class in `Game.cs` and add sample data in the `Program.cs`.

```C#
// Game.cs
using System;

namespace LearningLINQ
{
    internal class Game
    {
        public required string Title { get; set; }
        public string? Genre { get; set; }
        public int ReleaseYear { get; set; }
        public double Rating { get; set; }
        public int Price { get; set; }
    }
}
```

```C#
// Program.cs
using LearningLINQ;

var games = new List<Game>
{
    new Game { Title = "Galactic Frontier", Genre = "Sci‑Fi Strategy", ReleaseYear = 2022, Rating = 4.8, Price = 59 },
    new Game { Title = "Mystic Quest: The Hidden Temple", Genre = "Adventure / Puzzle", ReleaseYear = 2021, Rating = 4.5, Price = 49 },
    new Game { Title = "CyberStrike Arena", Genre = "FPS Multiplayer", ReleaseYear = 2023, Rating = 4.2, Price = 39 },
    new Game { Title = "Legends of the Shadow Realm", Genre = "Fantasy RPG", ReleaseYear = 2020, Rating = 4.7, Price = 54 },
    new Game { Title = "Pixel Racer: Turbo Drift", Genre = "Arcade Racing", ReleaseYear = 2023, Rating = 4.1, Price = 29 }
};
```

### Retrieving All Games

In the beginning, you may want to iterate through the entire list of games. This can be done using a `foreach` loop, but with LINQ, this can be handled more elegantly if needed for filtering or projection.

Example to retrieve all games (which would be similar to just using the `games` list directly):

```C#
foreach (var game in games)
{
    Console.WriteLine($"Title: {game.Title} | Genre: {game.Genre} | Release Year: {game.ReleaseYear} | Rating: {game.Rating}");
}
```

### Filtering Data

LINQ provides a powerful `Where` method to filter data based on conditions. This is useful when you want to find games of a specific genre, or that meet certain criteria.

#### Example: Filter by Genre

```C#
var sciFiGames = games.Where(game => game.Genre == "Sci‑Fi Strategy");

foreach (var game in sciFiGames)
{
    Console.WriteLine($"Title: {game.Title} | Genre: {game.Genre.ToLower()} | Release Year: {game.ReleaseYear} | Rating: {game.Rating}");
}
```

### Checking Conditions

LINQ also offers methods like `Any`, which allow you to check if any items in the collection satisfy a condition. This is useful for quick validations.

#### Example: Check if there are modern games

```C#
var modernGamesExist = games.Any(game => game.ReleaseYear > 2022);
Console.WriteLine($"Does there exist a modern game? {modernGamesExist}");
```

### Sorting Data

LINQ provides `OrderBy` and `OrderByDescending` methods to sort data in ascending or descending order based on one or more properties.

#### Example: Sorting by Release Year (Ascending)

```C#
var sortedGamesByYear = games.OrderBy(game => game.ReleaseYear);
Console.WriteLine("Games sorted by release year (ascending):");
foreach (var game in sortedGamesByYear)
{
    Console.WriteLine($"Title: {game.Title} | Genre: {game.Genre.ToLower()} | Release Year: {game.ReleaseYear} | Rating: {game.Rating}");
}
```

#### Example: Sorting by Release Year (Descending)

```C#
var sortedGamesByYearDesc = games.OrderByDescending(game => game.ReleaseYear);
Console.WriteLine("Games sorted by release year (descending):");
foreach (var game in sortedGamesByYearDesc)
{
    Console.WriteLine($"Title: {game.Title} | Genre: {game.Genre.ToLower()} | Release Year: {game.ReleaseYear} | Rating: {game.Rating}");
}
```

### Aggregating Data

LINQ offers several aggregation methods such as `Average`, `Max`, and `Min` for performing calculations on data sets.

#### Example: Calculate the Average Price

```C#
var averagePrice = games.Average(game => game.Price);
Console.WriteLine($"Average Game Price: ${averagePrice}");
```

#### Example: Find the Game with the Highest Rating

```C#
var highestRating = games.Max(game => game.Rating);
var bestGame = games.First(g => g.Rating == highestRating);
Console.WriteLine($"Highest rated game: {bestGame.Title} ({bestGame.Rating})");
```

### Chaining Multiple LINQ Methods

One of the strengths of Fluent API in LINQ is the ability to chain multiple methods together for more complex queries. You can filter, sort, and then aggregate the data all in a single chain.

#### Example: Filter, Sort, and Get Top Rated Game

```C#
var topRatedGame = games
    .Where(game => game.ReleaseYear > 2020)
    .OrderByDescending(game => game.Rating)
    .First();

Console.WriteLine($"Top rated game since 2020: {topRatedGame.Title} | Rating: {topRatedGame.Rating}");
```

### Projecting Data

You can also project data into different formats using the `Select` method. This can be useful when you only need certain fields or want to create new objects from the original data.

#### Example: Project Games with Title and Rating

```C#
var gameTitlesAndRatings = games.Select(game => new { game.Title, game.Rating });

foreach (var item in gameTitlesAndRatings)
{
    Console.WriteLine($"Game: {item.Title} | Rating: {item.Rating}");
}
```

### Summary of LINQ Concepts Used

* **Where**: Filter data based on a condition.
* **Any**: Check if any elements in the collection satisfy a condition.
* **OrderBy** / **OrderByDescending**: Sort data in ascending or descending order.
* **Average** / **Max**: Perform aggregation like calculating averages or finding maximum values.
* **First**: Get the first element that satisfies a condition.
* **Select**: Project elements into a new shape or object.
* **Chaining Methods**: Combine multiple LINQ methods to achieve complex queries.

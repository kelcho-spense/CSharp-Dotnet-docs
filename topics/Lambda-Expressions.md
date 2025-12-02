# Lambda Expressions

A lambda expression is a **short, inline anonymous function** you write with the `=>` operator (pronounced “goes to”).

* Syntax for a single expression:

  ```C#
  parameter => expression
  ```
* Syntax for multiple statements:

  ````C#
  (parameters) => { statement1; statement2; … } 
  ````
* Example:

  ````C#
  Func<int,int> square = x => x * x;
  Console.WriteLine(square(5));  // Output: 25
  ````

> Why this matters: lambdas let you define short logic **where it’s used**, rather than creating separate named methods.

---

## Anatomy of a Lambda Expression

| Part             | Description                                                                                                                            |
| ---------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| Input parameters | On the left of `=>`. If only one parameter you can omit parentheses.                                             |
| `=>` operator    | Separates parameters and body.                                                                                                         |
| Body             | Right side: either a single expression (expression lambda) or a block `{ … }` of statements (statement lambda).  |

**Key tip**: Use expression lambdas when you have a simple return value; use statement lambdas when you need multiple lines (e.g., logging + calculation).
Example statement lambda:

````C#
Action<string> greet = name => { var message = $"Hello {name}!"; Console.WriteLine(message); };
greet("World");  // Hello World!

---

## How Lambdas Work with LINQ  
LINQ (Language Integrated Query) enables querying collections (and other data sources) in a fluent, functional style. Lambda expressions are the backbone of LINQ’s “method syntax”.

Here are common operations:

### 1. Filtering (`Where`)  
```C#
List<int> numbers = new List<int>{1,2,3,4,5};
var evens = numbers.Where(n => n % 2 == 0);
// evens: 2, 4
````

* `n => n % 2 == 0` is a predicate lambda (returns `bool`).
* `Where` takes a `Func<T,bool>` delegate under the hood. 

### 2. Projection (`Select`)

```C#
List<Person> people = /* … */;
var names = people.Select(p => p.Name);
// yields sequence of names
```

* `p => p.Name` is a mapping lambda (takes `Person`, returns `string`).
* `Select` takes a `Func<TSource, TResult>`. 

### 3. Sorting, Grouping, etc.

```C#
var sorted = numbers.OrderBy(n => n);
var grouped = people.GroupBy(p => p.Department);
```

* Each uses a lambda to say *how* to order or group.
* Enables concise, expressive queries without loops. 

---

## Why Use Lambdas + LINQ?

* **Conciseness**: Eliminates boilerplate. 
* **Readability**: Logic stays close to data operations.
* **Expressiveness**: Easily chain operations (`Where`, `Select`, `OrderBy`, etc).
* **Functional style**: Encourages thinking in terms of data transformations rather than imperative loops.

---

## Quick “Skills” Example – Employee Scenario

Suppose you have a list of employees:

```C#
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Department { get; set; }
    public decimal Salary { get; set; }
}

List<Employee> employees = /* … */;
```

*Filter employees in “Sales” with salary > 50,000, then select their names:*

```C#
var result = employees
    .Where(emp => emp.Department == "Sales" && emp.Salary > 50000m)
    .Select(emp => emp.Name);
```

Here:

* `emp => emp.Department == "Sales" && emp.Salary > 50000m` → predicate lambda for `Where`.
* `emp => emp.Name` → projection lambda for `Select`.

*If you wanted a statement lambda to log each employee before projection:*

```C#
var result2 = employees
    .Where(emp =>
    {
        Console.WriteLine($"Checking employee {emp.Name}");
        return emp.Salary > 50000m;
    })
    .Select(emp => emp.Name);
```

---

## Best Practices & Tips

* Keep lambdas **short and clear**. If it becomes complex, extract a named method. 
* Use meaningful parameter names (not just `x`, `y`) when dealing with domain objects (e.g., `emp` rather than `e`).
* Be mindful of deferred execution: LINQ queries using lambdas evaluate only when iterated.
* Avoid side‑effects in lambdas if the intent is purely querying/filtering data—maintaining functional style aids readability and maintainability.

---
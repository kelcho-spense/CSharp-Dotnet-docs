# Fluent Validation
### Installation

Using the NuGet package manager console within Visual Studio run the following command:

```Bash
Install-Package FluentValidation
```

Or using the .net core CLI from a terminal window:

```Bash
dotnet add package FluentValidation
```

## Creating your first validator

To define a set of validation rules for a particular object, you will need to create a class that inherits from `AbstractValidator<T>`, where T is the type of class that you wish to validate.

For example, imagine that you have a Customer class:

```c#
public class Customer 
{
  public int Id { get; set; }
  public string Surname { get; set; }
  public string Forename { get; set; }
  public decimal Discount { get; set; }
  public string Address { get; set; }
}
```
You would define a set of validation rules for this class by inheriting from `AbstractValidator<Customer>`:

```C#
using FluentValidation;

public class CustomerValidator : AbstractValidator<Customer> 
{
}
```
The validation rules themselves should be defined in the validator class’s constructor.

To specify a validation rule for a particular property, call the `RuleFor` method, passing a lambda expression that indicates the property that you wish to validate. For example, to ensure that the `Surname` property is not null, the validator class would look like this:

```C#
using FluentValidation;

public class CustomerValidator : AbstractValidator<Customer>
{
  public CustomerValidator()
  {
    RuleFor(customer => customer.Surname).NotNull();
  }
}
```

To run the validator, instantiate the validator object and call the Validate method, passing in the object to validate.

```C#
Customer customer = new Customer();
CustomerValidator validator = new CustomerValidator();

ValidationResult result = validator.Validate(customer);
```

The Validate method returns a ValidationResult object. This contains two properties:

* `IsValid` - a boolean that says whether the validation succeeded.
* `Errors` - a collection of ValidationFailure objects containing details about any validation failures.

The following code would write any validation failures to the console:

```C#
using FluentValidation.Results; 

Customer customer = new Customer();
CustomerValidator validator = new CustomerValidator();

ValidationResult results = validator.Validate(customer);

if(! results.IsValid) 
{
  foreach(var failure in results.Errors)
  {
    Console.WriteLine("Property " + failure.PropertyName + " failed validation. Error was: " + failure.ErrorMessage);
  }
}
```
#### Chaining validators
You can chain multiple validators together for the same property:

```C#
using FluentValidation;

public class CustomerValidator : AbstractValidator<Customer>
{
  public CustomerValidator()
  {
    RuleFor(customer => customer.Surname).NotNull().NotEqual("foo");
  }
}
```
This would ensure that the surname is not null and is not equal to the string ‘foo’.

## Manual Validations – ASP.NET Core Web API

FluentValidation can be used within ASP.NET Core web applications to validate incoming models. There are several approaches for doing this:

* Manual validation
* Automatic validation (using the ASP.NET validation pipeline)
* Automatic validation (using a filter)

With manual validation, you inject the validator into your controller (or api endpoint), invoke the validator and act upon the result. This is the most straightforward approach and also the easiest to see what’s happening.

With automatic validation, FluentValidation is invoked automatically by ASP.NET earlier in the pipeline which allows models to be validated before a controller action is invoked.

## Getting started
The following examples will make use of a `Person` object which is validated using a `PersonValidator`. These classes are defined as follows:

```C#
public class Person 
{
  public int Id { get; set; }
  public string Name { get; set; }
  public string Email { get; set; }
  public int Age { get; set; }
}

public class PersonValidator : AbstractValidator<Person> 
{
  public PersonValidator() 
  {
    RuleFor(x => x.Id).NotNull();
    RuleFor(x => x.Name).Length(0, 10);
    RuleFor(x => x.Email).EmailAddress();
    RuleFor(x => x.Age).InclusiveBetween(18, 60);
  }
}
```

If you’re using MVC, Web Api or Razor Pages you’ll need to register your validator with the Service Provider in the `ConfigureServices` method of your application’s `Startup` class.

```C#
public void ConfigureServices(IServiceCollection services) 
{
    // If you're using MVC or WebApi you'll probably have
    // a call to AddMvc() or AddControllers() already.
    services.AddMvc();
    
    // ... other configuration ...
    
    services.AddScoped<IValidator<Person>, PersonValidator>();
}
```

> Note that you must register each validator as `IValidator<T>` where **T** is the type being validated. So if you have a PersonValidator that inherits from `AbstractValidator<Person>` then you should register it as `IValidator<Person>`

Here we use the `AddValidatorsFromAssemblyContaining` method from the `FluentValidation.DependencyInjectionExtension` package to automatically register all validators in the same assembly as PersonValidator with the service provider.

Now that the validators are registered with the service provider you can start working with either manual validation or automatic validation.

> The auto-registration method used above uses reflection to scan one or more assemblies for validators. An alternative approach would be to use a source generator such as AutoRegisterInject to set up registrations.

### Manual Validation
With the manual validation approach, you’ll inject the validator into your controller (or Razor page) and invoke it against the model.

For example, you might have a controller that looks like this:

```C#
public class PeopleController : Controller 
{
  private IValidator<Person> _validator;
  private IPersonRepository _repository;

  public PeopleController(IValidator<Person> validator, IPersonRepository repository) 
  {
    // Inject our validator and also a DB context for storing our person object.
    _validator = validator;
    _repository = repository;
  }

  public ActionResult Create() 
  {
    return View();
  }

  [HttpPost]
  public async Task<IActionResult> Create(Person person) 
  {
    ValidationResult result = await _validator.ValidateAsync(person);

    if (!result.IsValid) 
    {
      // Copy the validation results into ModelState.
      // ASP.NET uses the ModelState collection to populate 
      // error messages in the View.
      result.AddToModelState(this.ModelState);

      // re-render the view when validation failed.
      return View("Create", person);
    }

    _repository.Save(person); //Save the person to the database, or some other logic

    TempData["notice"] = "Person successfully created";
    return RedirectToAction("Index");
  }
}
```

Because our validator is registered with the Service Provider, it will be injected into our controller via the constructor. We can then make use of the validator inside the `Create` action by invoking it with `ValidateAsync`.

If validation fails, we need to pass the error messages back down to the view so they can be displayed to the end user. We can do this by defining an extension method for FluentValidation’s `ValidationResult` type that copies the error messages into ASP.NET’s `ModelState` dictionary:

```C#
public static class Extensions 
{
  public static void AddToModelState(this ValidationResult result, ModelStateDictionary modelState) 
  {
    foreach (var error in result.Errors) 
    {
      modelState.AddModelError(error.PropertyName, error.ErrorMessage);
    }
  }
}
```
This method is invoked inside the controller action in the example above.

### Automatic Validation
Automatic validation instantiates and invokes a validator before the controller action is executed, meaning the ModelState will already be populated with validation results by the time your controller action is invoked. There are 2 implementations for this approach:

* Using ASP.NET’s validation pipeline (no longer recommended)
* Using an Action Filter (supported by a 3rd party package)

#### Using the ASP.NET Validation Pipeline
The `FluentValidation.AspNetCore` package provides auto-validation for ASP.NET Core MVC projects by plugging into ASP.NET’s validation pipeline.

With automatic validation using the validation pipeline, FluentValidation plugs into ASP.NET’s bult-in validation process that’s part of ASP.NET Core MVC and allows models to be validated before a controller action is invoked (during model-binding). This approach to validation is more seamless but has several downsides:

* **The ASP.NET validation pipeline is not asynchronous**: If your validator contains asynchronous rules then your validator will not be able to run. You will receive an exception at runtime if you attempt to use an asynchronous validator with auto-validation.
* **It is MVC-only**: This approach for auto-validation only works with MVC Controllers and Razor Pages. It does not work with the more modern parts of ASP.NET such as Minimal APIs or Blazor.
* **It is harder to debug**: The ‘magic’ nature of auto-validation makes it hard to debug/troubleshoot if something goes wrong as so much is done behind the scenes.

> We no longer recommend using this approach for new projects but it is still available for legacy implementations.

Instructions for this approach can be found in the `FluentValidation.AspNetCore` package can be found on its project [page here](https://github.com/FluentValidation/FluentValidation.AspNetCore#aspnet-core-integration-for-fluentvalidation).

### Minimal APIs
When using FluentValidation with minimal APIs, you can still register the validators with the service provider, (or you can instantiate them directly if they don’t have dependencies) and invoke them inside your API endpoint.

```C#
var builder = WebApplication.CreateBuilder(args);
var app = builder.Build();

// Register validator with service provider (or use one of the automatic registration methods)
builder.Services.AddScoped<IValidator<Person>, PersonValidator>();

// Also registering a DB access repository for demo purposes
// replace this with whatever you're using in your application.
builder.Services.AddScoped<IPersonRepository, PersonRepository>();

app.MapPost("/person", async (IValidator<Person> validator, IPersonRepository repository, Person person) => 
{
  ValidationResult validationResult = await validator.ValidateAsync(person);

  if (!validationResult.IsValid) 
  {
    return Results.ValidationProblem(validationResult.ToDictionary());
  }

  repository.Save(person);
  return Results.Created($"/{person.Id}", person);
});
```

## Built-in Validators

FluentValidation ships with several built-in validators. The error message for each validator can contain special placeholders that will be filled in when the error message is constructed.

### NotNull Validator
Ensures that the specified property is not null.

Example:

```C#
RuleFor(customer => customer.Surname).NotNull();
```
Example error: ‘Surname’ must not be empty.

String format args:

* `{PropertyName}` – Name of the property being validated
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

### NotEmpty Validator
Ensures that the specified property is not null, an empty string or whitespace (or the default value for value types, e.g., 0 for int). When used on an IEnumerable (such as arrays, collections, lists, etc.), the validator ensures that the IEnumerable is not empty.

Example:

RuleFor(customer => customer.Surname).NotEmpty();
Example error: ‘Surname’ should not be empty. String format args:

* `{PropertyName}` – Name of the property being validated
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property
### NotEqual Validator
Ensures that the value of the specified property is not equal to a particular value (or not equal to the value of another property).

Example:

//Not equal to a particular value
```C#
RuleFor(customer => customer.Surname).NotEqual("Foo");

//Not equal to another property

RuleFor(customer => customer.Surname).NotEqual(customer => customer.Forename);
```
Example error: ‘Surname’ should not be equal to ‘Foo’

String format args:

* `{PropertyName}` – Name of the property being validated
* `{ComparisonValue}` – Value that the property should not equal
* `{ComparisonProperty}` – Name of the property being compared against (if any)
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property
* 
Optionally, a comparer can be provided to ensure a specific type of comparison is performed:

```C#
RuleFor(customer => customer.Surname).NotEqual("Foo", StringComparer.OrdinalIgnoreCase);
```

An ordinal comparison will be used by default. If you wish to do a culture-specific comparison instead, you should pass StringComparer.CurrentCulture as the second parameter.

### Equal Validator
Ensures that the value of the specified property is equal to a particular value (or equal to the value of another property).

Example:

```C#
//Equal to a particular value
RuleFor(customer => customer.Surname).Equal("Foo");

//Equal to another property
RuleFor(customer => customer.Password).Equal(customer => customer.PasswordConfirmation);
```

Example error: ‘Surname’ should be equal to ‘Foo’ String format args:

* `{PropertyName}` – Name of the property being validated
* `{ComparisonValue}` – Value that the property should equal
* `{ComparisonProperty}` – Name of the property being compared against (if any)
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

```C#
RuleFor(customer => customer.Surname).Equal("Foo", StringComparer.OrdinalIgnoreCase);
```

An ordinal comparison will be used by default. If you wish to do a culture-specific comparison instead, you should pass StringComparer.CurrentCulture as the second parameter.

### Length Validator
Ensures that the length of a particular string property is within the specified range. However, it doesn’t ensure that the string property isn’t null.

Example:

```C#
RuleFor(customer => customer.Surname).Length(1, 250); //must be between 1 and 250 chars (inclusive)
```

Example error: ‘Surname’ must be between 1 and 250 characters. You entered 251 characters.

Note: Only valid on string properties.

String format args:

* `{PropertyName}` – Name of the property being validated
* `{MinLength}` – Minimum length
* `{MaxLength}` – Maximum length
* `{TotalLength}` – Number of characters entered
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

### MaxLength Validator
Ensures that the length of a particular string property is no longer than the specified value.

Example:

```C#
RuleFor(customer => customer.Surname).MaximumLength(250); //must be 250 chars or fewer
```

Example error: The length of ‘Surname’ must be 250 characters or fewer. You entered 251 characters.

Note: Only valid on string properties.

String format args:

* `{PropertyName}` – Name of the property being validated
* `{MaxLength}` – Maximum length
* `{TotalLength}` – Number of characters entered
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

### MinLength Validator
Ensures that the length of a particular string property is longer than the specified value.

Example:

```C#
RuleFor(customer => customer.Surname).MinimumLength(10); //must be 10 chars or more
```

Example error: The length of ‘Surname’ must be at least 10 characters. You entered 5 characters.

Note: Only valid on string properties.

String format args:

* `{PropertyName}` – Name of the property being validated
* `{MinLength}` – Minimum length
* `{TotalLength}` – Number of characters entered
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

### Less Than Validator
Ensures that the value of the specified property is less than a particular value (or less than the value of another property).

Example:

```C#
//Less than a particular value
RuleFor(customer => customer.CreditLimit).LessThan(100);

//Less than another property
RuleFor(customer => customer.CreditLimit).LessThan(customer => customer.MaxCreditLimit);
```

Example error: ‘Credit Limit’ must be less than 100.

Notes: Only valid on types that implement IComparable<T>

String format args:

* `{PropertyName}` – Name of the property being validated
* `{ComparisonValue}` – Value to which the property was compared
* `{ComparisonProperty}` – Name of the property being compared against (if any)
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

### Less Than Or Equal Validator
Ensures that the value of the specified property is less than or equal to a particular value (or less than or equal to the value of another property).

Example:

```C#
//Less than a particular value
RuleFor(customer => customer.CreditLimit).LessThanOrEqualTo(100);

//Less than another property
RuleFor(customer => customer.CreditLimit).LessThanOrEqualTo(customer => customer.MaxCreditLimit);
```

Example error: ‘Credit Limit’ must be less than or equal to 100. Notes: Only valid on types that implement IComparable<T>

String format args:

* `{PropertyName}` – Name of the property being validated
* `{ComparisonValue}` – Value to which the property was compared
* `{ComparisonProperty}` – Name of the property being compared against (if any)
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

### Greater Than Validator
Ensures that the value of the specified property is greater than a particular value (or greater than the value of another property).

Example:

```C#
//Greater than a particular value
RuleFor(customer => customer.CreditLimit).GreaterThan(0);

//Greater than another property
RuleFor(customer => customer.CreditLimit).GreaterThan(customer => customer.MinimumCreditLimit);
```

Example error: ‘Credit Limit’ must be greater than 0. Notes: Only valid on types that implement IComparable<T>

String format args:

* `{PropertyName}` – Name of the property being validated
* `{ComparisonValue}` – Value to which the property was compared
* `{ComparisonProperty}` – Name of the property being compared against (if any)
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

### Greater Than Or Equal Validator
Ensures that the value of the specified property is greater than or equal to a particular value (or greater than or equal to the value of another property).

Example:

```C#
//Greater than a particular value
RuleFor(customer => customer.CreditLimit).GreaterThanOrEqualTo(1);

//Greater than another property
RuleFor(customer => customer.CreditLimit).GreaterThanOrEqualTo(customer => customer.MinimumCreditLimit);
```

Example error: ‘Credit Limit’ must be greater than or equal to 1. Notes: Only valid on types that implement IComparable<T>

String format args:

* `{PropertyName}` – Name of the property being validated
* `{ComparisonValue}` – Value to which the property was compared
* `{ComparisonProperty}` – Name of the property being compared against (if any)
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

### Predicate Validator
(Also known as `Must`)

Passes the value of the specified property into a delegate that can perform custom validation logic on the value.

Example:

```C#
RuleFor(customer => customer.Surname).Must(surname => surname == "Foo");
```

Example error: The specified condition was not met for ‘Surname’

String format args:

* `{PropertyName}` – Name of the property being validated
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

Note that there is an additional overload for Must that also accepts an instance of the parent object being validated. This can be useful if you want to compare the current property with another property from inside the predicate:

```C#
RuleFor(customer => customer.Surname).Must((customer, surname) => surname != customer.Forename)
```

Note that in this particular example, it would be better to use the cross-property version of NotEqual.

### Regular Expression Validator
Ensures that the value of the specified property matches the given regular expression.

Example:
````C#
RuleFor(customer => customer.Surname).Matches("some regex here");
````

Example error: ‘Surname’ is not in the correct format. String format args:

* `{PropertyName}` – Name of the property being validated
* `{PropertyValue}` – Current value of the property
* `{RegularExpression}` – Regular expression that was not matched
* `{PropertyPath}` - The full path of the property

### Email Validator
Ensures that the value of the specified property is a valid email address format.

Example:

```C#
RuleFor(customer => customer.Email).EmailAddress();
```

Example error: ‘Email’ is not a valid email address.

String format args:

* `{PropertyName}` – Name of the property being validated
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

The email address validator can work in 2 modes. The default mode just performs a simple check that the string contains an “@” sign which is not at the beginning or the end of the string. This is an intentionally naive check to match the behaviour of ASP.NET Core’s EmailAddressAttribute, which performs the same check. For the reasoning behind this, see this post:

From the comments:

“The check is intentionally naive because doing something infallible is very hard. The email really should be validated in some other way, such as through an email confirmation flow where an email is actually sent. The validation attribute is designed only to catch egregiously wrong values such as for a U.I.”
Alternatively, you can use the old email validation behaviour that uses a regular expression consistent with the .NET 4.x version of the ASP.NET `EmailAddressAttribute`. You can use this behaviour in FluentValidation by calling RuleFor(x => x.Email).EmailAddress(EmailValidationMode.Net4xRegex). Note that this approach is deprecated and will generate a warning as regex-based email validation is not recommended.

**Note:**

> In FluentValidation 9, the ASP.NET Core-compatible “simple” check is the default mode. In FluentValidation 8.x (and older), the Regex mode is the default.

### Credit Card Validator
Checks whether a string property could be a valid credit card number.

Example:

```C#
RuleFor(x => x.CreditCard).CreditCard();
```

Example error: ‘Credit Card’ is not a valid credit card number.

String format args:

* `{PropertyName}` – Name of the property being validated
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

### Enum Validator
Checks whether a numeric value is valid to be in that enum. This is used to prevent numeric values from being cast to an enum type when the resulting value would be invalid. For example, the following is possible:

```C#
public enum ErrorLevel
{
Error = 1,
Warning = 2,
Notice = 3
}

public class Model
{
public ErrorLevel ErrorLevel { get; set; }
}

var model = new Model();
model.ErrorLevel = (ErrorLevel)4;
```

The compiler will allow this, but a value of 4 is technically not valid for this enum. The Enum validator can prevent this from happening.

```C#
RuleFor(x => x.ErrorLevel).IsInEnum();
```

Example error: ‘Error Level’ has a range of values which does not include ‘4’.

String format args:

{PropertyName} – Name of the property being validated
{PropertyValue} – Current value of the property
{PropertyPath} - The full path of the property
### Enum Name Validator
Checks whether a string is a valid enum name.

Example:
```C#
// For a case sensitive comparison
RuleFor(x => x.ErrorLevelName).IsEnumName(typeof(ErrorLevel));

// For a case-insensitive comparison
RuleFor(x => x.ErrorLevelName).IsEnumName(typeof(ErrorLevel), caseSensitive: false);
```
Example error: ‘Error Level’ has a range of values which does not include ‘Foo’.

String format args:

* `{PropertyName}` – Name of the property being validated
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

### Empty Validator
Opposite of the NotEmpty validator. Checks if a property value is null, or is the default value for the type. When used on an IEnumerable (such as arrays, collections, lists, etc.), the validator ensures that the IEnumerable is empty.

Example:
```C#
RuleFor(x => x.Surname).Empty();
```
Example error: ‘Surname’ must be empty.

String format args:

* `{PropertyName}` – Name of the property being validated
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

### Null Validator
Opposite of the NotNull validator. Checks if a property value is null.

Example:
```C#
RuleFor(x => x.Surname).Null();
```
Example error: ‘Surname’ must be empty.

String format args:

* `{PropertyName}` – Name of the property being validated
* `{PropertyValue}` – Current value of the property
* `{PropertyPath}` - The full path of the property

### ExclusiveBetween Validator
Checks whether the property value is in a range between the two specified numbers (exclusive).

Example:
```C#
RuleFor(x => x.Id).ExclusiveBetween(1,10);
```
Example error: ‘Id’ must be between 1 and 10 (exclusive). You entered 1.

String format args:

* `{PropertyName}` – Name of the property being validated
* `{PropertyValue}` – Current value of the property
* `{From}` – Lower bound of the range
* `{To}` – Upper bound of the range
* `{PropertyPath}` - The full path of the property

### InclusiveBetween Validator
Checks whether the property value is in a range between the two specified numbers (inclusive).

Example:
```C#
RuleFor(x => x.Id).InclusiveBetween(1,10);
```

Example error: ‘Id’ must be between 1 and 10. You entered 0.

String format args:

* `{PropertyName}` – Name of the property being validated
* `{PropertyValue}` – Current value of the property
* `{From}` – Lower bound of the range
* `{To}` – Upper bound of the range
* `{PropertyPath}` - The full path of the property

### PrecisionScale Validator
Checks whether a decimal value has the specified precision and scale.

Example:
```C#
RuleFor(x => x.Amount).PrecisionScale(4, 2, false);
```

Example error: ‘Amount’ must not be more than 4 digits in total, with allowance for 2 decimals. 5 digits and 3 decimals were found.

String format args:

* `{PropertyName}` – Name of the property being validated
* `{PropertyValue}` – Current value of the property
* `{ExpectedPrecision}` – Expected precision
* `{ExpectedScale}` – Expected scale
* `{Digits}` – Total number of digits in the property value
* `{ActualScale}` – Actual scale of the property value
* `{PropertyPath}` - The full path of the property

> Note that the 3rd parameter of this method is `ignoreTrailingZeros`. When set to `true`, trailing zeros after the decimal point will not count towards the expected number of decimal places.

Example:

* When `ignoreTrailingZeros` is `false` then the decimal `123.4500` will be considered to have a precision of 7 and scale of 4
* When `ignoreTrailingZeros` is `true` then the decimal `123.4500` will be considered to have a precision of 5 and scale of 2.

Please also note that this method implies certain range of values that will be accepted. For example in case of `.PrecisionScale(3, 1)`, the method will accept values between `-99.9` and `99.9`, inclusive. Which means that integer part is always controlled to contain at most `3 - 1` digits, independently from `ignoreTrailingZeros` parameter.

> Note that prior to FluentValidation 11.4, this method was called `ScalePrecision` instead and had its parameters reversed.

## Custom Validators
There are several ways to create a custom, reusable validator. The recommended way is to make use of the Predicate Validator to write a custom validation function, but you can also use the Custom method to take full control of the validation process.

For these examples, we’ll imagine a scenario where you want to create a reusable validator that will ensure a List object contains fewer than 10 items.

### Predicate Validator
The simplest way to implement a custom validator is by using the Must method, which internally uses the PredicateValidator.

Imagine we have the following class:

```C#
public class Person {
  public IList<Pet> Pets {get;set;} = new List<Pet>();
}
```

To ensure our list property contains fewer than 10 items, we could do this:

```C#
public class PersonValidator : AbstractValidator<Person> {
  public PersonValidator() {
    RuleFor(x => x.Pets).Must(list => list.Count < 10)
      .WithMessage("The list must contain fewer than 10 items");
  }
}
```
To make this logic reusable, we can wrap it an extension method that acts upon any List<T> type.

```C#
public static class MyCustomValidators {
  public static IRuleBuilderOptions<T, IList<TElement>> ListMustContainFewerThan<T, TElement>(this IRuleBuilder<T, IList<TElement>> ruleBuilder, int num) {
	return ruleBuilder.Must(list => list.Count < num).WithMessage("The list contains too many items");
  }
}
```

Here we create an extension method on `IRuleBuilder<T,TProperty>`, and we use a generic type constraint to ensure this method only appears in intellisense for List types. Inside the method, we call the Must method in the same way as before but this time we call it on the passed-in `RuleBuilder` instance. We also pass in the number of items for comparison as a parameter. Our rule definition can now be rewritten to use this method:

```C#
RuleFor(x => x.Pets).ListMustContainFewerThan(10);
```

### Custom message placeholders
We can extend the above example to include a more useful error message. At the moment, our custom validator always returns the message “The list contains too many items” if validation fails. Instead, let’s change the message so it returns “’Pets’ must contain fewer than 10 items.” This can be done by using custom message placeholders. FluentValidation supports several message placeholders by default including `{PropertyName}` and `{PropertyValue}`

We need to modify our extension method slightly to use a different overload of the `Must` method, one that accepts a `ValidationContext<T>` instance. This context provides additional information and methods we can use when performing validation:

```C#
public static IRuleBuilderOptions<T, IList<TElement>> ListMustContainFewerThan<T, TElement>(this IRuleBuilder<T, IList<TElement>> ruleBuilder, int num) {

  return ruleBuilder.Must((rootObject, list, context) => {
    context.MessageFormatter.AppendArgument("MaxElements", num);
    return list.Count < num;
  })
  .WithMessage("{PropertyName} must contain fewer than {MaxElements} items.");
}
```

Note that the overload of Must that we’re using now accepts 3 parameters: the root (parent) object, the property value itself, and the context. We use the context to add a custom message replacement value of `MaxElements` and set its value to the number passed to the method. We can now use this placeholder as `{MaxElements}` within the call to `WithMessage`.

The resulting message will now be `'Pets' must contain fewer than 10 items`. We could even extend this further to include the number of elements that the list contains like this:

```C#
public static IRuleBuilderOptions<T, IList<TElement>> ListMustContainFewerThan<T, TElement>(this IRuleBuilder<T, IList<TElement>> ruleBuilder, int num) {

  return ruleBuilder.Must((rootObject, list, context) => {
    context.MessageFormatter
      .AppendArgument("MaxElements", num)
      .AppendArgument("TotalElements", list.Count);

    return list.Count < num;
  })
  .WithMessage("{PropertyName} must contain fewer than {MaxElements} items. The list contains {TotalElements} element");
}
```

### Writing a Custom Validator
If you need more control of the validation process than is available with `Must`, you can write a custom rule using the `Custom` method. This method allows you to manually create the `ValidationFailure` instance associated with the validation error. Usually, the framework does this for you, so it is more verbose than using `Must`.

```C#
public class PersonValidator : AbstractValidator<Person> {
  public PersonValidator() {
   RuleFor(x => x.Pets).Custom((list, context) => {
     if(list.Count > 10) {
       context.AddFailure("The list must contain 10 items or fewer");
     }
   });
  }
}
```

The advantage of this approach is that it allows you to return multiple errors for the same rule (by calling the `context.AddFailure` method multiple times). In the above example, the property name in the generated error will be inferred as “Pets”, although this could be overridden by calling a different overload of `AddFailure`:

```C#
context.AddFailure("SomeOtherProperty", "The list must contain 10 items or fewer");
// Or you can instantiate the ValidationFailure directly:
context.AddFailure(new ValidationFailure("SomeOtherProperty", "The list must contain 10 items or fewer");
```

As before, this could be wrapped in an extension method to simplify the consuming code.

```C#
public static IRuleBuilderOptionsConditions<T, IList<TElement>> ListMustContainFewerThan<T, TElement>(this IRuleBuilder<T, IList<TElement>> ruleBuilder, int num) {

  return ruleBuilder.Custom((list, context) => {
     if(list.Count > 10) {
       context.AddFailure("The list must contain 10 items or fewer");
     }
   });
}
```
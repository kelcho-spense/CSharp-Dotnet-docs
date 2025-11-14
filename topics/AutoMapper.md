# AutoMapper
![image_185.png](image_185.png)

## Why do we need AutoMapper in C#?
Let’s understand why we need Automapper in C# with an example. Let’s say we have the following two classes: Employee and EmployeeDTO. First, create a class file named Employee.cs, and then copy and paste the following code. This is a very simple class having 4 properties.

```C#
namespace AutoMapperDemo
{
    public class Employee
    {
        public string Name { get; set; }
        public int Salary { get; set; }
        public string Address { get; set; }
        public string Department { get; set; }
    }
}
```

Next, create another class file with the name EmployeeDTO.cs and then copy and paste the following code into it. This class is identical to the Employee class, i.e., having 4 properties.

```C#
namespace AutoMapperDemo
{
    public class EmployeeDTO
    {
        public string Name { get; set; }
        public int Salary { get; set; }
        public string Address { get; set; }
        public string Department { get; set; }
    }
}
```

> Now, what is our business requirement is to copy the data or transfer the data from the Employee object to the EmployeeDTO object

In the traditional approach (without using Automapper), first, we need to create and populate the Employee object, as shown in the image below.

![image_186.png](image_186.png)

Once you have the employee object, you need to create the EmployeeDTO object and copy the data from the Employee object to the EmployeeDTO object, as shown in the image below.

![image_187.png](image_187.png)

### Mapping Object in Traditional Approach in C#

Let us understand how the Object is Mapped to another Object in C# using the Traditional Approach. For a better understanding, please have a look at the following example. First, we create an instance of the Employee object and populate the four properties with the required data. Then, we create an instance of the EmployeeDTO class and populate the EmployeeDTO properties with the values from the Employee object. Finally, we displayed the values of the EmployeeDTO object.

```C#
using System;
namespace AutoMapperDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            //Create and Populate Employee Object
            Employee emp = new Employee
            {
                Name = "James",
                Salary = 20000,
                Address = "London",
                Department = "IT"
            };

            //Mapping Employee Object to EmployeeDTO Object
            EmployeeDTO empDTO = new EmployeeDTO
            {
                Name = emp.Name,
                Salary = emp.Salary,
                Address = emp.Address,
                Department = emp.Department
            };

            Console.WriteLine("Name:" + empDTO.Name + ", Salary:" + empDTO.Salary + ", Address:" + empDTO.Address + ", Department:" + empDTO.Department);
            Console.ReadLine();
        }
    }
}
```
If you run the application, you will get the output as expected. But tomorrow, what will you do if the data, i.e., the properties in the Employee and EmployeeDTO classes, are increased? Then, you must write the data moving code for each property from the source class to the destination class. That means the code mapping is repeated between the source and the destination. Again, if the same mapping is at different places, then you need to make the changes at different places, which is time-consuming and error-prone.

In Real-Time Projects, we often need to map the objects between the UI Layer or Presentation Layer and Business Logic layers. Mapping the objects between them is very hectic using the abovementioned traditional approach. So, is there any simplest solution by which we can map two objects? Yes, there is, and the solution is **AutoMapper**

## What is AutoMapper in C#
AutoMapper is a popular open-source library in C# that simplifies mapping data between different classes or objects. It helps eliminate repetitive and error-prone code when copying data from one object to another. AutoMapper is especially useful in scenarios like mapping database entities to DTOs (Data Transfer Objects) or ViewModel objects.

The AutoMapper in C# is a mapper between two objects. That is, AutoMapper is an Object-Object Mapper. It maps the properties of two different objects by transforming the input object of one type to the output object of another.

### Installing AutoMapper Library in Your Project
AutoMapper is an open-source library present in [GitHub](https://github.com/AutoMapper). To install this library, open the Package Manager Console window. To Open the Package Manager Console window, select Tools => NuGet Package Manager => Package Manager Console from the context menu, as shown in the image below.


![image_188.png](image_188.png)

Once you open the Package Manager Console window, type the command Install-Package AutoMapper and press the enter key to install the AutoMapper Library in your project, as shown in the image below.

![image_189.png](image_189.png)

## AutoMapper Tutorial Project

A comprehensive C# project demonstrating AutoMapper for object-to-object mapping, including Entity models, DTOs (Data Transfer Objects), and ViewModels with practical examples.

## Overview

This project demonstrates the proper use of AutoMapper in a .NET 8 console application. It showcases mapping between three different layers of data representation:

1. **Entity Layer** (`Employee.cs`) - Database model
2. **DTO Layer** (`EmployeeDto.cs`) - Data transfer object for API/service layer
3. **View Layer** (`EmployeeViewModel.cs`) - UI representation model

The project uses AutoMapper 15.1.0 with proper configuration and licensing.

---

## What is AutoMapper?

AutoMapper is a powerful .NET library that automatically maps objects from one type to another. It reduces boilerplate code and makes your application more maintainable.

### Key Benefits:

- **Convention-Based Mapping**: Automatically maps properties with matching names
- **Flattening**: Combines nested properties into a single flat object
- **Custom Transformations**: Supports complex mapping logic via `ForMember()`
- **Type Safety**: Uses generic types for compile-time safety
- **Lazy Loaded**: Only executes mappings when needed
- **Validation**: Can validate mapping configurations at startup

### Why Use AutoMapper?

- Reduces repetitive mapping code
- Improves code maintainability
- Separates concerns between layers
- Makes unit testing easier
- Supports complex transformation scenarios

---

## Project Structure

```plain text
AutomapperTutorial/
├── Models/
│   └── Employee.cs              # Entity model (database representation)
├── DTOs/
│   └── EmployeeDto.cs           # Data Transfer Object
├── View/
│   └── EmployeeViewModel.cs      # ViewModel for UI
├── EmployeeProfile.cs            # AutoMapper configuration/profile
├── Program.cs                    # Application entry point
├── Properties/
│   └── launchSettings.json       # Launch settings
├── appsettings.json              # Application configuration
├── appsettings.Development.json  # Development-specific settings
├── AutomapperTutorial.csproj    # Project file
└── README.md                     # This file
```

---

## Getting Started

### Prerequisites

- .NET 8 SDK or later
- Visual Studio, Visual Studio Code, or any .NET IDE

### Project creation

1. **Create a new project ASP.NET Core Empty**
![image_190.png](image_190.png)

2. **Create below folder structure**
![image_191.png](image_191.png)
 
### Running the Project using CLI 

1. **Build the project**
   ```bash
   dotnet build
   ```

2. **Run the application**
   ```bash
   dotnet run --project AutomapperTutorial.csproj
   ```

---

## Architecture & Mapping Patterns

### Layered Architecture

This project follows a **3-layer mapping pattern**:

#### Layer 1: Entity Model
The **Entity** represents data stored in your database.

```c#
// Employee.cs - Database representation
public class Employee
{
    public int Id { get; set; }
    public string? FirstName { get; set; }
    public string? LastName { get; set; }
    public DateTime DateOfBirth { get; set; }
    public string? Department { get; set; }
}
```

**Characteristics**:
- Directly maps to database schema
- May contain sensitive information
- Not suitable for external APIs
- Can have navigation properties for related entities

#### Layer 2: Data Transfer Object (DTO)
The **DTO** transfers data between services, combining and transforming data as needed.

```c#
// EmployeeDto.cs - Service layer representation
public class EmployeeDto
{
    public int Id { get; set; }
    public string? FullName { get; set; }        // Flattened (FirstName + LastName)
    public string? Department { get; set; }
    public string? DateOfBirthFormatted { get; set; }  // Formatted date
}
```

**Characteristics**:
- Combines related fields (flattening)
- Formats data appropriately
- Controls what data is exposed
- Enables API versioning

#### Layer 3: ViewModel
The **ViewModel** provides UI-specific representation, hiding unnecessary details.

```c#
// EmployeeViewModel.cs - UI representation
public class EmployeeViewModel
{
    public string? FullName { get; set; }
    public string? Department { get; set; }
    public string? DateOfBirthFormatted { get; set; }
}
```

**Characteristics**:
- Contains only UI-needed properties
- Already formatted for display
- Lightweight and focused
- No unnecessary identifiers

### Mapping Flow

```
Employee (Entity)
     ↓
  [Mapper using EmployeeProfile]
     ↓
EmployeeDto (DTO)
     ↓
  [Mapper using EmployeeProfile]
     ↓
EmployeeViewModel (ViewModel)
```

---

## Key Components

### 1. **`EmployeeProfile.cs`** - Mapping Configuration

Employee AutoMapper Profile - Defines all mapping configurations for employee-related classes.

 **This profile centralizes mapping logic between:**
 1. Employee (Entity) → EmployeeDto (DTO): Data transformation with flattening and formatting
 2. EmployeeDto (DTO) → EmployeeViewModel (ViewModel): UI-specific data projection

 **AutoMapper Concepts Used:**
 - CreateMap: Defines source-to-destination type mapping
 - ForMember: Customizes individual property mapping
 - MapFrom: Specifies custom transformation logic
 - Flattening: Combining multiple source properties into one destination property
 - Value Transformation: Converting data types and formats

 **Profile Best Practices:**
 - Keep profiles organized by domain (Employee, Order, etc.)
 - Register profiles in MapperConfiguration at application startup
 - Use meaningful names for clarity
 - Document complex mapping logic

```c#
using AutoMapper;
using AutomapperTutorial.DTOs;
using AutomapperTutorial.Models;
using AutomapperTutorial.View;

public class EmployeeProfile : Profile
{
    public EmployeeProfile()
    {
        // ============================================================
        // MAPPING 1: Employee (Entity) → EmployeeDto (DTO)
        // ============================================================
        // Purpose: Transform database entity into API/service layer DTO
        // Features:
        //   - Flattens FirstName + LastName into single FullName property
        //   - Formats DateTime to string in ISO 8601 format
        //   - Simplifies the data structure for external consumption
        // ============================================================
        CreateMap<Employee, EmployeeDto>()
            // Flattening Example: Combine two source properties into one
            // Source: Employee.FirstName (string) + Employee.LastName (string)
            // Destination: EmployeeDto.FullName (string)
            // Logic: Concatenate with space separator
            .ForMember(
                dest => dest.FullName,
                opt => opt.MapFrom(src => $"{src.FirstName} {src.LastName}"))

            // Value Transformation Example: Format DateTime to string
            // Source: Employee.DateOfBirth (DateTime)
            // Destination: EmployeeDto.DateOfBirthFormatted (string)
            // Logic: Convert to ISO 8601 format (yyyy-MM-dd)
            .ForMember(
                dest => dest.DateOfBirthFormatted,
                opt => opt.MapFrom(src => src.DateOfBirth.ToString("yyyy-MM-dd")));

        // ============================================================
        // MAPPING 2: EmployeeDto (DTO) → EmployeeViewModel (ViewModel)
        // ============================================================
        // Purpose: Project DTO data into UI-specific ViewModel
        // Features:
        //   - Excludes non-UI properties (like Id)
        //   - Passes already-formatted data through
        //   - Optimizes for presentation layer consumption
        // ============================================================
        CreateMap<EmployeeDto, EmployeeViewModel>()
            // Direct property mapping - Uses convention-based mapping
            // These ForMember declarations are optional (properties match by name)
            // but included here for documentation and explicit control
            .ForMember(
                dest => dest.FullName,
                opt => opt.MapFrom(src => src.FullName))

            .ForMember(
                dest => dest.Department,
                opt => opt.MapFrom(src => src.Department))

            .ForMember(
                dest => dest.DateOfBirthFormatted,
                opt => opt.MapFrom(src => src.DateOfBirthFormatted));
    }
}
```

**Key Features**:
- **Inheritance from Profile**: Organizes related mappings
- **CreateMap<TSource, TDestination>()**: Defines mapping rules
- **ForMember()**: Customizes individual property mapping
- **MapFrom()**: Specifies source for destination property

### 2. **Program.cs** - Application Entry Point & Configuration

Initializes AutoMapper with license and runs the demonstration:

```c#
using AutoMapper;
using AutomapperTutorial.DTOs;
using AutomapperTutorial.Models;
using AutomapperTutorial.View;

class Program
{
    
    /// Static IMapper instance. Should be created once per AppDomain.
    /// In production, this would typically be injected via dependency injection.
   
    private static IMapper? _mapper;

    
    /// Application entry point. Demonstrates complete AutoMapper workflow.
    
    /// Steps:
    /// 1. Initialize logging factory
    /// 2. Configure AutoMapper with profiles and license
    /// 3. Create mapper instance
    /// 4. Create sample entity
    /// 5. Map through layers (Entity → DTO → ViewModel)
    /// 6. Display results
   
    static void Main(string[] args)
    {
        // ============================================================
        // STEP 1: Initialize AutoMapper Configuration
        // ============================================================
        // Create logger factory for diagnostics and error reporting
        // In production, this would be injected from DI container
        var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole());

        // Create mapper configuration expression
        var configExpression = new MapperConfigurationExpression();

        // ============================================================
        // STEP 2: Set AutoMapper License (Required for AutoMapper 15.0+)
        // ============================================================
        // IMPORTANT: For production deployment, use one of these approaches:
        // 
        // Option A: Environment Variable (Recommended)
        //   var licenseKey = Environment.GetEnvironmentVariable("AUTOMAPPER_LICENSE_KEY");
        //   configExpression.LicenseKey = licenseKey;
        //
        // Option B: Configuration File
        //   var licenseKey = configuration["AutoMapper:LicenseKey"];
        //   configExpression.LicenseKey = licenseKey;
        //
        // Option C: Azure Key Vault (Production)
        //   var licenseKey = await keyVaultClient.GetSecretAsync("automapper-license");
        //   configExpression.LicenseKey = licenseKey.Value;
        //
        // Current approach: Direct string (development/demo only)
        configExpression.LicenseKey = "eyJhbGciOiJSUzI1NiIsImtpZCI6Ikx1Y2t5UGVubnlTb2Z0d2FyZUxpY2Vuc2VLZXkvYmJiMTNhY2I1OTkwNGQ4OWI0Y2IxYzg1ZjA4OGNjZjkiLCJ0eXAiOiJKV1QifQ.eyJpc3MiOiJodHRwczovL2x1Y2t5cGVubnlzb2Z0d2FyZS5jb20iLCJhdWQiOiJMdWNreVBlbm55U29mdHdhcmUiLCJleHAiOiIxNzk0MzU1MjAwIiwiaWF0IjoiMTc2Mjg2NzE4NCIsImFjY291bnRfaWQiOiIwMTlhNzMxMjAwZGE3YmY3ODhhNjVkM2NjNDdjNDBmYiIsImN1c3RvbWVyX2lkIjoiY3RtXzAxazlzaDUxMHZ0MjVjZTgxcDB0djIyOHl3Iiwic3ViX2lkIjoiLSIsImVkaXRpb24iOiIwIiwidHlwZSI6IjIifQ.P8lFhXbHdB94O8QLFnwseIHsvcpP3xR7yEnJXM7z-Oi-jEO_kwVM5d5uB0vXrP6uGa91nPz6Bx7uVdnh7xQp0lXyFHXWoXRNAfwQn27CnxCeQz5lhvDOJiqwOd1XlWysFp2ZaotoEuF88R4kMC-WP1IPKJr7Pt2gaJURYOysav3xxG2rfevX_a9XNXz2uZx5RCivu0Q5IDPKjguk8xYMTNsUFzri_twv2NID5fWDkyA9lqlBg2bi33xxiZw9YnLXUbqxD7FP_7QtxrawvrR2J2GFg8CaIod3KRahnMzFiFS0co6N5vBB1KyPejUhhp42bKV8jcPx4bcOE2GNxY0CRQ";  // Get your license at https://luckypennysoftware.com

        // ============================================================
        // STEP 3: Register Mapping Profiles
        // ============================================================
        // AddProfile registers the EmployeeProfile which contains
        // all mapping rules for Employee, EmployeeDto, and EmployeeViewModel
        configExpression.AddProfile<EmployeeProfile>();

        // ============================================================
        // STEP 4: Create MapperConfiguration and Mapper
        // ============================================================
        // Note: AutoMapper 15.0+ requires ILoggerFactory as second parameter
        // This is a breaking change from earlier versions
        var config = new MapperConfiguration(configExpression, loggerFactory);
        _mapper = config.CreateMapper();

        Console.WriteLine("=== AutoMapper Tutorial ===\n");

        // ============================================================
        // STEP 5: Create Sample Entity (Database Model)
        // ============================================================
        // Create an Employee entity representing database data
        Employee employee = new Employee
        {
            Id = 1,
            FirstName = "John",
            LastName = "Doe",
            DateOfBirth = new DateTime(1985, 5, 20),
            Department = "IT"
        };

        Console.WriteLine("1. Source Entity (Employee):");
        Console.WriteLine($"   FirstName: {employee.FirstName}");
        Console.WriteLine($"   LastName: {employee.LastName}");
        Console.WriteLine($"   DateOfBirth: {employee.DateOfBirth}");
        Console.WriteLine($"   Department: {employee.Department}\n");

        // ============================================================
        // STEP 6a: Map Entity → DTO
        // ============================================================
        // AutoMapper transforms:
        // - FirstName + LastName → FullName (flattening)
        // - DateOfBirth → DateOfBirthFormatted (value transformation)
        EmployeeDto employeeDto = _mapper.Map<EmployeeDto>(employee);

        Console.WriteLine("2. Mapped to DTO (EmployeeDto):");
        Console.WriteLine($"   ID: {employeeDto.Id}");
        Console.WriteLine($"   Full Name: {employeeDto.FullName} (flattened from FirstName + LastName)");
        Console.WriteLine($"   Department: {employeeDto.Department}");
        Console.WriteLine($"   Date of Birth: {employeeDto.DateOfBirthFormatted} (formatted as yyyy-MM-dd)\n");

        // ============================================================
        // STEP 6b: Map DTO → ViewModel
        // ============================================================
        // AutoMapper projects DTO data to UI-specific ViewModel
        // Excludes unnecessary properties like Id
        EmployeeViewModel employeeViewModel = _mapper.Map<EmployeeViewModel>(employeeDto);

        Console.WriteLine("3. Mapped to ViewModel (EmployeeViewModel):");
        Console.WriteLine($"   Full Name: {employeeViewModel.FullName}");
        Console.WriteLine($"   Department: {employeeViewModel.Department}");
        Console.WriteLine($"   Date of Birth: {employeeViewModel.DateOfBirthFormatted}\n");

        Console.WriteLine("=== Mapping Complete ===");
        Console.WriteLine("\nKey Takeaways:");
        Console.WriteLine("• AutoMapper automates object transformation");
        Console.WriteLine("• Flattening combines multiple properties into one");
        Console.WriteLine("• Value transformation converts data types and formats");
        Console.WriteLine("• Profiles organize mapping logic by domain");
        Console.WriteLine("• License key required for AutoMapper 15.0+ production use");
    }
}

```

**Important Notes**:
- `MapperConfiguration` requires `ILoggerFactory` parameter (AutoMapper 15.0+)
- Must be created once per AppDomain
- Should be initialized during application startup
- License key is required for production environments

### 3. **EmployeeViewModel.cs** - Represents the UI-specific data model.

```c#
namespace AutomapperTutorial.View
{
    
    /// Employee ViewModel - Represents the UI-specific data model.
    
    /// This class is designed specifically for display in the user interface.
    /// It contains only the properties needed for UI rendering and excludes
    /// unnecessary information like database IDs.
    
    /// Source: EmployeeDto (Data Transfer Object)
    
    /// Display Properties:
    /// - FullName: Employee's complete name (already formatted from DTO)
    /// - Department: Employee's department assignment
    /// - DateOfBirthFormatted: Employee's date of birth in human-readable format
    
    /// Note: The Id property is intentionally excluded as it's not needed for UI display.
   
    public class EmployeeViewModel
    {
        
        /// Employee's full name for display.
        /// Mapped directly from EmployeeDto.FullName (already formatted).
        /// Example: "John Doe"
       
        public string? FullName { get; set; }

        
        /// Employee's department for display.
        /// Mapped directly from EmployeeDto.Department.
        /// Example: "IT", "HR", "Finance"
       
        public string? Department { get; set; }

        
        /// Employee's date of birth in human-readable format.
        /// Mapped directly from EmployeeDto.DateOfBirthFormatted.
        /// Format: "yyyy-MM-dd"
        /// Example: "1985-05-20"
       
        public string? DateOfBirthFormatted { get; set; }
    }
}
```

### 4. **EmployeeDto.cs** - Represents the data transfer layer.

```C#
namespace AutomapperTutorial.DTOs
{
    
    /// Employee Data Transfer Object (DTO) - Represents the data transfer layer.

    /// This class is used for transferring employee data between services and APIs.
    /// It combines and transforms data from the Employee entity:
    /// - FirstName + LastName are combined into FullName (flattening)
    /// - DateOfBirth is formatted as a string in "yyyy-MM-dd" format
    /// - Excludes unnecessary properties from the entity

    /// Source: Employee (Entity Model)
    /// Target: EmployeeViewModel (View Model)

    /// Mapping Pattern:
    /// Employee.FirstName + Employee.LastName → EmployeeDto.FullName
    /// Employee.DateOfBirth → EmployeeDto.DateOfBirthFormatted (as "yyyy-MM-dd")
    /// Employee.Department → EmployeeDto.Department (direct copy)
   
    public class EmployeeDto
    {
        
        /// Unique identifier for the employee. Copied from Employee.Id.
       
        public int Id { get; set; }

        
        /// Full name of the employee. Created by combining Employee.FirstName and Employee.LastName.
        /// This is an example of property flattening - combining multiple source properties into one.
        /// Example: "John Doe"
       
        public string? FullName { get; set; }

        
        /// Employee's department assignment.
        /// Directly mapped from Employee.Department.
       
        public string? Department { get; set; }

        
        /// Employee's date of birth in string format. Created from Employee.DateOfBirth.
        /// Format: "yyyy-MM-dd" (ISO 8601 format)
        /// Example: "1985-05-20"

        /// This is an example of value transformation - converting DateTime to formatted string.
       
        public string? DateOfBirthFormatted { get; set; }
    }
}

```
### 5. **Employee.cs** - Represents the database model for an employee.

```C#
namespace AutomapperTutorial.Models
{
    
    /// Employee Entity - Represents the database model for an employee.

    /// This class directly maps to the database table structure and contains
    /// all employee-related information as stored in the database.
 
    /// Mapping Targets:
    /// - Maps to: EmployeeDto (flattens FirstName + LastName into FullName)
   
    public class Employee
    {
        
        /// Unique identifier for the employee. Primary key in the database.
       
        public int Id { get; set; }

        
        /// Employee's first name. Nullable string.
        /// Combined with LastName to create FullName in EmployeeDto.
       
        public string? FirstName { get; set; }

        
        /// Employee's last name. Nullable string.
        /// Combined with FirstName to create FullName in EmployeeDto.
       
        public string? LastName { get; set; }

        
        /// Employee's date of birth. Used for age calculation and formatted display.
        /// Formatted as "yyyy-MM-dd" when mapped to EmployeeDto.
       
        public DateTime DateOfBirth { get; set; }

        
        /// Employee's department assignment. Examples: IT, HR, Finance, Sales.
        /// Passed through to EmployeeDto without modification.
       
        public string? Department { get; set; }
    }
}

```
---

## Examples

### Basic Mapping

```c#
// Create source object
Employee employee = new Employee
{
    Id = 1,
    FirstName = "John",
    LastName = "Doe",
    DateOfBirth = new DateTime(1985, 5, 20),
    Department = "IT"
};

// Map to DTO
EmployeeDto employeeDto = _mapper.Map<EmployeeDto>(employee);

// Result: FullName = "John Doe", DateOfBirthFormatted = "1985-05-20"
```

### Chained Mapping

```c#
// Map from Entity → DTO → ViewModel in sequence
EmployeeDto employeeDto = _mapper.Map<EmployeeDto>(employee);
EmployeeViewModel viewModel = _mapper.Map<EmployeeViewModel>(employeeDto);

// Or use ProjectTo for LINQ queries
var viewModels = dbContext.Employees
    .ProjectTo<EmployeeViewModel>(_mapper.ConfigurationProvider)
    .ToList();
```

### Custom Transformations

The `ForMember()` method allows custom transformations:

```c#
CreateMap<Employee, EmployeeDto>()
    // Concatenate first and last names
    .ForMember(dest => dest.FullName, 
        opt => opt.MapFrom(src => $"{src.FirstName} {src.LastName}"))
    
    // Format date
    .ForMember(dest => dest.DateOfBirthFormatted, 
        opt => opt.MapFrom(src => src.DateOfBirth.ToString("yyyy-MM-dd")))
    
    // Conditional mapping
    .ForMember(dest => dest.Department, 
        opt => opt.Condition(src => src.Department != null));
```

---

## Running the Application

### Standard Execution

```bash
dotnet run --project AutomapperTutorial.csproj
```

### With License Key (Environment Variable)

```bash
# Set environment variable first
$env:AUTOMAPPER_LICENSE_KEY="your-license-key"

# Then run
dotnet run --project AutomapperTutorial.csproj
```

### Build and Run

```bash
# Build
dotnet build

# Run specific executable
./bin/Debug/net8.0/AutomapperTutorial.exe
```

### Expected Output

```
Employee DTO:
ID: 1
Full Name: John Doe
Department: IT
Date of Birth: 1985-05-20

Employee ViewModel:
Full Name: John Doe
Department: IT
Date of Birth: 1985-05-20
```

## License Information

### AutoMapper 15.1.0 License Requirement

Starting with AutoMapper 15.0, the library requires a valid license key for production use. This is provided by Lucky Penny Software.

#### License Types:

- **Development/Testing**: Free - no license key required (with warning)
- **Production**: Requires a valid license key

#### Obtaining a License:

1. Visit: https://luckypennysoftware.com
2. Register an account
3. Subscribe to a plan (monthly or annual)
4. Copy your license key from the dashboard

#### Adding Your License Key:

In `Program.cs`, replace the license key:

```c#
configExpression.LicenseKey = "your-actual-license-key-here";
```

#### Best Practices for License Management:

**Option 1: Environment Variable (Recommended)**
```c#
var licenseKey = Environment.GetEnvironmentVariable("AUTOMAPPER_LICENSE_KEY");
configExpression.LicenseKey = licenseKey;
```

Set the environment variable:
```bash
# Windows PowerShell
[Environment]::SetEnvironmentVariable("AUTOMAPPER_LICENSE_KEY", "your-key", "User")

# Windows Command Prompt
setx AUTOMAPPER_LICENSE_KEY "your-key"

# Linux/macOS
export AUTOMAPPER_LICENSE_KEY="your-key"
```

**Option 2: Configuration File**
```json
{
  "AutoMapper": {
    "LicenseKey": "your-license-key-here"
  }
}
```

In `Program.cs`:
```c#
var licenseKey = builder.Configuration["AutoMapper:LicenseKey"];
configExpression.LicenseKey = licenseKey;
```

**Option 3: Azure Key Vault (Production)**
```c#
var licenseKey = await keyVaultClient.GetSecretAsync("automapper-license-key");
configExpression.LicenseKey = licenseKey.Value;
```

#### Current License Status:

✅ **Current License Key**: Valid (expires January 15, 2027)
- Account ID: 019a7312-00da-7bf7-88a6-5d3cc47c40fb
- Customer ID: ctm_01k9sh510vt25ce81p0tv228yw
- Edition: 0
- Type: 2

---


## License

This project uses AutoMapper, which requires a license key for production use.

**AutoMapper License**: https://luckypennysoftware.com

**Project Code**: MIT (or your chosen license)

---
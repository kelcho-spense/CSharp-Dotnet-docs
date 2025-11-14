# OOP Real-Time Examples

## Encapsulation Principle
### Real-time Example of Encapsulation Principle: Bank Account Management System
One real-time example of encapsulation is a bank account management system. Let’s consider the scenario where a bank customer can deposit or withdraw money from their account. Still, certain rules and validations are applied, such as ensuring that the balance does not go below a minimum limit or the customer can’t withdraw more than what they have in the account. Let us see how we can implement this example using the Encapsulation Principle in C#:

```C#
using System;
namespace EncapsulationPrincipleCSharp
{
    public class BankAccount
    {
        // This private field is encapsulated and can't be directly accessed from outside the class.
        private decimal balance; 

        public decimal Balance
        {
            // Only provides a way to read the balance but not modify it directly.
            get { return balance; } 
        }

        public BankAccount(decimal initialBalance)
        {
            if (initialBalance < 0)
            {
                throw new ArgumentException("Initial balance cannot be negative.");
            }

            balance = initialBalance;
        }

        public void Deposit(decimal amount)
        {
            if (amount <= 0)
            {
                throw new ArgumentException("Deposit amount should be positive.");
            }

            balance += amount;
        }

        public void Withdraw(decimal amount)
        {
            if (amount <= 0)
            {
                throw new ArgumentException("Withdrawal amount should be positive.");
            }

            if (balance - amount < 0)
            {
                throw new InvalidOperationException("Insufficient funds.");
            }

            balance -= amount;
        }
    }

    //Testing Encapsulation Principle
    public class Program
    {
        static void Main(string[] args)
        {
            // Starts with a balance of 500
            BankAccount myAccount = new BankAccount(500);

            // Balance becomes 700
            myAccount.Deposit(200); 
            Console.WriteLine(myAccount.Balance); // Outputs: 700

            // Balance becomes 600
            myAccount.Withdraw(100); 
            Console.WriteLine(myAccount.Balance); // Outputs: 600

            // myAccount.balance = -1000;  // This would be an error, as the balance field is private and inaccessible directly.
            
            Console.Read();
        }
    }
}
```

In this Example:

* The BankAccount class encapsulates the balance field, meaning it can’t be directly accessed or modified from outside the class. Instead, operations like depositing or withdrawing are controlled by methods (Deposit and Withdraw).
* The Balance property only provides a getter, ensuring the balance can be viewed but not changed directly.
* Methods Deposit and Withdraw encapsulate the business rules of our banking system, such as ensuring you can’t have a negative initial balance, can’t deposit negative amounts, and can’t withdraw more than you have.

### Encapsulation Real-time Example: Coffee Machine

Let’s consider another Real-time Example of the coffee machine. A coffee machine has internal mechanisms and operations (like grinding coffee beans, heating water, etc.) that the user shouldn’t be concerned about. The user selects the type of coffee they want, and the machine delivers the final product. Let us see how we can implement this example using the Encapsulation Principle in C#:

```C#
using System;
namespace EncapsulationPrincipleCSharp
{
    public class CoffeeMachine
    {
        private int waterAmount; // in milliliters
        private int beansAmount; // in grams
        private bool isHeated;

        public CoffeeMachine(int water, int beans)
        {
            waterAmount = water;
            beansAmount = beans;
            isHeated = false;
        }

        private void HeatWater()
        {
            if (!isHeated)
            {
                Console.WriteLine("Heating water...");
                isHeated = true;
            }
        }

        private void GrindBeans(int amount)
        {
            if (beansAmount < amount)
            {
                throw new InvalidOperationException("Not enough coffee beans!");
            }
            Console.WriteLine("Grinding coffee beans...");
            beansAmount -= amount;
        }

        // This is the method exposed to the user.
        public void MakeEspresso()
        {
            HeatWater();
            GrindBeans(20); // let's say we need 20 grams of beans for an espresso
            Console.WriteLine("Making Espresso...");
        }

        // Another method exposed to the user.
        public void MakeLatte()
        {
            HeatWater();
            GrindBeans(25); // we need 25 grams of beans for a latte
            Console.WriteLine("Making Latte...");
        }

        public int BeansLeft()
        {
            return beansAmount;
        }

        public int WaterLeft()
        {
            return waterAmount;
        }
    }
    
    //Testing Encapsulation Principle
    public class Program
    {
        static void Main(string[] args)
        {
            CoffeeMachine myMachine = new CoffeeMachine(1000, 100);  // Initialize with 1000 ml of water and 100 grams of beans

            myMachine.MakeEspresso();  // Outputs: Heating water... Grinding coffee beans... Making Espresso...

            Console.WriteLine($"Beans left: {myMachine.BeansLeft()} grams");  // Outputs: Beans left: 80 grams

            Console.Read();
        }
    }
}
```

In the above example:

* The CoffeeMachine class encapsulates the internal workings of the machine. Methods like HeatWater and GrindBeans are private and cannot be accessed outside the class.
* Only methods meant for public use, like MakeEspresso and MakeLatte, are exposed. This ensures the user interacts with the machine in a controlled manner.
* The internal state of the coffee machine (water and bean amounts, whether water is heated or not) is protected from external manipulation, ensuring its integrity.

This is a simplified representation of a coffee machine, but it illustrates the encapsulation concept: hiding the internal complexity and exposing only what’s necessary for the user. When you run the above code, you will get the following output:

![image_127.png](image_127.png)

## Abstraction Principle
### Real-Time Example of Abstraction: Vehicle System

Let’s consider a Real-time Example Vehicle System. Suppose you want to model a basic vehicle system. All vehicles can be started and stopped, but the underlying details of how this happens can differ for each vehicle type. Let us see how we can implement this example using the Abstraction Principle in C#:

```C#
using System;
namespace AbstractionPrincipleCSharp
{
    //Abstract Base Class (Abstraction)
    public abstract class Vehicle
    {
        // These are abstract methods; the derived classes will provide the implementation.
        public abstract void Start();
        public abstract void Stop();
    }

    //Concrete Implementations
    public class Car : Vehicle
    {
        public override void Start()
        {
            Console.WriteLine("Car is starting with a key turn.");
        }

        public override void Stop()
        {
            Console.WriteLine("Car is stopping using its brakes.");
        }
    }

    public class ElectricTrain : Vehicle
    {
        public override void Start()
        {
            Console.WriteLine("Electric train is starting by powering up.");
        }

        public override void Stop()
        {
            Console.WriteLine("Electric train is stopping by cutting off the power.");
        }
    }
    
    //Testing Abstraction Principle
    public class Program
    {
        static void Main(string[] args)
        {
            //Using the Abstraction
            Vehicle myCar = new Car();
            Vehicle myTrain = new ElectricTrain();

            StartVehicle(myCar);  // Output: Car is starting with a key turn.
            StartVehicle(myTrain); // Output: Electric train is starting by powering up.

            Console.Read();
        }

        static void StartVehicle(Vehicle vehicle)
        {
            vehicle.Start();
        }
    }
}
```

In the example:

* The Vehicle class provides an abstraction for any vehicle. It doesn’t detail how a vehicle starts or stops. It just declares that any vehicle should be able to start and stop.
* The Car and ElectricTrain classes are concrete implementations of the Vehicle abstraction. They provide specific ways to start and stop.
* In the Main method, we use the Vehicle abstraction to start different types of vehicles. The actual method that gets called depends on the type of object passed in, which showcases polymorphism, another OOP principle.

In real-world applications, abstraction allows you to define a contract (in this case, “vehicles can start and stop”) without committing to specific details. This makes it easier to add new types of vehicles in the future without changing existing code.

### Real-Time Example of Abstraction Principle: Messaging System

Imagine a system that has to communicate with multiple **`messaging platforms: Email, SMS, and Push Notification`**. Without abstraction, you might end up with methods like `SendEmail()`, `SendSms()`, `SendPushNotification()`, etc. scattered throughout your application. If a new messaging method is added, you must add another method and call it differently. Let us see how we can implement this example using the Abstraction Principle in C#:

```C#
using System;
namespace AbstractionPrincipleCSharp
{
    //Abstraction Layer
    //Define a common interface for all messaging platforms
    public interface IMessagingService
    {
        void SendMessage(string recipient, string message);
    }

    //Concrete Implementations
    //Now, we'll create concrete classes that implement this interface.
    public class EmailService : IMessagingService
    {
        public void SendMessage(string recipient, string message)
        {
            Console.WriteLine($"Sending Email to {recipient}: {message}");
            // Here, you'd have the actual logic to send an email.
        }
    }

    public class SmsService : IMessagingService
    {
        public void SendMessage(string recipient, string message)
        {
            Console.WriteLine($"Sending SMS to {recipient}: {message}");
            // Here, you'd have the actual logic to send an SMS.
        }
    }

    public class PushNotificationService : IMessagingService
    {
        public void SendMessage(string recipient, string message)
        {
            Console.WriteLine($"Sending Push Notification to {recipient}: {message}");
            // Here, you'd have the actual logic to send a push notification.
        }
    }
    
    //Testing Abstraction Principle
    public class Program
    {
        static void Main(string[] args)
        {
            // Using the Abstraction
            // With the Abstraction Principle applied, if you need to send a message, 
            // you don’t need to know if it’s an email, SMS, or push notification.
            // You just call SendMessage() on any service implementing IMessagingService.
            IMessagingService emailService = new EmailService();
            IMessagingService smsService = new SmsService();
            IMessagingService pushService = new PushNotificationService();

            SendAlert(emailService, "example@example.com", "Hello via Email!");
            SendAlert(smsService, "1234567890", "Hello via SMS!");
            SendAlert(pushService, "User123", "Hello via Push Notification!");

            Console.Read();
        }

        static void SendAlert(IMessagingService service, string recipient, string message)
        {
            service.SendMessage(recipient, message);
        }
    }
}
```

By following the Abstraction Principle, we’ve ensured:

* A consistent method to send messages across different platforms.
* Ease of extension. If we add a new messaging platform, we implement IMessagingService without changing how we send messages in our main application.
* Decoupling. The high-level logic (sending alerts) is decoupled from the low-level messaging details (email, SMS, push).

When you run the above code, you will get the following output:

![image_128.png](image_128.png)

## Inheritance Principle 
### Real-Time Example of Inheritance Principle: Vehicle Management System

Consider different types of vehicles, such as a basic vehicle, a car, and a motorcycle. All vehicles can be started and stopped and have a speed. However, cars and motorcycles have specific properties and behaviors. Let us see how we can implement this example using the Inheritance Principle in C#:

```C#
using System;
namespace InheritancePrincipleCSharp
{
    //Base Class (Parent Class) - Vehicle
    public class Vehicle
    {
        public int Speed { get; protected set; }

        public void Start()
        {
            Console.WriteLine("Vehicle started.");
        }

        public void Stop()
        {
            Console.WriteLine("Vehicle stopped.");
        }

        public virtual void Accelerate()
        {
            Speed += 5;
            Console.WriteLine($"Vehicle accelerates. Current speed: {Speed} km/h.");
        }
    }

    //Derived Class (Child Class) - Car
    public class Car : Vehicle
    {
        public int Doors { get; set; }

        public override void Accelerate()
        {
            Speed += 10;
            Console.WriteLine($"Car accelerates. Current speed: {Speed} km/h.");
        }

        public void OpenSunroof()
        {
            Console.WriteLine("Sunroof opened.");
        }
    }

    //Derived Class (Child Class) - Motorcycle
    public class Motorcycle : Vehicle
    {
        public bool HasSideCar { get; set; }

        public override void Accelerate()
        {
            Speed += 7;
            Console.WriteLine($"Motorcycle accelerates. Current speed: {Speed} km/h.");
        }

        public void UseKickstand()
        {
            Console.WriteLine("Kickstand placed.");
        }
    }
    
    //Testing Inheritance Principle
    public class Program
    {
        static void Main(string[] args)
        {
            //Using the Inheritance
            Car myCar = new Car { Doors = 4 };
            myCar.Start();
            myCar.Accelerate();
            myCar.OpenSunroof();
            myCar.Stop();

            Console.WriteLine();

            Motorcycle myBike = new Motorcycle { HasSideCar = false };
            myBike.Start();
            myBike.Accelerate();
            myBike.UseKickstand();
            myBike.Stop();

            Console.Read();
        }
    }
}
```
In this Example:

* The Vehicle class represents the general properties and behaviors of vehicles.
* The Car and Motorcycle classes inherit from Vehicle, which means they automatically get the Start, Stop, and Accelerate methods. Additionally, they can have their own specific properties (like Doors for Cars or HasSideCar for Motorcycles) and behaviors (like OpenSunroof for Cars or UseKickstand for Motorcycles).
* The Accelerate method is overridden in both Car and Motorcycle to provide specific acceleration behaviors for each.

This example demonstrates the power of inheritance to promote code reuse, establish hierarchies, and allow specialized behavior in derived classes. When you run the above code, you will get the following output:

![image_129.png](image_129.png)

### Real-Time Example of Inheritance Principle: Education System
Let’s consider another real-time example to understand the inheritance principle in C#, i.e., the education system scenario with students and teachers. In an educational institution, students and teachers are people with common attributes like name, age, and address. However, they also have distinct properties and behaviors. For instance, a student may enroll in a course while a teacher might teach one. Let us see how we can implement this example using the Inheritance Principle in C#:

```C#
using System;
namespace InheritancePrincipleCSharp
{
    //Base Class (Parent Class) - Person
    public class Person
    {
        public string Name { get; set; }
        public int Age { get; set; }
        public string Address { get; set; }

        public Person(string name, int age, string address)
        {
            Name = name;
            Age = age;
            Address = address;
        }

        public void DisplayDetails()
        {
            Console.WriteLine($"Name: {Name}, Age: {Age}, Address: {Address}");
        }
    }

    //Derived Class (Child Class) - Student
    public class Student : Person
    {
        public string StudentId { get; set; }

        public Student(string name, int age, string address, string studentId)
            : base(name, age, address) // Calling base class constructor
        {
            StudentId = studentId;
        }

        public void Enroll(string courseName)
        {
            Console.WriteLine($"{Name} has enrolled in {courseName} course.");
        }
    }

    //Derived Class (Child Class) - Teacher
    public class Teacher : Person
    {
        public string EmployeeId { get; set; }

        public Teacher(string name, int age, string address, string employeeId)
            : base(name, age, address) // Calling base class constructor
        {
            EmployeeId = employeeId;
        }

        public void Teach(string courseName)
        {
            Console.WriteLine($"{Name} is teaching {courseName} course.");
        }
    }
    
    //Testing Inheritance Principle
    public class Program
    {
        static void Main(string[] args)
        {
            //Using the Inheritance
            Student john = new Student("John Doe", 20, "123 Main St", "S12345");
            john.DisplayDetails();
            john.Enroll("Mathematics");

            Console.WriteLine();

            Teacher mrsSmith = new Teacher("Mrs. Smith", 40, "456 Elm St", "T98765");
            mrsSmith.DisplayDetails();
            mrsSmith.Teach("Physics");

            Console.Read();
        }
    }
}
```
In this example:

* The Person class captures general attributes and behaviors common to students and teachers.
* The Student and Teacher classes inherit from Person. This inheritance means they automatically have properties like Name, Age, and Address and the DisplayDetails method. Additionally, they introduce specific properties and methods like Enroll for Student and Teach for Teacher.
* The derived classes’ constructors utilize the base keyword to invoke the base class’s constructor, ensuring that the common properties are set.

This example demonstrates how inheritance can model real-world relationships, facilitate code reuse, and provide a structured way to represent hierarchies in your system

## Polymorphism Principle
### Real-Time Example of Polymorphism Principle: Animals Sounds

Here’s a simple real-time example: consider animals making sounds. While every animal makes a sound, each animal’s sound is distinct. You can leverage polymorphism to model this scenario. Let’s walk through it:

* Base Class: Animal
* Derived Classes: Dog, Cat
Let us see how we can implement this example using the Polymorphism Principle in C#:

```C#
using System;

namespace PolymorphismExample
{
    // Base class
    public abstract class Animal
    {
        public abstract void MakeSound();
    }

    // Derived class
    public class Dog : Animal
    {
        public override void MakeSound()
        {
            Console.WriteLine("The dog barks.");
        }
    }

    // Another derived class
    public class Cat : Animal
    {
        public override void MakeSound()
        {
            Console.WriteLine("The cat meows.");
        }
    }

    //Testing Polymorphism Principle
    public class Program
    {
        static void Main(string[] args)
        {
            Animal myDog = new Dog();
            Animal myCat = new Cat();

            MakeAnimalSound(myDog); // Outputs: The dog barks.
            MakeAnimalSound(myCat); // Outputs: The cat meows.

            Console.Read();
        }

        // This function showcases polymorphism in action.
        // Even though it accepts a parameter of type 'Animal', 
        // it's able to handle any derived type.
        static void MakeAnimalSound(Animal animal)
        {
            animal.MakeSound();
        }
    }
} 
```

In this Example:

* We have an abstract Animal class with an abstract method, MakeSound().
* Derived classes (Dog and Cat) provide their own implementation of the MakeSound() method.
* In the Program class, even though we use the Animal type to hold references to the derived classes, we can still call the appropriate derived class’s MakeSound() method. This is the essence of polymorphism.

This allows for flexibility and makes adding more animal types in the future easier without making major changes to existing code. If you were to add a new animal, say Bird, you’d need to create a Bird class derived from Animal and provide its own implementation for the MakeSound() method.

### Real-Time Example of Polymorphism Principle: Payment Processing System

Let’s understand polymorphism using another real-world example: Payment Processing System. Different payment methods, such as credit cards, bank transfers, and digital wallets, could have different processes to execute a payment. Here’s how this can be represented with polymorphism:

* Base Class: PaymentMethod
* Derived Classes: CreditCard, BankTransfer, DigitalWallet
Let us see how we can implement this example using the Polymorphism Principle in C#:

````C#
using System;

namespace PaymentPolymorphismExample
{
    // Base class
    public abstract class PaymentMethod
    {
        public abstract void ExecutePayment(decimal amount);
    }

    // Derived class for credit card payment
    public class CreditCard : PaymentMethod
    {
        public override void ExecutePayment(decimal amount)
        {
            Console.WriteLine($"Processing a credit card payment for ${amount}.");
            // Here you'd have logic specific to credit card processing
        }
    }

    // Derived class for bank transfer
    public class BankTransfer : PaymentMethod
    {
        public override void ExecutePayment(decimal amount)
        {
            Console.WriteLine($"Processing a bank transfer for ${amount}.");
            // Logic specific to bank transfers would be here
        }
    }

    // Derived class for digital wallet payment
    public class DigitalWallet : PaymentMethod
    {
        public override void ExecutePayment(decimal amount)
        {
            Console.WriteLine($"Processing a digital wallet payment for ${amount}.");
            // Logic specific to digital wallet payment would go here
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            PaymentMethod creditCardPayment = new CreditCard();
            PaymentMethod bankTransferPayment = new BankTransfer();
            PaymentMethod digitalWalletPayment = new DigitalWallet();

            ProcessPayment(creditCardPayment, 100.00M);      // Outputs: Processing a credit card payment for $100.00.
            ProcessPayment(bankTransferPayment, 250.50M);    // Outputs: Processing a bank transfer for $250.50.
            ProcessPayment(digitalWalletPayment, 75.25M);    // Outputs: Processing a digital wallet payment for $75.25.

            Console.ReadKey();
        }

        // Demonstrating polymorphism.
        // This function can accept any payment method derived from PaymentMethod.
        static void ProcessPayment(PaymentMethod paymentMethod, decimal amount)
        {
            paymentMethod.ExecutePayment(amount);
        }
    }
}
````
In this Example:

* The PaymentMethod base class provides a contract with an abstract ExecutePayment method.
* Each derived class (e.g., CreditCard, BankTransfer, DigitalWallet) provides its own specific implementation for processing the payment.
* The ProcessPayment function in the Program class accepts any payment method (an object of a type derived from PaymentMethod) and processes the payment using polymorphism. This provides flexibility to introduce new payment methods in the future without changing the core payment processing logic.

When you run the above code, you will get the following output:

![image_130.png](image_130.png)

## Interface 

### Real-Time Example of Interface: Vehicles
Imagine you have different types of vehicles like cars, boats, and airplanes. Each of these vehicles can move, but how they move differs. Create a simple program where each vehicle type displays its mode of movement. Let us implement this example using Interface in C#:

```C#
using System;
using System.Collections.Generic;

namespace InterfaceCSharp
{
    //Step 1: Define the IMovable interface.
    public interface IMovable
    {
        void Move();
    }

    //Step 2: Implement the interface for different types of vehicles.
    // Car.cs
    public class Car : IMovable
    {
        public void Move()
        {
            Console.WriteLine("The car drives on the road.");
        }
    }

    // Boat.cs
    public class Boat : IMovable
    {
        public void Move()
        {
            Console.WriteLine("The boat sails on the water.");
        }
    }

    // Airplane.cs
    public class Airplane : IMovable
    {
        public void Move()
        {
            Console.WriteLine("The airplane flies in the sky.");
        }
    }
    
    //Step 4: Testing the application.
    class Program
    {
        static void Main(string[] args)
        {
            List<IMovable> vehicles = new List<IMovable>
            {
                new Car(),
                new Boat(),
                new Airplane()
            };

            foreach (var vehicle in vehicles)
            {
                vehicle.Move();
            }
            Console.ReadKey();
        }
    }
}
```
In this real-time example, the interface IMovable acts as a contract, ensuring that every vehicle type that implements it will have a Move method. This abstraction allows us to handle different types of vehicles uniformly, demonstrated in the foreach loop in the main program.

With this pattern, if we want to introduce more vehicle types (e.g., a bicycle, a skateboard, or a rocket), we create a new class for each and implement the IMovable interface. Our main program remains unchanged and functions correctly for any new vehicle type. When you run the above code, you will get the following output:

![image_131.png](image_131.png)

### Real-Time Example of Interface: Payment Gateway Integration

This is a typical scenario in many applications, where you might want to support multiple payment providers (like PayPal, Stripe, and others). In this context, interfaces become invaluable because they allow us to define a consistent way of processing payments, regardless of the underlying provider. Let us implement this example using Interface in C#:

```C#
using System;
namespace InterfaceCSharp
{
    //Step 1: Define the IPaymentGateway interface.
    public interface IPaymentGateway
    {
        bool ProcessPayment(decimal amount);
    }

    //Step 2: Implement the interface for different payment providers.
    // PayPalPaymentGateway.cs
    public class PayPalPaymentGateway : IPaymentGateway
    {
        public bool ProcessPayment(decimal amount)
        {
            // Call PayPal's API to process the payment
            Console.WriteLine($"Processing ${amount} payment using PayPal...");
            return true;  // assume success for the sake of this example
        }
    }

    // StripePaymentGateway.cs
    public class StripePaymentGateway : IPaymentGateway
    {
        public bool ProcessPayment(decimal amount)
        {
            // Call Stripe's API to process the payment
            Console.WriteLine($"Processing ${amount} payment using Stripe...");
            return true;  // assume success for this example
        }
    }

    //Step 3: Use the implementations in a shopping cart scenario.
    public class ShoppingCart
    {
        private IPaymentGateway _paymentGateway;

        public ShoppingCart(IPaymentGateway paymentGateway)
        {
            _paymentGateway = paymentGateway;
        }

        public void Checkout(decimal amount)
        {
            if (_paymentGateway.ProcessPayment(amount))
            {
                Console.WriteLine("Payment was successful!");
            }
            else
            {
                Console.WriteLine("Payment failed. Please try again.");
            }
        }
    }

    //Step 4: Testing the application.
    class Program
    {
        static void Main(string[] args)
        {
            // Customer selects PayPal as their preferred payment method
            ShoppingCart cart = new ShoppingCart(new PayPalPaymentGateway());
            cart.Checkout(100.00M);

            // Another customer selects Stripe
            cart = new ShoppingCart(new StripePaymentGateway());
            cart.Checkout(200.00M);

            Console.ReadKey();
        }
    }
}
```
n this real-time example, the IPaymentGateway interface abstracts the specifics of processing payments. The ShoppingCart class doesn’t need to know the details of each payment provider. It is called ProcessPayment, and the correct payment provider takes care of the rest.

This architecture makes it easy to add more payment providers in the future. When you want to add a new one, implement the IPaymentGateway interface, and the rest of the system can remain unchanged. When you run the above code, you will get the following output:

![image_132.png](image_132.png)

### Real-Time Example of Interface in C#: User Authentication Methods
Imagine a scenario where an application allows users to authenticate using different methods, such as a password, fingerprint, or face recognition. Develop a mechanism that can support multiple authentication methods without changing the core authentication process. Let us implement this example using Interface in C#:

```C#
using System;
using System.Collections.Generic;

namespace InterfaceCSharp
{
    //Step 1: Define the IAuthenticator interface.
    public interface IAuthenticator
    {
        bool Authenticate();
    }

    //Step 2: Implement the interface for different authentication methods.
    // PasswordAuthenticator.cs
    public class PasswordAuthenticator : IAuthenticator
    {
        public bool Authenticate()
        {
            // Logic for password authentication
            Console.WriteLine("Authenticating using password...");
            return true;  // For simplicity, assume authentication always succeeds
        }
    }

    // FingerprintAuthenticator.cs
    public class FingerprintAuthenticator : IAuthenticator
    {
        public bool Authenticate()
        {
            // Logic for fingerprint authentication
            Console.WriteLine("Authenticating using fingerprint...");
            return true;
        }
    }

    // FaceRecognitionAuthenticator.cs
    public class FaceRecognitionAuthenticator : IAuthenticator
    {
        public bool Authenticate()
        {
            // Logic for face recognition authentication
            Console.WriteLine("Authenticating using face recognition...");
            return true;
        }
    }

    //Step 3: Implement the authentication process using the interfaces.
    public class AuthService
    {
        private IAuthenticator _authenticator;

        public AuthService(IAuthenticator authenticator)
        {
            _authenticator = authenticator;
        }

        public void AuthenticateUser()
        {
            if (_authenticator.Authenticate())
            {
                Console.WriteLine("Authentication successful!");
            }
            else
            {
                Console.WriteLine("Authentication failed.");
            }
        }
    }

    //Step 4: Testing the application.
    class Program
    {
        static void Main(string[] args)
        {
            AuthService passwordService = new AuthService(new PasswordAuthenticator());
            passwordService.AuthenticateUser();

            AuthService fingerprintService = new AuthService(new FingerprintAuthenticator());
            fingerprintService.AuthenticateUser();

            AuthService faceService = new AuthService(new FaceRecognitionAuthenticator());
            faceService.AuthenticateUser();

            Console.ReadKey();
        }
    }
}
```

The IAuthenticator interface offers a consistent way to implement different authentication methods. The main AuthService class remains decoupled from the specifics of each method. This architecture makes it easy to add or change authentication methods in the future without affecting the core logic of the AuthService.

## Abstract Class
### Real-Time Example of Abstract Class in C#: Banking System

Let’s see a real-time example of an abstract class in C# by considering a scenario related to the Banking System.

* Abstract Class – BankAccount
* Concrete Classes – SavingsAccount, CurrentAccount

Let us implement this example using Abstract Class in C#:

```C#
using System;
namespace AbstractClassCSharp
{
    //BankAccount.cs (Abstract Class)
    public abstract class BankAccount
    {
        public string AccountNumber { get; private set; }
        public string AccountHolder { get; private set; }
        protected double Balance { get; set; }

        public BankAccount(string accountNumber, string accountHolder)
        {
            AccountNumber = accountNumber;
            AccountHolder = accountHolder;
        }

        // Abstract methods
        public abstract void Deposit(double amount);
        public abstract void Withdraw(double amount);

        // Virtual method with a default implementation
        public virtual void DisplayBalance()
        {
            Console.WriteLine($"Account Number: {AccountNumber}, Account Holder: {AccountHolder}, Balance: ${Balance}");
        }
    }

    //SavingsAccount.cs (Concrete Class)
    public class SavingsAccount : BankAccount
    {
        private double _interestRate = 0.03; // 3% annual interest

        public SavingsAccount(string accountNumber, string accountHolder)
            : base(accountNumber, accountHolder) { }

        public override void Deposit(double amount)
        {
            Balance += amount;
            Console.WriteLine($"Deposited ${amount} into Savings Account.");
        }

        public override void Withdraw(double amount)
        {
            if (Balance - amount >= 0)
            {
                Balance -= amount;
                Console.WriteLine($"Withdrew ${amount} from Savings Account.");
            }
            else
            {
                Console.WriteLine("Insufficient funds for withdrawal.");
            }
        }

        public void AddInterest()
        {
            Balance += Balance * _interestRate;
            Console.WriteLine($"Interest added. New Balance: ${Balance}");
        }
    }

    //CurrentAccount.cs (Concrete Class)
    public class CurrentAccount : BankAccount
    {
        private double _overdraftLimit = 1000.0;

        public CurrentAccount(string accountNumber, string accountHolder)
            : base(accountNumber, accountHolder) { }

        public override void Deposit(double amount)
        {
            Balance += amount;
            Console.WriteLine($"Deposited ${amount} into Current Account.");
        }

        public override void Withdraw(double amount)
        {
            if ((Balance - amount) >= -_overdraftLimit)
            {
                Balance -= amount;
                Console.WriteLine($"Withdrew ${amount} from Current Account.");
            }
            else
            {
                Console.WriteLine("Withdrawal exceeds overdraft limit.");
            }
        }
    }
    
    //Step 4: Testing the application.
    class Program
    {
        static void Main(string[] args)
        {
            SavingsAccount johnsSavings = new SavingsAccount("SA123456", "John Doe");
            johnsSavings.Deposit(1000);
            johnsSavings.Withdraw(200);
            johnsSavings.AddInterest();
            johnsSavings.DisplayBalance();

            CurrentAccount janesCurrent = new CurrentAccount("CA654321", "Jane Smith");
            janesCurrent.Deposit(500);
            janesCurrent.Withdraw(1000);
            janesCurrent.DisplayBalance();

            Console.ReadKey();
        }
    }
}
```

In this example, the abstract class BankAccount defines any bank account’s essential structure and functionalities, such as depositing and withdrawing money. The concrete classes, SavingsAccount and CurrentAccount, provide specific implementations for these functionalities and may have additional features like adding interest or handling overdrafts. When you run the above code, you will get the following output:

![image_134.png](image_134.png)





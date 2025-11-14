# OOP Real-Time Exercise Schenarios

## Encapsulation
### Encapsulation Real-time Example in C#: Car’s speedometer
Consider another real-world example: a car’s speedometer and related components. A car’s speedometer displays the current speed to the driver. Internally, this speed is determined by several factors and calculations related to wheel rotations and gear ratios, but the driver only needs to see the final speed value. They don’t need to know (and often don’t want to know) the complex calculations and mechanisms involved.

In the above Example:

* The car’s internal mechanism (e.g., the Move method and wheelRotations variable) is hidden from the driver.
* The driver only interacts with the Drive and ResetSpeedometer methods, which are exposed for public use.
* The CalculateSpeed method, though internal, takes care of the logic related to determining the current speed based on the wheel rotations and gear ratio.

Here, the complexity of calculating the speed and the internal state management related to wheel rotations are encapsulated within the Car class, and the driver only sees the final speed result.

### Encapsulation Principle Real-time Example in C#: Digital Wallet
Let’s see a different example to understand the Encapsulation Principle in C#, i.e., a digital wallet. A digital wallet stores money and users can perform operations such as depositing, withdrawing, and checking their balance. Internally, there might be various checks, encryption, and transaction logging, but these details are abstracted from the user. Let us see how we can implement this example using the Encapsulation Principle in C#:

In this Example:

* The DigitalWallet class encapsulates the balance and the transaction log.
* Deposit and withdrawal methods are designed with safety checks, and the transaction log records all transactions.
* Password verification is required for certain actions, enhancing security.
* GetTransactionLog returns a copy of the transaction log list to prevent external modifications, further emphasizing encapsulation.
## Abstraction
### Real-Time Example of Abstraction Principle in C#: Payment Processing
Let’s imagine a scenario where you’re developing a system for an online store that supports multiple payment methods like Credit Cards, PayPal, and Bitcoin. Without abstraction, you’d potentially have disparate methods like ProcessCreditCardPayment(), ProcessPayPalPayment(), etc. This would make the system rigid and difficult to extend if you were to introduce new payment methods. Let us see how we can implement this example using the Abstraction Principle in C#:

In this example, With abstraction:

* We’ve streamlined the process of handling various payment methods.
* Introducing a new payment method in the future becomes straightforward: implement the IPaymentMethod interface.
* The CheckoutSystem class is decoupled from specific payment implementations, making the codebase easier to maintain and extend.

### Real-Time Example of Abstraction Principle in C#: Zoo Management System
Consider a zoo management system where you have various types of animals, and each animal can make a sound. Without abstraction, you’d have separate methods for each animal’s sound. But with the Abstraction Principle, you can simplify this. Let us see how we can implement this example using the Abstraction Principle in C#:

Advantages:
* Consistency: The ZooKeeper can interact with any animal using a consistent method, MakeSound(), regardless of the animal’s specific type.
* Extensibility: To introduce a new animal into the system, you create a new class that implements the IAnimal interface. The rest of the system doesn’t need to change.
* Decoupling: The ZooKeeper class is now decoupled from specific animal implementations. This makes the codebase more maintainable and easier to understand.

## Inheritance
### Real-Time Example of Inheritance Principle in C#: Animals in a Zoo
A zoo has different types of animals, like mammals, birds, and reptiles. All animals have common properties such as name, age, and diet, but each type might have unique behaviors. For instance, birds can fly, while mammals might have a specific communication method. Let us see how we can implement this example using the Inheritance Principle in C#:

In this example:

* The Animal class represents general properties and behaviors common to all animals.
* The Bird and Mammal classes inherit from Animals, implying they automatically obtain properties like Name, Age, Diet, and the Eat method. However, they also introduce specific behaviors: Bird has a Fly method, while Mammal has a Communicate method.
* The Display method is overridden in both Bird and Mammal classes to provide specialized behavior in addition to the base behavior.

### Real-Time Example of Inheritance Principle in C#: Library System
A library contains various items, including books, magazines, and DVDs. All library items have a unique identifier and a title and can be borrowed or returned. However, each type might have unique properties; for instance, a book has an author and many pages, while a DVD might have a runtime. Let us see how we can implement this example using the Inheritance Principle in C#:

In this Example:

* The LibraryItem class defines basic properties and behaviors common to all library items, such as borrowing and returning.
* The Book and DVD classes inherit from LibraryItem, automatically obtaining properties like Id and Title and methods like Borrow and Return. Yet, they also add their own unique attributes: Book introduces Author and Pages, while DVD introduces Runtime.
* Each class has a method to display specific details: DisplayBookInfo for Book and DisplayDVDInfo for DVD.

Through this example, the inheritance principle in C# allows us to model a real-world library system, promote code reuse, and maintain an organized structure of the different items within the library.

## Polymorphism
### Real-Time Example of Polymorphism Principle in C#: Managing a Fleet of Vehicles
Let’s understand polymorphism using another real-world example: Managing a Fleet of Vehicles. Each vehicle can be driven, but the driving experience and method might vary based on the type of vehicle. For example, you drive a car differently than you would pilot a boat.

* Base Class: Vehicle
* Derived Classes: Car, Boat, Bicycle

In this Example:

* The Vehicle is the base class with an abstract method, Drive.
* The derived classes Car, Boat, and Bicycle each provide their own unique implementations of the Drive method.
* The OperateVehicle function in the Program class can accept any object of type Vehicle (or derived from Vehicle) and invokes the appropriate Drive method due to polymorphism.

Should you need to introduce a new type of vehicle, such as a “Helicopter,” you’d create a new derived class from the Vehicle and implement a specific way to pilot it. No changes to the OperateVehicle method would be necessary.

### Real-Time Example of Polymorphism Principle in C#: Notification System
Let’s understand polymorphism using another real-world example: a notification system in which different notification methods (e.g., Email, SMS, Push Notification) are utilized. Here’s the setup:

* Base Class: Notification
* Derived Classes: EmailNotification, SmsNotification, PushNotification

In this Example:

* The base class Notification has properties like Recipient and Message and an abstract method Send.
* The derived classes (EmailNotification, SmsNotification, PushNotification) each provide specific implementations for the Send method.
* The SendNotification method in the Program class accepts any notification object derived from the base Notification class and sends it using polymorphism.

This approach allows for easy scalability. If, in the future, you wanted to add a new type of notification, say a “SlackNotification,” you’d derive it from the base Notification class and provide a specific Send implementation.

## Interface

### Real-Time Example of Interface in C#: Database Operations
Suppose we’re working on an application where we need to support interactions with different types of databases. By using interfaces, we can make our application flexible, extensible, and decoupled from the specifics of each database. Let us implement this example using Interface in C#:

In this real-world example, the IDatabase interface provides a clear contract for database operations, ensuring consistency regardless of the underlying database system. As a result, our DatabaseManager class becomes adaptable to any database system that implements the IDatabase interface. This means we can easily support new database systems without major changes to our application. When you run the above code, you will get the following output:

![image_133.png](image_133.png)

### Real-Time Example of Interface in C#: User Authentication Methods
Imagine a scenario where an application allows users to authenticate using different methods, such as a password, fingerprint, or face recognition. Develop a mechanism that can support multiple authentication methods without changing the core authentication process. Let us implement this example using Interface in C#:

The IAuthenticator interface offers a consistent way to implement different authentication methods. The main AuthService class remains decoupled from the specifics of each method. This architecture makes it easy to add or change authentication methods in the future without affecting the core logic of the AuthService.
# OOP Principles: Abstraction

The process of representing the essential features without including the background details is called Abstraction. In simple words, we can say that it is a process of defining a class by providing the necessary details to call the object operations (i.e., methods) by hiding its implementation details. It is called abstraction in C#

## Real-Time Example of Abstraction

We all use ATM machines for cash withdrawals, money transfers, retrieving min-statements, etc. in our daily lives. But we don’t know internally what things are happening inside an ATM machine when we insert an ATM card for performing different kinds of operations. Information `like where the server is,` `where the database server is`, what `programming language` they use to write the logic, `how they are validating the data`, how they are implementing logic for various kinds of operations, and what `SQL statements get executed on the database` when we perform any operations, all these things are hidden from us. What they provide as part of the ATM machine is services (cash withdrawal, money transfer, retrieving min-statement, etc), but how these services are implemented is abstracted to us.


### Example to Understand Abstraction Principle
Now, we are going to develop one application to implement the Banking functionality. First, we will develop the application without following the Abstraction Principle, and then we will understand the problems. Then, we will see what are the different mechanisms to implement the abstraction principle in C#. So, what we will do is we will create two classes. One class is for SBI Bank, and another class is for AXIX Bank. As part of each class, we are going to provide 5 services, which are as follows:

1. ValidateCard
2. WithdrawMoney
3. CheckBalanace
4. BankTransfer
5. MiniStatement

Then, from the Main method, we will create the instances of each class and will invoke the respective services, i.e., respective methods. Here, you can consider the Main method is the user who will use the services provided by the Bank classes.

```C#
using System;
namespace GarbageCollectionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Transaction doing SBI Bank");
            SBI sbi = new SBI();
            sbi.ValidateCard();
            sbi.WithdrawMoney();
            sbi.CheckBalanace();
            sbi.BankTransfer();
            sbi.MiniStatement();

            Console.WriteLine("\nTransaction doing AXIX Bank");
            AXIX AXIX = new AXIX();
            AXIX.ValidateCard();
            AXIX.WithdrawMoney();
            AXIX.CheckBalanace();
            AXIX.BankTransfer();
            AXIX.MiniStatement();

            Console.Read();
        }
    }
    
    public class SBI 
    {
        public void BankTransfer()
        {
            Console.WriteLine("SBI Bank Bank Transfer");
        }

        public void CheckBalanace()
        {
            Console.WriteLine("SBI Bank Check Balanace");
        }

        public void MiniStatement()
        {
            Console.WriteLine("SBI Bank Mini Statement");
        }

        public void ValidateCard()
        {
            Console.WriteLine("SBI Bank Validate Card");
        }

        public void WithdrawMoney()
        {
            Console.WriteLine("SBI Bank Withdraw Money");
        }
    }

    public class AXIX 
    {
        public void BankTransfer()
        {
            Console.WriteLine("AXIX Bank Bank Transfer");
        }

        public void CheckBalanace()
        {
            Console.WriteLine("AXIX Bank Check Balanace");
        }

        public void MiniStatement()
        {
            Console.WriteLine("AXIX Bank Mini Statement");
        }

        public void ValidateCard()
        {
            Console.WriteLine("AXIX Bank Validate Card");
        }

        public void WithdrawMoney()
        {
            Console.WriteLine("AXIX Bank Withdraw Money");
        }
    }
}
```
That is fine. We are getting output as expected. 
`Then what is the problem with the above implementation? `

> The problem is the user of our application accesses the SBI and AXIX classes directly. 
> Directly means they can go to the class definition and see the implementation details of the methods. That is, the user will come to know how the services or methods are implemented. This might cause security issues. We should not expose our implementation details to the outside.

## How to Implement Abstraction Principle
In C#, we can implement the abstraction OOPs principle in two ways. They are as follows:

* Using **Interface**
* Using **Abstract Classes** and **Abstract Methods**

`What are Interfaces`, and `what are Abstract Methods` and Abstract Classes that we will discuss in detail in our `upcoming article`? But for now, you just need to understand one thing: `both interface and abstract classes and abstract methods provide some mechanism to hide the implementation details by only exposing the services`. The user only knows what are services or methods available, but the user will not know how these services or methods are implemented. Let us see this with examples.

### Example to Implement Abstraction Principle using Interface
In the below example, I am using an interface to achieve the abstraction principle in C#. Using the interface, we can achieve 100% abstraction. Now, the user will only know the services that are defined in the interface, but how the services are implemented, the user will never know. This is how we can implement abstraction in C# by hiding the implementation details from the user. Here, the user will only know about IBank, but the user will not know about the SBI and AXIX Classes.

```C#
using System;
namespace GarbageCollectionDemo
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Transaction doing SBI Bank");
            IBank sbi = BankFactory.GetBankObject("SBI");
            sbi.ValidateCard();
            sbi.WithdrawMoney();
            sbi.CheckBalanace();
            sbi.BankTransfer();
            sbi.MiniStatement();

            Console.WriteLine("\nTransaction doing AXIX Bank");
            IBank AXIX = BankFactory.GetBankObject("AXIX");
            AXIX.ValidateCard();
            AXIX.WithdrawMoney();
            AXIX.CheckBalanace();
            AXIX.BankTransfer();
            AXIX.MiniStatement();

            Console.Read();
        }
    }

    public interface IBank
    {
        void ValidateCard();
        void WithdrawMoney();
        void CheckBalanace();
        void BankTransfer();
        void MiniStatement();
    }

    public class BankFactory
    {
        public static IBank GetBankObject(string bankType)
        {
            IBank BankObject = null;
            if (bankType == "SBI")
            {
                BankObject = new SBI();
            }
            else if (bankType == "AXIX")
            {
                BankObject = new AXIX();
            }
            return BankObject;
        }
    }

    public class SBI : IBank
    {
        public void BankTransfer()
        {
            Console.WriteLine("SBI Bank Bank Transfer");
        }

        public void CheckBalanace()
        {
            Console.WriteLine("SBI Bank Check Balanace");
        }

        public void MiniStatement()
        {
            Console.WriteLine("SBI Bank Mini Statement");
        }

        public void ValidateCard()
        {
            Console.WriteLine("SBI Bank Validate Card");
        }

        public void WithdrawMoney()
        {
            Console.WriteLine("SBI Bank Withdraw Money");
        }
    }

    public class AXIX : IBank
    {
        public void BankTransfer()
        {
            Console.WriteLine("AXIX Bank Bank Transfer");
        }

        public void CheckBalanace()
        {
            Console.WriteLine("AXIX Bank Check Balanace");
        }

        public void MiniStatement()
        {
            Console.WriteLine("AXIX Bank Mini Statement");
        }

        public void ValidateCard()
        {
            Console.WriteLine("AXIX Bank Validate Card");
        }

        public void WithdrawMoney()
        {
            Console.WriteLine("AXIX Bank Withdraw Money");
        }
    }
}
```

### Example to Implement Abstraction Principle using Abstract Class and Abstract Methods:
In the below example, we are using abstract class and abstract methods to achieve the abstraction principle in C#. We can achieve 0 to 100% abstraction using the abstract class and abstract methods. In the below example, the user will only know the services that are defined in the abstract class, but how these services are implemented, the user will never know. This is how we can implement abstraction in C# by hiding the implementation details from the user.

```C#
using System;
namespace Bank
{
   
    public abstract class IBank
    {
        public abstract void ValidateCard();
        public abstract void WithdrawMoney();
        public abstract void CheckBalanace();
        public abstract void BankTransfer();
        public abstract void MiniStatement();
    }

    public class BankFactory
    {
        public static IBank GetBankObject(string bankType)
        {
            IBank BankObject = null;
            if (bankType == "SBI")
            {
                BankObject = new SBI();
            }
            else if (bankType == "AXIX")
            {
                BankObject = new AXIX();
            }
            return BankObject;
        }
    }

    public class SBI : IBank
    {
        public override void BankTransfer()
        {
            Console.WriteLine("SBI Bank Bank Transfer");
        }

        public override void CheckBalanace()
        {
            Console.WriteLine("SBI Bank Check Balanace");
        }

        public override void MiniStatement()
        {
            Console.WriteLine("SBI Bank Mini Statement");
        }

        public override void ValidateCard()
        {
            Console.WriteLine("SBI Bank Validate Card");
        }

        public override void WithdrawMoney()
        {
            Console.WriteLine("SBI Bank Withdraw Money");
        }
    }

    public class AXIX : IBank
    {
        public override void BankTransfer()
        {
            Console.WriteLine("AXIX Bank Bank Transfer");
        }

        public override void CheckBalanace()
        {
            Console.WriteLine("AXIX Bank Check Balanace");
        }

        public override void MiniStatement()
        {
            Console.WriteLine("AXIX Bank Mini Statement");
        }

        public override void ValidateCard()
        {
            Console.WriteLine("AXIX Bank Validate Card");
        }

        public override void WithdrawMoney()
        {
            Console.WriteLine("AXIX Bank Withdraw Money");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Transaction doing SBI Bank");
            IBank sbi = BankFactory.GetBankObject("SBI");
            sbi.ValidateCard();
            sbi.WithdrawMoney();
            sbi.CheckBalanace();
            sbi.BankTransfer();
            sbi.MiniStatement();

            Console.WriteLine("\nTransaction doing AXIX Bank");
            IBank AXIX = BankFactory.GetBankObject("AXIX");
            AXIX.ValidateCard();
            AXIX.WithdrawMoney();
            AXIX.CheckBalanace();
            AXIX.BankTransfer();
            AXIX.MiniStatement();

            Console.Read();
        }
    }
}
```

> Using abstract class, we can achieve `0 to 100% abstraction`. The reason is that you can also provide implementation to the methods inside the abstract class. It does not matter whether you implement all methods or none of the methods inside the abstract class. This is allowed, which is not possible with an interface.

## Encapsulation vs Abstraction

* The `Encapsulation Principle is all about data hiding` (or **information hiding**). On the other hand, the `Abstraction Principle is all about detailed hiding` (**implementation hiding**).
* `Encapsulation principle`, we are exposing the data through `publicly exposed methods and properties`. The advantage is that we can validate the data before storing and returning it. On the other hand, using the `Abstraction Principle`, we are `exposing only the services so that the user can consume the services, but how the services/methods are implemented is hidden from the user`. The user will never know how the method is implemented.
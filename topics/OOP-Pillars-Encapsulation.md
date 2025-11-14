# OOP Principles: Encapsulation

Encapsulation Hides the internal state and functionality of an object and only allows access through a public set of functions. Let us simplify the above definition as follows:

The process of binding or grouping the `State (i.e., Data Members)` and `Behaviour (i.e., Member Functions)` together into a single unit `(i.e., class, interface, struct, etc.)` is called Encapsulation in C#. The Encapsulation Principle ensures that the state and behavior of a unit `(i.e., class, interface, struct, etc.)` cannot be accessed directly from other units (i.e., class, interface, struct, etc.).

![image_98.png](image_98.png)

### Example to Understand Encapsulation in C#:
Every class, interface, struct, enum, etc. that we created is an example of encapsulation, so let’s create a class called Bank as follows to understand the encapsulation:

```C#
namespace BankExample
{
     public class Bank
 {
     public long AccountNumber;
     public string Name;
     public int Balance;

     public Bank(long _accountNumber, string _name, int _balance)
     {
         AccountNumber = _accountNumber;
         Name = _name;
         Balance = _balance;
     }

     public void Getbalance()
     {
         Console.WriteLine($"Account Number: {AccountNumber}, Name: {Name}, Balance: {Balance}");
     }

     public int  WithdrawAmount(int deduction) 
     {
         return Balance -= deduction;
     }

     public int Deposit(int MoneyDeposited)
     {
         return  Balance += MoneyDeposited;
     }
 }
```

If other classes want to access these details, they need to create the object of the Bank class to access its data and behavior, as shown in the code below.

```C#
namespace BankExample
{
     class Program
 {
     static void Main(string[] args)
     {
         
         Bank equity = new Bank(1234567890, "John Doe", 5000);
         equity.Getbalance();
         equity.Deposit(2000);
         equity.Getbalance();
         equity.WithdrawAmount(1500);
         equity.Getbalance();
         Console.ReadKey();
     }
 }
}
```
### What is Data Hiding?
Data hiding or Information Hiding is a Process in which we hide internal data from outside the world. The purpose of data hiding is to protect the data from misuse by the outside world.

> Data Encapsulation is also called Data Hiding because by using this principle, we can hide the internal data from outside the class.

## How can we Implement Data Hiding

* By declaring the variables as private (to restrict their direct access from outside the class)
* By defining one pair of public setter and getter methods or properties to access private variables from outside the class

#### RECAP
* `public`: The public members can be accessed by any other code in the same assembly or another assembly that references it.
* `private`: The private members can be accessed only by code in the same class.
* `protected`: The protected Members in C# are available within the same class as well as to the classes that are derived from that class.
* `internal`: The internal members can be accessed by any code in the same assembly but not from another assembly.
* `protected internal`: The protected internal members can be accessed by any code in the assembly in which it’s declared or from within a derived class in another assembly.
* `private protected`: The private protected members can be accessed by types derived from the class that is declared within its containing assembly.

### Implementing Data Encapsulation/Hiding in C# using Setter and Getter Methods:

```C#
using System;
namespace EncapsulationDemo
{
    public class Bank
    {
        //Hiding class data by declaring the variable as private
        private double balance;

        //Creating public Setter and Getter methods

        //Public Getter Method
        //This method is used to return the data stored in the balance variable
        public double GetBalance()
        {
            //add validation logic if needed
            return balance;
        }

        //Public Setter Method
        //This method is used to stored the data  in the balance variable
        public void SetBalance(double balance)
        {
            // add validation logic to check whether data is correct or not
            this.balance = balance;
        }
    }
    class Program
    {
        public static void Main()
        {
            Bank bank = new Bank();
            //You cannot access the Private Variable
            //bank.balance; //Compile Time Error

            //You can access the private variable via public setter and getter methods
            bank.SetBalance(500);
            Console.WriteLine(bank.GetBalance());
            Console.ReadKey();
        }
    }
}
```

### Implementing Data Encapsulation/Hiding in C# using Properties:

The Properties are a new language feature introduced in C#. Properties in C# help in protecting a field or variable of a class by reading and writing the values to it. The first approach, i.e., `setter` and `getter` itself, is good, but Data Encapsulation in C# can be accomplished much smoother with properties.

Let us understand how to implement Data Encapsulation or Data Hiding in C# using properties with an example

```C#
using System;
namespace EncapsulationDemo
{
    public class Bank
    {
        private double _Amount;
        public double Amount
        {
            get
            {
                return _Amount;
            }
            set
            {
                // Validate the value before storing it in the _Amount variable
                if (value < 0)
                {
                    throw new Exception("Please Pass a Positive Value");
                }
                else
                {
                    _Amount = value;
                }
            }
        }
    }
    class Program
    {
        public static void Main()
        {
            try
            {
                Bank bank = new Bank();
                //We cannot access the _Amount Variable directly
                //bank._Amount = 50; //Compile Time Error
                //Console.WriteLine(bank._Amount); //Compile Time Error

                //Setting Positive Value using public Amount Property
                bank.Amount= 10;

                //Setting the Value using public Amount Property
                Console.WriteLine(bank.Amount);
                
                //Setting Negative Value
                bank.Amount = -150;
                Console.WriteLine(bank.Amount);
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }

            Console.ReadKey();
        }
    }
}
```
Output : 
```Plain Text
10
Please Pass a Positive Value
```

**Advantages of Encapsulation in C#:**

* Data protection: You can validate the data before storing it in the variable.
* Achieving Data Hiding: The user will have no idea about the inner implementation of the class.
* Security: The encapsulation Principle helps to secure our code since it ensures that other units(classes, interfaces, etc) can not access the data directly.
* Flexibility: The encapsulation Principle in C# makes our code more flexible, allowing the programmer to easily change or update the code.
* Control: The encapsulation Principle gives more control over the data stored in the variables. For example, we can control the data by validating whether the data is good enough to store in the variable.

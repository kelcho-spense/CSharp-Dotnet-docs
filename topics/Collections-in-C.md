# Collections in C#
**`Collections are nothing but groups of records that can be treated as one logical unit`**. For example, you can have a country collection that can have a country code and country name. You can have a product collection that has the Product Id and Product Name. You can have an employee collection having the employee’s name and employee id. You can have a book collection having an ISBN number and book name. For a better understanding, please have a look at the below image.

![image_157.png](image_157.png)

## General Categories of Collections:
Collections are classified into 4 types such as `Indexed Based`, `Key-Value Pair`, `Prioritized Collection`, and `Specialized Collections`. For a better understanding, please have a look at the below diagram.

![image_158.png](image_158.png)

### Indexed Base Collections:
In Indexed Based, we have two kinds of collections i.e. Array and List. To understand the Indexed Based collection, please have a look at the following country collection. So, when we add any elements to .NET Collections like Array, List, or ArrayList, it maintains its own internal index number.

![image_159.png](image_159.png)

### Key-Value Pair Collections
In the Key-Value Pair collection, we have `Hashtable`, `Dictionary`, and `SortedList`. In real-time projects, we rarely accessed the records using the internal index numbers. We generally use some kind of keys to identify the records and there we use the Key-Value Pair collections like `Hashtable`, `Dictionary`, and `SortedList`.

![image_160.png](image_160.png)

### Prioritized Collections:
The Prioritized Collections help you to access the elements in a particular sequence. The **`Stack`** and **`Queue`** collections belong to the Prioritized Collections category. If you want First in First Out (FIFO) access to the elements of a collection, then you need to use Queue collection. On the other hand, if you want Last in First Out (LIFO) access to the elements of a collection, then you need to use the Stack collection.

![image_161.png](image_161.png)

Specialized Collections:
The Specialized Collections are specifically designed for a specific purpose. For example, a Hybrid Dictionary starts as a list and then becomes a hashtable.
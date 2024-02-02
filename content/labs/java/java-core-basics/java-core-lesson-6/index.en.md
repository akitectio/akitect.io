---
categories:
  - java
date: 2023-01-30T08:00:00+08:00
description: Object-Oriented Programming (OOP) is a programming method that allows building applications based on objects, such as real-world objects in the physical world. OOP focuses on encapsulating data and related functions into a single object, making it easier to develop, maintain, and expand applications. OOP includes important concepts such as inheritance, polymorphism, abstraction, and data encapsulation, helping programmers build flexible and maintainable applications.
draft: false
featuredImage: /series/java-core-basic/java-core-lesson-6-object-oriented-programming-oop.webp
images:
  - /series/java-core-basic/java-core-lesson-6-object-oriented-programming-oop.webp
  - /java-core-lesson-6-object-oriented-programming-oop/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - java-core-basics
tags:
  - Java
  - Java core
  - java OOP
title: Java Core Lesson 6 - Object Oriented Programming (OOP)
url: /java-core-lesson-6-object-oriented-programming-oop
weight: 6
---

Object-Oriented Programming (OOP) is a programming method that allows building applications based on objects, such as real-world objects in the physical world. OOP focuses on encapsulating data and related functions into a single object, making it easier to develop, maintain, and expand applications. OOP includes important concepts such as inheritance, polymorphism, abstraction, and data encapsulation, helping programmers build flexible and maintainable applications.

### Concept

Object refers to entities in the real world, such as pens, chairs, tables, computers, clocks, etc. Object-Oriented Programming (OOP) is a method or model for designing a program using classes and objects. It simplifies software development and maintenance by providing some concepts:

- **Object**: An object is an entity in the real world, such as a pen, a chair, a table, a computer, a clock, etc. In OOP, an object is an instance of a class.
- **Class**: A class is a blueprint or template for creating objects. It contains the attributes (characteristics) and methods (behaviors) of the object.
- **Inheritance**: Inheritance is a feature that allows a new class to be created by using the attributes and methods of an existing class. The new class is called a subclass and the original class is called a superclass. Inheritance helps to increase reusability and reduce code duplication.
- **Polymorphism**: Polymorphism is the ability of an object to perform different actions in different ways. In OOP, polymorphism is achieved through inheritance and method overriding.
- **Abstraction**: Abstraction is the ability to remove unnecessary details and focus on the important characteristics of an object. In OOP, abstraction is achieved through the use of abstract classes and interfaces.
- **Encapsulation**: Encapsulation is the ability to hide the details of an object from the outside world, allowing access only to the public components. In OOP, encapsulation is achieved through the use of public methods and private attributes.

Example:

```java
// Define the Student class with the name, age, and grade attributes
public class Student {
    private String name;
    private int age;
    private double grade;

    public Student(String name, int age, double grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }

    // Method to get the name of the student
    public String getName() {
        return name;
    }

    // Method to get the age of the student
    public int getAge() {
        return age;
    }

    // Method to get the grade of the student
    public double getGrade() {
        return grade;
    }

    // Method to set the grade of the student
    public void setGrade(double grade) {
        this.grade = grade;
    }

    // Method to calculate the scholarship for the student
    public double calculateScholarship() {
        if (grade >= 9.0) {
            return 1000.0;
        } else if (grade >= 8.0) {
            return 500.0;
        } else {
            return 0.0;
        }
    }
}

// Main program using the Student class
public class Main {
    public static void main(String[] args) {
        // Create a Student object
        Student student = new Student("John Doe", 20, 8.5);

        // Print the student information
        System.out.println("Name: " + student.getName());
        System.out.println("Age: " + student.getAge());
        System.out.println("Grade: " + student.getGrade());

        // Set a new grade for the student
        student.setGrade(9.5);
        System.out.println("New grade: " + student.getGrade());

        // Calculate the scholarship for the student
        double scholarship = student.calculateScholarship();
        System.out.println("Scholarship: " + scholarship);
    }
}
```

In this example, we define a **Student** class with **name**, **age**, and **grade** attributes, and methods to get and set values for these attributes, along with a method to calculate a scholarship based on the student's grade. Then, we use the **Student** class in the main program to create a **Student** object, print out the student's information, set a new grade for the student, and calculate the scholarship for the student.

In addition to the above concepts, there are other terms used in object-oriented design:

- **Coupling**: Coupling refers to the degree of dependency between classes in a program. A weak coupling is better, as it makes the code easier to read, maintain, and reuse.
- **Cohesion**: Cohesion refers to the degree of aggregation of the components of a class or module. A module with high cohesion has components that work well together, making the code easier to understand, maintain, and reuse.
- **Association**: Association refers to the relationship between objects in a program. Association can be "has a" or "is related to" and is represented as a connection between objects.
- **Aggregation**: Aggregation is a type of association where an object contains one or more other objects as part of itself. For example, a class may include a list of objects of another class.
- **Composition**: Composition is a stronger form of aggregation, where an object exists as part of another object. This means that if the parent object no longer exists, the child object will also no longer exist.

In the example above, we have two classes: **Animal** and **Cat**. The **Cat** class is a subclass of the **Animal** class, inheriting the attributes and methods of the parent class. We also define other terms such as static method, method overriding, association, aggregation, and composition. In the makeSound() method of the **Cat** class, we override the method of the parent class to display the string "**Meow!**" instead of the default string "**The animal makes a sound.**."

### Benefits of OOP over Procedure-oriented programming

1. OOP makes development and maintenance easier, while in procedural programming languages, management will not be easy if the source code becomes larger as the project size increases.

Example:

Procedural programming:

```java
public class Main {
    public static void main(String[] args) {
        int length = 5;
        int width = 3;
        int area = length * width;
        int perimeter = 2 * (length + width);

        System.out.println("Area of rectangle: " + area);
        System.out.println("Perimeter of rectangle: " + perimeter);
    }
}
```

Object-oriented programming:

```java
public class Rectangle {
    private int length;
    private int width;

    public Rectangle(int length, int width) {
        this.length = length;
        this.width = width;
    }

    public int getArea() {
        return length * width;
    }

    public int getPerimeter() {
        return 2 * (length + width);
    }
}

public class Main {
    public static void main(String[] args) {
        Rectangle rectangle = new Rectangle(5, 3);

        System.out.println("Area of rectangle: " + rectangle.getArea());
        System.out.println("Perimeter of rectangle: " + rectangle.getPerimeter());
    }
}
```

In this example, object-oriented programming uses the **Rectangle** object to store the properties of the rectangle and methods to calculate the area and perimeter. Using objects makes the source code easier to read, maintain, and reuse.

2. OOP provides data hiding, while in procedural programming languages, global data can be accessed from anywhere.

We can understand more about data hiding in OOP with the following example:

```java
public class BankAccount {
    private double balance;

    public void deposit(double amount) {
        balance += amount;
    }

    public void withdraw(double amount) {
        if (balance >= amount) {
            balance -= amount;
        } else {
            System.out.println("Insufficient balance!");
        }
    }

    public double getBalance() {
        return balance;
    }
}
```

In this example, we have a **BankAccount** class representing a bank account. The balance variable is defined as a private data variable, hidden inside the class. This means that other objects cannot directly access or change the value of this variable.

Instead, the **deposit**, **withdraw**, and **getBalance** methods are provided to allow other objects to interact with the **balance** variable safely and in a controlled manner. For example, the **deposit** method is used to add money to the account and the **withdraw** method is used to withdraw money from the account, but only if the balance in the account is sufficient.

Therefore, data hiding helps protect our data from unauthorized and incorrect operations.

3. OOP provides the ability to simulate real-world events more efficiently. If an object-oriented programming language is used, we can provide solutions to real-world problems.

An example of using OOP to simulate real-world events is building a library management application. In this application, we can define a **Book** class with properties such as book name, author, publisher, price, etc. and methods such as borrowing books, returning books, deleting books, etc. We can use this class to manage books in the library.

In addition, we can also define a **Member** class to manage information of members in the library such as name, address, phone number, etc. and methods to register, cancel registration, search, etc. We can use this class to manage members of the library.

Finally, we can define a **Library** class to manage books and members in the library. This class has methods such as borrowing books, returning books, searching for books, etc. We can use this class to perform library management operations.

Here is an example Java code for the **Book** class:

```java
public class Book {
   private String title;
   private String author;
   private String publisher;
   private double price;

   public Book(String title, String author, String publisher, double price) {
      this.title = title;
      this.author = author;
      this.publisher = publisher;
      this.price = price;
   }

   public String getTitle() {
      return title;
   }

   public void setTitle(String title) {
      this.title = title;
   }

   public String getAuthor() {
      return author;
   }

   public void setAuthor(String author) {
      this.author = author;
   }

   public String getPublisher() {
      return publisher;
   }

   public void setPublisher(String publisher) {
      this.publisher = publisher;
   }

   public double getPrice() {
      return price;
   }

   public void setPrice(double price) {
      this.price = price;
   }

   public void borrowBook() {
      // code to borrow book
   }

   public void returnBook() {
      // code to return book
   }

   public void deleteBook() {
      // code to delete book
   }
}
```

Here, we have defined the Book class with properties such as **title**, **author**, **publisher**, **price** and methods such as **borrowBook**, etc.

### Constructor

In object-oriented programming, a constructor is a special method called to initialize a new object of a class. The constructor has the same name as the class and is used to set the initial values of the object's properties. When creating a new object of a class, the constructor is called immediately to initialize the object and return that object.

Constructors can have parameters or no parameters, depending on the requirements of the class. If the class does not define a constructor, a default constructor will be automatically created by Java to initialize the object.

Some rules when defining constructors:

- The constructor must have the same name as the class.
- The constructor has no return type.
- The constructor can have or not have parameters.
- The constructor can be called from another constructor in the same class using the "this" keyword.
- The constructor can call the constructor of the superclass using the "super" keyword.

Example:

```java
public class Person {
    private String name;
    private int age;

    // no-argument constructor
    public Person() {
        name = "Unknown";
        age = 0;
    }

    // parameterized constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // getter and setter
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

In this example, the Person class has two constructors: a no-argument constructor and a two-argument constructor with name and age. The no-argument constructor is used to initialize a Person object with the default values of name and age as "Unknown" and 0. The parameterized constructor is used to initialize a Person object with the values of name and age passed in.

### Access Modifier

In object-oriented programming, access modifier is an important concept to manage the access rights of components in a class such as properties, methods, and subclasses. Java provides 4 different types of access modifiers:

- **Public**: Allows access from anywhere, even from classes in different packages.
- **Private**: Only allows access from within the same class, not from outside.
- **Protected**: Allows access from within the same class and its subclasses, as well as classes in the same package.
- **Default**: Allows access from within the same class and classes in the same package, but not from outside the package.

> Using appropriate access modifiers will help manage the access rights of components in the class tightly, protect data, and ensure the security of the application.

Example:

```java
public class MyClass {
    public int x; // accessible from anywhere
    protected int y; // accessible within the class and its subclasses
    int z; // accessible within the package
    private int w; // accessible only within the class
}
```

In this example, **x** has a **public** access modifier so it can be accessed from anywhere in the program. **y** has a **protected** access modifier so it can only be accessed from within the MyClass or its subclasses. **z** has a default access modifier (no **public**, **protected**, or **private** keyword) so it can only be accessed from within the same **package**. **w** has a **private** access modifier so it can only be accessed from within the MyClass.

### Conclusion

In this article, we have learned about object-oriented programming (OOP) and its basic concepts including object, class, inheritance, polymorphism, abstraction, and encapsulation. We have also explored other terms such as coupling, cohesion, aggregation, and association.

Furthermore, we have compared OOP with procedural programming languages and found that OOP has many advantages, especially in developing and maintaining complex projects.

Finally, we have looked at an example of OOP in Java to understand how OOP is applied in practice.

With OOP, we can design software programs in an easily understandable and maintainable way, while also providing the ability to simulate real-world events. Therefore, mastering the concepts and skills of object-oriented programming is very important for any programmer.

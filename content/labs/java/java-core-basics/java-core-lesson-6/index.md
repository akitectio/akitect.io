---
categories:
  - java
date: 2023-01-30T08:00:00+08:00
description: Object-Oriented Programming (OOP) là một phương pháp lập trình cho phép xây dựng các ứng dụng dựa trên các đối tượng, như là đối tượng thực tế trong thế giới thực. OOP tập trung vào việc đóng gói dữ liệu và chức năng liên quan vào một đối tượng đơn lẻ, giúp cho việc phát triển, bảo trì và mở rộng ứng dụng trở nên dễ dàng hơn. OOP bao gồm các khái niệm quan trọng như kế thừa, đa hình, trừu tượng hóa và đóng gói dữ liệu, giúp cho các lập trình viên xây dựng ứng dụng có tính linh hoạt và dễ bảo trì.
draft: false
featuredImage: /series/java-core-basic/java-core-lesson-6-object-oriented-programming-oop.webp
images:
  - /series/java-core-basic/java-core-lesson-6-object-oriented-programming-oop.webp
  - /java-core-lesson-6-object-oriented-programming-oop/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - java-core-basics
tags:
  - Java
  - Java core
  - java OOP
title: Java Core Lesson 6 - Lập trình hướng đối tượng (OOP - Object Oriented Programming)
url: /java-core-lesson-6-lap-trinh-huong-doi-tuong-oop-object-oriented-programming
weight: 6
---

Object-Oriented Programming (OOP) là một phương pháp lập trình cho phép xây dựng các ứng dụng dựa trên các đối tượng, như là đối tượng thực tế trong thế giới thực. OOP tập trung vào việc đóng gói dữ liệu và chức năng liên quan vào một đối tượng đơn lẻ, giúp cho việc phát triển, bảo trì và mở rộng ứng dụng trở nên dễ dàng hơn. OOP bao gồm các khái niệm quan trọng như kế thừa, đa hình, trừu tượng hóa và đóng gói dữ liệu, giúp cho các lập trình viên xây dựng ứng dụng có tính linh hoạt và dễ bảo trì.

### Khái niệm

Đối tượng có nghĩa là thực thể trong thế giới thực, chẳng hạn như bút, ghế, bàn, máy tính, đồng hồ, v.v. Lập trình Hướng đối tượng là một phương pháp hoặc mô hình để thiết kế một chương trình bằng cách sử dụng lớp và đối tượng. Nó đơn giản hóa việc phát triển và bảo trì phần mềm bằng cách cung cấp một số khái niệm:

- **Object**: Đối tượng là một thực thể trong thế giới thực, ví dụ như một bút, một chiếc ghế, một bàn, một máy tính, một chiếc đồng hồ, vv. Trong lập trình hướng đối tượng, đối tượng là một thể hiện của một lớp.
- **Class**: Lớp là một bản thiết kế hoặc mẫu cho việc tạo ra các đối tượng. Nó chứa các thuộc tính (đặc điểm) và phương thức (hành vi) của đối tượng.
- **Inheritance**: Kế thừa là một tính năng cho phép lớp mới được tạo ra bằng cách sử dụng các thuộc tính và phương thức của một lớp hiện có. Lớp mới được gọi là lớp con và lớp ban đầu được gọi là lớp cha. Tính kế thừa giúp tăng tính tái sử dụng và giảm lượng mã lặp lại.
- **Polymorphism**: Đa hình là khả năng của một đối tượng để thực hiện các hành động khác nhau theo cách khác nhau. Trong lập trình hướng đối tượng, đa hình được đạt được thông qua việc sử dụng kế thừa và ghi đè phương thức.
- **Abstraction**: Trừu tượng là khả năng để loại bỏ chi tiết không cần thiết và tập trung vào tính chất quan trọng của đối tượng. Trong lập trình hướng đối tượng, trừu tượng được đạt được thông qua việc sử dụng các lớp trừu tượng và giao diện.
- **Encapsulation**: Bao đóng là khả năng để ẩn các chi tiết của một đối tượng khỏi thế giới bên ngoài, chỉ cho phép truy cập đến các thành phần được công khai. Trong lập trình hướng đối tượng, đóng gói được đạt được thông qua việc sử dụng các phương thức công khai và các thuộc tính riêng tư.

Ví dụ:

```java
// Định nghĩa lớp Student với các thuộc tính name, age, và grade
public class Student {
    private String name;
    private int age;
    private double grade;

    public Student(String name, int age, double grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }

    // Phương thức lấy tên của sinh viên
    public String getName() {
        return name;
    }

    // Phương thức lấy tuổi của sinh viên
    public int getAge() {
        return age;
    }

    // Phương thức lấy điểm số của sinh viên
    public double getGrade() {
        return grade;
    }

    // Phương thức thiết lập điểm số của sinh viên
    public void setGrade(double grade) {
        this.grade = grade;
    }

    // Phương thức tính toán học bổng cho sinh viên
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

// Chương trình chính sử dụng lớp Student
public class Main {
    public static void main(String[] args) {
        // Tạo đối tượng Student
        Student student = new Student("John Doe", 20, 8.5);

        // In ra thông tin sinh viên
        System.out.println("Name: " + student.getName());
        System.out.println("Age: " + student.getAge());
        System.out.println("Grade: " + student.getGrade());

        // Thiết lập điểm số mới cho sinh viên
        student.setGrade(9.5);
        System.out.println("New grade: " + student.getGrade());

        // Tính toán học bổng cho sinh viên
        double scholarship = student.calculateScholarship();
        System.out.println("Scholarship: " + scholarship);
    }
}
```

Trong ví dụ này, chúng ta định nghĩa lớp **Student** với các thuộc tính **name**, **age**, và **grade**, và các phương thức để lấy và thiết lập giá trị cho các thuộc tính này, cùng với một phương thức để tính toán học bổng dựa trên điểm số của sinh viên. Sau đó, chúng ta sử dụng lớp **Student** trong chương trình chính để tạo một đối tượng **Student**, in ra thông tin của sinh viên, thiết lập một điểm số mới cho sinh viên, và tính toán học bổng cho sinh viên.

Ngoài các khái niệm trên, còn có một số thuật ngữ khác được sử dụng trong thiết kế hướng đối tượng:

- **Coupling (Liên kết)**: Liên kết đề cập đến mức độ phụ thuộc giữa các lớp trong một chương trình. Một liên kết yếu là tốt hơn, vì nó làm cho mã dễ đọc, bảo trì và tái sử dụng hơn.
- **Cohesion (Cohesion)**: Cohesion đề cập đến mức độ tổng hợp của các thành phần của một lớp hoặc một module. Một module có cohesion cao có các thành phần hoạt động tốt với nhau, làm cho mã dễ hiểu, bảo trì và tái sử dụng hơn.
- **Association (Liên kết)**: Liên kết đề cập đến mối quan hệ giữa các đối tượng trong một chương trình. Association có thể được coi là "có một" hoặc "liên quan đến" và được thể hiện dưới dạng đường kết nối giữa các đối tượng.
- **Aggregation (Tổng hợp**): Aggregation là một dạng liên kết mà một đối tượng chứa một hay nhiều đối tượng khác như một phần của nó. Ví dụ, một lớp có thể bao gồm một danh sách các đối tượng của một lớp khác.
- **Composition (Tổng hợp):** Composition là một dạng liên kết mạnh hơn so với aggregation, trong đó một đối tượng tồn tại như một phần của một đối tượng khác. Nghĩa là, nếu đối tượng chủ không còn tồn tại thì đối tượng con cũng sẽ không còn tồn tại.

Ví dụ:

```java
// Định nghĩa lớp Animal
public class Animal {
    // Thuộc tính
    private String name;
    private int age;

    // Phương thức khởi tạo
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Phương thức getter
    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    // Phương thức tĩnh
    public static void makeSound() {
        System.out.println("The animal makes a sound.");
    }
}

// Định nghĩa lớp Cat, kế thừa từ lớp Animal
public class Cat extends Animal {
    // Thuộc tính
    private String color;

    // Phương thức khởi tạo
    public Cat(String name, int age, String color) {
        super(name, age);
        this.color = color;
    }

    // Phương thức getter
    public String getColor() {
        return color;
    }

    // Ghi đè phương thức của lớp cha
    @Override
    public static void makeSound() {
        System.out.println("Meow!");
    }
}

// Sử dụng các lớp trên
public class Main {
    public static void main(String[] args) {
        // Tạo đối tượng Cat
        Cat myCat = new Cat("Garfield", 5, "orange");

        // Gọi phương thức của lớp Animal
        System.out.println(myCat.getName()); // Output: Garfield
        System.out.println(myCat.getAge()); // Output: 5

        // Gọi phương thức của lớp Cat
        System.out.println(myCat.getColor()); // Output: orange

        // Gọi phương thức tĩnh của lớp Animal
        Animal.makeSound(); // Output: The animal makes a sound.

        // Gọi phương thức tĩnh của lớp Cat (ghi đè phương thức của lớp cha)
        Cat.makeSound(); // Output: Meow!
    }
}
```

Trong ví dụ này, chúng ta có hai lớp: **Animal** và **Cat**. Lớp **Cat** là lớp con của lớp **Animal**, kế thừa các thuộc tính và phương thức của lớp cha. Chúng ta cũng định nghĩa các thuật ngữ khác như phương thức tĩnh, ghi đè phương thức, liên kết, tổng hợp và tổng hợp mạnh. Trong phương thức makeSound() của lớp Cat, chúng ta ghi đè phương thức của lớp cha để hiển thị chuỗi "**Meow!**" thay vì chuỗi mặc định "**The animal makes a sound.**."

### Lợi ích của OOP so với ngôn ngữ lập trình thủ tục (Procedure-oriented)

1. OOP làm cho việc phát triển và bảo trì dễ dàng hơn, trong khi đó trong ngôn ngữ lập trình hướng thủ tục, việc quản lý sẽ không dễ dàng nếu mã nguồn trở nên lớn hơn khi kích thước dự án tăng lên.

Ví dụ:

Lập trình hướng thủ tục:

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

Lập trình hướng đối tượng:

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

Trong ví dụ này, lập trình hướng đối tượng sử dụng đối tượng **Rectangle** để lưu trữ các thuộc tính của hình chữ nhật và các phương thức để tính toán diện tích và chu vi. Việc sử dụng đối tượng giúp mã nguồn trở nên dễ đọc, bảo trì và tái sử dụng hơn.

2. OOP cung cấp tính che dấu dữ liệu, trong khi đó trong ngôn ngữ lập trình hướng thủ tục, dữ liệu toàn cục có thể được truy cập từ bất cứ đâu.

Chúng ta có thể hiểu rõ hơn về tính che dấu dữ liệu trong OOP bằng ví dụ sau:

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

Trong ví dụ này, chúng ta có một lớp **BankAccount** đại diện cho tài khoản ngân hàng. Biến balance được định nghĩa là một biến dữ liệu riêng tư, được che dấu bên trong lớp. Điều này có nghĩa là các đối tượng khác không thể trực tiếp truy cập hoặc thay đổi giá trị của biến này.

Thay vào đó, các phương thức **deposit**, **withdraw**, và **getBalance** được cung cấp để cho phép các đối tượng khác tương tác với biến **balance** một cách an toàn và kiểm soát. Ví dụ, phương thức **deposit** được sử dụng để thêm tiền vào tài khoản và phương thức **withdraw** được sử dụng để rút tiền ra khỏi tài khoản, nhưng chỉ khi số dư trong tài khoản đủ lớn.

Do đó, tính che dấu dữ liệu giúp bảo vệ dữ liệu của chúng ta khỏi các thao tác không đúng và không được ủy quyền.

3. OOP cung cấp khả năng mô phỏng sự kiện thế giới thực hiện hiệu quả hơn. Nếu sử dụng ngôn ngữ lập trình hướng đối tượng, chúng ta có thể cung cấp giải pháp cho các vấn đề thực tế.

Ví dụ về việc sử dụng OOP để mô phỏng sự kiện thực tế là việc xây dựng một ứng dụng quản lý thư viện. Trong ứng dụng này, chúng ta có thể định nghĩa một lớp Book (Sách) với các thuộc tính như tên sách, tác giả, nhà xuất bản, giá, v.v. và các phương thức như mượn sách, trả sách, xóa sách, v.v. Chúng ta có thể sử dụng lớp này để quản lý các cuốn sách trong thư viện.

Ngoài ra, chúng ta còn có thể định nghĩa lớp Member (Thành viên) để quản lý thông tin của các thành viên trong thư viện như tên, địa chỉ, số điện thoại, v.v. và các phương thức để đăng ký, hủy đăng ký, tìm kiếm, v.v. Chúng ta có thể sử dụng lớp này để quản lý các thành viên của thư viện.

Cuối cùng, chúng ta có thể định nghĩa lớp Library (Thư viện) để quản lý các cuốn sách và thành viên trong thư viện. Lớp này có các phương thức như mượn sách, trả sách, tìm kiếm sách, v.v. Chúng ta có thể sử dụng lớp này để thực hiện các thao tác quản lý thư viện.

Dưới đây là ví dụ code Java cho lớp Book:

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

Ở đây, chúng ta đã định nghĩa lớp Book với các thuộc tính như **tiêu đề (title)**, **tác giả (author)**, **nhà xuất bản (publisher)**, **giá (price)** và các phương thức như **mượn sách (borrowBook),** vvv ...

### Constructor

> Trong lập trình hướng đối tượng, constructor là một phương thức đặc biệt được gọi để khởi tạo một đối tượng mới của một lớp. Constructor có tên giống như tên lớp và được sử dụng để thiết lập giá trị ban đầu của các thuộc tính của đối tượng. Khi tạo một đối tượng mới của lớp, constructor sẽ được gọi ngay lập tức để khởi tạo đối tượng và trả về đối tượng đó.

Constructor có thể có tham số hoặc không có tham số, tùy thuộc vào yêu cầu của lớp. Nếu lớp không có constructor được định nghĩa thì một constructor mặc định sẽ được tạo tự động bởi Java để khởi tạo đối tượng.

Một số quy tắc khi định nghĩa constructor:

- Constructor phải có tên giống với tên lớp.
- Constructor không có kiểu trả về.
- Constructor có thể có hoặc không có tham số.
- Constructor có thể được gọi từ constructor khác trong cùng lớp sử dụng từ khóa "this".
- Constructor có thể gọi constructor của lớp cha sử dụng từ khóa "super".

Ví dụ:

```java
public class Person {
    private String name;
    private int age;

    // constructor không tham số
    public Person() {
        name = "Unknown";
        age = 0;
    }

    // constructor có tham số
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // getter và setter
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

Trong ví dụ này, lớp Person có hai constructor: một constructor không tham số và một constructor có hai tham số là name và age. Constructor không tham số được sử dụng để khởi tạo một đối tượng Person với giá trị mặc định của name và age là "Unknown" và 0. Constructor có tham số được sử dụng để khởi tạo một đối tượng Person với giá trị name và age được truyền vào.

### Phạm vi truy cập (Access Modifier)

Trong lập trình hướng đối tượng, phạm vi truy cập (access modifier) là một khái niệm quan trọng để quản lý quyền truy cập của các thành phần trong lớp (class) như thuộc tính (property), phương thức (method) và lớp con (subclass). Java cung cấp 4 loại phạm vi truy cập khác nhau:

- **Public**: Cho phép truy cập từ bất kỳ đâu, ngay cả từ các lớp khác trong các package khác nhau.
- **Private**: Chỉ cho phép truy cập từ trong cùng lớp đó, không cho phép truy cập từ bên ngoài.
- **Protected**: Cho phép truy cập từ trong cùng lớp đó và các lớp con, cũng như các lớp ở trong cùng package.
- **Default**: Cho phép truy cập từ trong cùng lớp đó và các lớp ở trong cùng package, nhưng không cho phép truy cập từ bên ngoài package.

> Việc sử dụng phạm vi truy cập phù hợp sẽ giúp quản lý quyền truy cập của các thành phần trong lớp một cách chặt chẽ, bảo vệ dữ liệu và đảm bảo tính bảo mật của ứng dụng.

Ví dụ:

```java
public class MyClass {
    public int x; // accessible from anywhere
    protected int y; // accessible within the class and its subclasses
    int z; // accessible within the package
    private int w; // accessible only within the class
}
```

Trong ví dụ này, **x** có phạm vi truy cập **public** nên có thể truy cập từ bất kỳ đâu trong chương trình. y có phạm vi truy cập **protected** nên chỉ có thể truy cập từ bên trong lớp **MyClass** hoặc các lớp con của nó. **z** có phạm vi truy cập mặc định (không có từ khóa **public**, **protected** hoặc **private**) nên chỉ có thể truy cập từ bên trong cùng **package**. **w** có phạm vi truy cập **private** nên chỉ có thể truy cập từ bên trong lớp **MyClass**.

### Kết luận

Trong bài viết này, chúng ta đã tìm hiểu về lập trình hướng đối tượng (OOP) và các khái niệm cơ bản của nó bao gồm đối tượng, lớp, kế thừa, đa hình, trừu tượng và đóng gói. Chúng ta cũng đã khám phá các thuật ngữ khác như liên kết, cohesions, tổng hợp và liên kết.

Ngoài ra, chúng ta đã so sánh OOP với ngôn ngữ lập trình hướng thủ tục và thấy rằng OOP có nhiều ưu điểm hơn, đặc biệt là trong việc phát triển và bảo trì các dự án phức tạp.

Cuối cùng, chúng ta cũng đã xem xét ví dụ về OOP trong Java để hiểu cách OOP được áp dụng trong thực tế.

Với OOP, chúng ta có thể thiết kế các chương trình phần mềm theo cách dễ dàng hiểu và bảo trì, đồng thời cũng cung cấp khả năng mô phỏng các sự kiện thế giới thực. Vì vậy, việc nắm vững các khái niệm và kỹ năng lập trình hướng đối tượng là rất quan trọng cho bất kỳ lập trình viên nào.

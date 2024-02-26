---
categories:
  - database
date: 2023-11-19T08:00:00+08:00
description: PostgreSQL so với cơ sở dữ liệu khác như thế nào?
draft: false
series:
  - postgresql-dba-series
featuredImage: /series/database/postgresql-dba-03-object-model-trong-postgresql.webp
images:
  - /postgresql-dba-03-object-model-trong-postgresql/images/index.png
  - /series/database/postgresql-dba-03-object-model-trong-postgresql.webp
license: <a rel=***license external nofollow noopener noreffer*** href=***https://creativecommons.org/licenses/by-nc/4.0/*** target=***_blank***>CC BY-NC 4.0</a>
tags:
  - Database
  - PostgreSQL
  - RDBMS Concepts
  - Object Model
  - ORDBMS
  - Object Model PostgreSQL
  - PostgreSQL DBA
title: Lesson 3 - PostgreSQL Object Model
url: /postgresql-dba-03-object-model-trong-postgresql
weight: 3
---

## Object Model là gì?

PostgreSQL là hệ thống quản lý cơ sở dữ liệu đối tượng-quan hệ (ORDBMS). Điều đó có nghĩa là nó kết hợp các tính năng của cả cơ sở dữ liệu quan hệ (RDBMS) và cơ sở dữ liệu hướng đối tượng (OODBMS). Mô hình đối tượng trong PostgreSQL cung cấp các tính năng như kiểu dữ liệu do người dùng định nghĩa, kế thừa, và đa hình, làm tăng khả năng của nó hơn so với một RDBMS dựa trên SQL thông thường.

### Cơ sở dữ liệu (Database)

Một cơ sở dữ liệu (Database) là một tập hợp các dữ liệu có tổ chức được lưu trữ và quản lý để cung cấp thông tin hoặc hỗ trợ các hoạt động kinh doanh cụ thể. Cơ sở dữ liệu thường được sử dụng để lưu trữ, truy xuất và quản lý dữ liệu một cách hiệu quả.

#### Tạo cơ sở dữ liệu

Để tạo cơ sở dữ liệu, bạn có thể sử dụng câu lệnh SQL **_CREATE DATABASE_** hoặc tận dụng các tiện ích PostgreSQL như createb. Đây là một ví dụ về câu lệnh SQL **_CREATE DATABASE_**:

```sql
CREATE DATABASE QUANLYNHANSU;
```

Như vậy đã tạo thành công DB **_QUANLYNHANSU_**.

#### Quản lý cơ sở dữ liệu

PostgreSQL cung cấp một số lệnh SQL và tiện ích để quản lý cơ sở dữ liệu, bao gồm:

1. Liệt kê cơ sở dữ liệu: Sử dụng lệnh **_\l_** trong giao diện dòng lệnh **_psql_**, hoặc thực thi câu lệnh SQL **_SELECT name FROM QUANLYNHANSU_**;.

2. Chuyển đổi cơ sở dữ liệu: Sử dụng lệnh **_\connect_** hoặc **_\c_**, sau đó là tên cơ sở dữ liệu trong giao diện dòng lệnh **\*psql**.

3. Đổi tên cơ sở dữ liệu: Sử dụng câu lệnh SQL **_ALTER DATABASE QUANLYNHANSU RENAME TO QUANLYNHANSU_NEW_**;.

4. Xóa cơ sở dữ liệu: Sử dụng câu lệnh SQL **_DROP DATABASE QUANLYNHANSU_NEW_**; hoặc tiện ích **_dropdb_**. Hãy cẩn thận khi xóa cơ sở dữ liệu, vì nó sẽ xóa vĩnh viễn tất cả dữ liệu và đối tượng của nó.

#### Thuộc tính cơ sở dữ liệu

Mỗi cơ sở dữ liệu PostgreSQL có một số thuộc tính bạn có thể cấu hình để điều chỉnh hành vi và hiệu suất của nó, chẳng hạn như:

1. **_Encoding (Mã hóa)_**: Xác định mã hóa ký tự được sử dụng trong cơ sở dữ liệu. Mặc định, PostgreSQL sử dụng mã hóa giống với hệ điều hành máy chủ (ví dụ: UTF-8 trên hầu hết các hệ thống dựa trên Unix).
2. **_Collation (Thứ tự sắp xếp)_**: Xác định quy tắc sắp xếp chuỗi trong cơ sở dữ liệu. Mặc định, PostgreSQL sử dụng quy tắc sắp xếp mặc định của hệ điều hành máy chủ.
3. **_Tablespaces (Không gian bảng)_**: Điều khiển nơi lưu trữ các tệp cơ sở dữ liệu trên hệ thống tệp. Mặc định, PostgreSQL sử dụng không gian bảng mặc định của máy chủ. Bạn có thể tạo thêm không gian bảng để lưu trữ dữ liệu trên các đĩa hoặc hệ thống tệp khác nhau, để cải thiện hiệu suất hoặc mục đích sao lưu.

### Bảng (Table)

Trong PostgreSQL, "Tables" là cấu trúc dữ liệu cơ bản được sử dụng để lưu trữ và quản lý dữ liệu. Một bảng trong PostgreSQL là một tập hợp các hàng và cột, nơi hàng đại diện cho các bản ghi dữ liệu, và mỗi cột chứa dữ liệu của một trường cụ thể.

**_Ví dụ_** :

1. Tạo Bảng

```sql
CREATE TABLE employees (
   id SERIAL PRIMARY KEY,
   name VARCHAR(100),
   position VARCHAR(50),
   salary DECIMAL(10, 2),
   hire_date DATE
);
```

Trong ví dụ này, một bảng employees được tạo với các cột **_id_**, **_name_**, **_position_**, **_salary_**, và **_hire_date_**. 2. Thêm dữ liệu vào bảng

```sql
INSERT INTO employees (name, position, salary, hire_date)
VALUES ('John Doe', 'Software Engineer', 75000.00, '2021-01-15');
```

Đoạn mã này thêm một hàng vào bảng **_employees_**.

3. Truy vấn dữ liệu từ bảng

```sql
SELECT * FROM employees;
```

Đoạn mã này truy vấn tất cả các hàng từ bảng **_employees_**.

### Kiểu dữ liệu do người dùng định nghĩa (User-Defined Data Types)

User-Defined Data Types (Kiểu dữ liệu do người dùng định nghĩa) trong cơ sở dữ liệu là một tính năng cho phép người dùng tạo ra các kiểu dữ liệu tùy chỉnh dựa trên nhu cầu cụ thể của họ. Thay vì sử dụng các kiểu dữ liệu tiêu chuẩn có sẵn trong hệ thống quản lý cơ sở dữ liệu (DBMS), người dùng có thể định nghĩa các kiểu dữ liệu riêng biệt và phức tạp hơn để lưu trữ dữ liệu theo cách mà họ mong muốn.

Các ứng dụng của User-Defined Data Types bao gồm:

1. Tạo kiểu dữ liệu phù hợp với mô hình dữ liệu của ứng dụng cụ thể.
2. Giảm sự phụ thuộc vào kiểu dữ liệu tiêu chuẩn và tạo ra các kiểu dữ liệu tùy chỉnh giúp quản lý dữ liệu hiệu quả hơn.
3. Tăng tính rõ ràng và tự mô tả của cơ sở dữ liệu bằng cách sử dụng tên kiểu dữ liệu tùy chỉnh có ý nghĩa trong mô hình dữ liệu.

Ví dụ: Giả sử bạn đang phát triển một ứng dụng quản lý thư viện và muốn lưu trữ thông tin về sách trong cơ sở dữ liệu. Thay vì sử dụng các kiểu dữ liệu tiêu chuẩn như chuỗi (string) hoặc số (integer) cho các trường thông tin của sách, bạn có thể tạo một kiểu dữ liệu tùy chỉnh **_Book_** bao gồm các trường sau:

- Title (Tiêu đề sách): Kiểu dữ liệu chuỗi (string).
- Author (Tác giả): Kiểu dữ liệu chuỗi (string).
- PublicationYear (Năm xuất bản): Kiểu dữ liệu số nguyên (integer).
- ISBN (Mã số sách): Kiểu dữ liệu chuỗi (string).
- Genre (Thể loại): Kiểu dữ liệu chuỗi (string).

Bạn có thể định nghĩa kiểu dữ liệu **_Book_** như sau trong PostgreSQL:

```sql
CREATE TYPE Book AS (
    Title       text,
    Author      text,
    PublicationYear integer,
    ISBN        text,
    Genre       text
);
```

Sau đó, khi bạn tạo bảng lưu trữ thông tin về sách, bạn có thể sử dụng kiểu dữ liệu **_Book_** cho một trường cụ thể:

```sql
CREATE TABLE Library (
    BookID serial PRIMARY KEY,
    BookInfo Book
);
```

Khi thêm dữ liệu vào bảng Library, bạn có thể sử dụng kiểu dữ liệu **_Book_** để biểu diễn thông tin về sách một cách tự nhiên và rõ ràng:

```sql
INSERT INTO Library (BookInfo)
VALUES (
    ('The Great Gatsby', 'F. Scott Fitzgerald', 1925, '978-0-7432-7356-5', 'Novel')
);
```

Sử dụng User-Defined Data Types giúp làm cho cơ sở dữ liệu của bạn dễ đọc và dễ quản lý hơn, đồng thời cung cấp tính nhất quán trong mô hình dữ liệu của bạn.

### Kế thừa (Inheritance)

Trong PostgreSQL, **_Inheritance_** (Kế thừa) là một tính năng cho phép bạn tạo ra một cấu trúc dữ liệu mới bằng cách kế thừa từ một bảng hoặc kiểu dữ liệu hiện có. Tính năng này cho phép bạn tạo một sự liên kết giữa các bảng hoặc kiểu dữ liệu, cho phép chia sẻ thông tin chung và ưu điểm hóa lại cấu trúc dữ liệu.

Ví dụ, hãy xem xét một tình huống trong đó bạn có một ứng dụng quản lý dự án và bạn muốn theo dõi thông tin về các loại công việc khác nhau như **_Công việc phát triển_**, **_Công việc kiểm thử_**, và **_Công việc triển khai_**. Mỗi loại công việc này có một số thông tin riêng biệt, nhưng cũng chia sẻ một số thông tin chung, ví dụ như **_Tên công việc_**, **_Mô tả công việc_**, **_Người thực hiện_**, và **_Thời hạn hoàn thành_**.

Bạn có thể sử dụng tính năng kế thừa để tạo một kiểu dữ liệu gốc là **_Công việc_** (Job) và sau đó tạo các kiểu dữ liệu con cho từng loại công việc:

```sql
-- Tạo kiểu dữ liệu gốc ***Công việc*** (Job)
CREATE TABLE Job (
    JobID serial PRIMARY KEY,
    Title text,
    Description text,
    Assignee text,
    Deadline date
);

-- Tạo kiểu dữ liệu con ***Công việc phát triển*** kế thừa từ ***Công việc*** (Job)
CREATE TABLE DevelopmentJob (
    TechStack text,
    EstimatedHours integer
) INHERITS (Job);

-- Tạo kiểu dữ liệu con ***Công việc kiểm thử*** kế thừa từ ***Công việc*** (Job)
CREATE TABLE TestingJob (
    TestCases text[],
    TestEnvironment text
) INHERITS (Job);

-- Tạo kiểu dữ liệu con ***Công việc triển khai*** kế thừa từ ***Công việc*** (Job)
CREATE TABLE DeploymentJob (
    TargetServer text,
    DeploymentScript text
) INHERITS (Job);
```

Khi bạn sử dụng tính năng kế thừa này, bạn có thể thêm dữ liệu cho từng loại công việc một cách độc lập và truy vấn thông tin về tất cả các công việc dưới dạng **_Công việc_** gốc. Bất kỳ thay đổi nào bạn thực hiện trong kiểu dữ liệu gốc **_Công việc_** cũng sẽ ảnh hưởng đến tất cả các kiểu dữ liệu con.

Ví dụ, bạn có thể thêm một công việc phát triển như sau:

```sql
INSERT INTO DevelopmentJob (Title, Description, Assignee, Deadline, TechStack, EstimatedHours)
VALUES ('Phát triển tính năng A', 'Phát triển tính năng A cho ứng dụng', 'John Doe', '2023-12-31', 'React', 40);
```

Điều này sẽ tạo một bản ghi mới trong bảng **_Công việc_** (Job) và bảng **_Công việc phát triển_** (DevelopmentJob) cùng lúc.

### Đa hình (Polymorphism)

Đa hình (Polymorphism) là khả năng của một đối tượng dữ liệu hoặc một hàm để thực hiện nhiều hành động khác nhau dựa trên loại đối tượng hoặc tham số đầu vào. Điều này có nghĩa là bạn có thể sử dụng cùng một phương thức hoặc hàm để thao tác với các đối tượng khác nhau một cách tự nhiên, bất kể loại đối tượng.

Ví dụ, hãy xem xét một tình huống trong đó bạn có một cơ sở dữ liệu lưu trữ thông tin về các loại hình động vật, bao gồm **_Chó_** và **_Mèo_**. Mỗi loại động vật có một số thuộc tính riêng như **_Tên_**, **_Tuổi_**, và **_Âm thanh tiếng kêu_**. Bạn muốn thực hiện một hàm để phát ra tiếng kêu của động vật mà không cần biết cụ thể là **_Chó_** hay **_Mèo_**.

Trong ngôn ngữ SQL, bạn có thể sử dụng đa hình để thực hiện điều này. Dưới đây là một ví dụ về cách bạn có thể thực hiện đa hình trong PostgreSQL:

```sql
-- Tạo bảng chứa thông tin về động vật
CREATE TABLE Animals (
    AnimalID serial PRIMARY KEY,
    Name text,
    Age integer,
    Sound text
);

-- Tạo hai bản ghi cho Chó và Mèo
INSERT INTO Animals (Name, Age, Sound)
VALUES ('Fido', 3, 'Woof');

INSERT INTO Animals (Name, Age, Sound)
VALUES ('Whiskers', 2, 'Meow');

-- Tạo hàm đa hình để phát ra tiếng kêu của động vật
CREATE OR REPLACE FUNCTION MakeSound(animal Animals) RETURNS text AS $$
BEGIN
    RETURN animal.Sound;
END;
$$ LANGUAGE plpgsql;

```

Trong ví dụ này, chúng ta đã tạo một bảng **_Animals_** để lưu trữ thông tin về động vật và thêm hai bản ghi cho **_Chó_** và **_Mèo_**. Sau đó, chúng ta đã tạo một hàm **_MakeSound_** có thể thực hiện đa hình trên đối tượng **_Animals_**. Hàm này trả về tiếng kêu của động vật, và nó có thể được sử dụng với bất kỳ đối tượng **_Animals_** nào mà không cần biết loại cụ thể của động vật đó.

Ví dụ sử dụng hàm **_MakeSound_** với cả hai động vật **_Chó_** và **_Mèo_**:

```sql
SELECT Name, Age, MakeSound(Animals) AS AnimalSound FROM Animals;
```

Kết quả sẽ trả về danh sách các động vật với tên, tuổi và tiếng kêu của họ, và hàm **_MakeSound_** đã được áp dụng đa hình để trả về tiếng kêu chính xác cho từng động vật mà không cần phải biết trước loại động vật đó.

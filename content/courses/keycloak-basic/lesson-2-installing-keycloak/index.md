---
draft: false
featuredImage: /courses/keycloak-basic/lesson-2-installing-keycloak.webp
images: [/courses/keycloak-basic/lesson-2-installing-keycloak.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series: [keycloak-basic]
tags: [keycloak, oauth2, openid-connect]
title: Lesson 2 - Cài đặt Keycloak
description: Hướng dẫn cài đặt Keycloak trên các hệ điều hành khác nhau và cấu hình ban đầu cho Keycloak.
date: 2024-07-02T19:23:30.000Z
weight: 2
---

### Cài đặt Keycloak trên các hệ điều hành khác nhau

#### Cài đặt trên Windows

1.  **Tải Keycloak**:
    -   Truy cập [Keycloak Downloads](https://www.keycloak.org/downloads) để tải phiên bản Keycloak mới nhất cho Windows. Chọn phiên bản ổn định và nhấp vào liên kết tải xuống.

2.  **Giải nén Keycloak**:
    -   Sau khi tải về, bạn sẽ có một tệp nén (ZIP hoặc TAR.GZ). Sử dụng phần mềm giải nén như WinRAR, 7-Zip hoặc phần mềm giải nén mặc định của Windows để giải nén tệp vào thư mục mong muốn, ví dụ: `C:\keycloak`.

3.  **Cài đặt Java**:
    -   Keycloak yêu cầu Java 11 hoặc Java 17. Nếu bạn chưa cài đặt Java, truy cập [Oracle JDK Downloads](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html) hoặc [OpenJDK](https://adoptopenjdk.net/) để tải và cài đặt JDK.
    -   Sau khi cài đặt, kiểm tra phiên bản Java bằng cách mở Command Prompt và chạy lệnh:
        ```sh
        java -version
        ```
        Nếu Java đã được cài đặt đúng, bạn sẽ thấy phiên bản Java hiện tại.

4.  **Thiết lập JAVA_HOME**:
    -   Thiết lập biến môi trường `JAVA_HOME` để chỉ đến thư mục cài đặt JDK. Mở `System Properties`:
        -   Nhấn `Win + Pause/Break` hoặc nhấn chuột phải vào `This PC` (hoặc `My Computer`) và chọn `Properties`.
        -   Chọn `Advanced system settings`.
        -   Chọn `Environment Variables`.
        -   Trong phần `System variables`, nhấn `New` để tạo biến mới với tên `JAVA_HOME` và giá trị là đường dẫn tới thư mục cài đặt JDK, ví dụ: `C:\Program Files\Java\jdk-11`.

5.  **Cập nhật PATH**:
    -   Trong cùng cửa sổ `Environment Variables`, tìm biến `Path` trong phần `System variables`, chọn `Edit`.
    -   Thêm một mục mới với giá trị là `C:\Program Files\Java\jdk-11\bin` (đường dẫn tới thư mục `bin` trong thư mục JDK).

6.  **Chạy Keycloak**:
    -   Mở Command Prompt với quyền quản trị (nhấn chuột phải vào Command Prompt và chọn `Run as administrator`).
    -   Điều hướng đến thư mục `bin` trong thư mục Keycloak bằng lệnh:
        ```sh
        cd C:\keycloak\bin
        ```
    -   Chạy lệnh sau để khởi động Keycloak:
        ```sh
        standalone.bat
        ```
        Keycloak sẽ khởi động và bạn sẽ thấy các thông báo khởi động trong Command Prompt.

7.  **Truy cập Keycloak**:
    -   Mở trình duyệt web và truy cập địa chỉ:
        ```url
        http://localhost:8080/auth
        ```
    -   Đây là giao diện quản trị của Keycloak, nơi bạn có thể cấu hình và quản lý các tính năng của Keycloak.

8.  **Tạo tài khoản admin**:
    -   Nếu đây là lần đầu bạn khởi động Keycloak, bạn cần tạo tài khoản quản trị (admin). Thực hiện lệnh sau trong thư mục `bin`:
        ```sh
        add-user-keycloak.bat -r master -u admin -p admin
        ```
        Thay đổi `admin` thành tên đăng nhập và mật khẩu mà bạn mong muốn. Sau đó, đăng nhập vào giao diện quản trị với tài khoản này.

9.  **Cấu hình cơ bản**:
    -   Sau khi đăng nhập, bạn có thể bắt đầu cấu hình các cài đặt cơ bản như tạo realms, clients, và người dùng. Hãy tham khảo tài liệu của Keycloak để biết thêm chi tiết về các bước cấu hình này.

#### Cài đặt trên Linux

1.  **Tải Keycloak**:
    -   Truy cập [Keycloak Downloads](https://www.keycloak.org/downloads) để tải phiên bản Keycloak mới nhất cho Linux. Chọn phiên bản ổn định và nhấp vào liên kết tải xuống. 

2.  **Giải nén Keycloak**:
    -   Sau khi tải về, bạn sẽ có một tệp nén (TAR.GZ). Mở Terminal và điều hướng đến thư mục chứa tệp đã tải về. Giải nén tệp bằng lệnh:
        ```sh
        tar -xvf keycloak-<version>.tar.gz -C /opt
        ```
        Thay `<version>` bằng phiên bản cụ thể của Keycloak mà bạn đã tải xuống. Thao tác này sẽ giải nén Keycloak vào thư mục `/opt/keycloak-<version>`.

3.  **Cài đặt Java**:
    -   Keycloak yêu cầu Java 11 hoặc Java 17. Nếu Java chưa được cài đặt trên hệ thống của bạn, cài đặt JDK bằng lệnh:
        ```sh
        sudo apt-get update
        sudo apt-get install openjdk-11-jdk
        ```
    -   Kiểm tra phiên bản Java đã được cài đặt bằng lệnh:
        ```sh
        java -version
        ```
        Bạn sẽ thấy thông tin về phiên bản Java hiện tại nếu Java đã được cài đặt đúng cách.

4.  **Thiết lập JAVA_HOME**:
    -   Thiết lập biến môi trường `JAVA_HOME` để chỉ đến thư mục cài đặt JDK. Mở file `~/.bashrc` bằng trình soạn thảo văn bản như nano hoặc vim:
        ```sh
        nano ~/.bashrc
        ```
    -   Thêm các dòng sau vào cuối file `~/.bashrc`:
        ```sh
        export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
        export PATH=$JAVA_HOME/bin:$PATH
        ```
    -   Lưu file và thoát khỏi trình soạn thảo. Sau đó, tải lại file `~/.bashrc` để các thay đổi có hiệu lực:
        ```sh
        source ~/.bashrc
        ```

5.  **Chạy Keycloak**:
    -   Điều hướng đến thư mục `bin` trong thư mục Keycloak bằng lệnh:
        ```sh
        cd /opt/keycloak-<version>/bin
        ```
    -   Khởi động Keycloak bằng lệnh:
        ```sh
        ./standalone.sh
        ```
        Keycloak sẽ khởi động và bạn sẽ thấy các thông báo khởi động trong Terminal.

6.  **Truy cập Keycloak**:
    -   Mở trình duyệt web và truy cập địa chỉ:
        ```url
        http://localhost:8080/auth
        ```
    -   Đây là giao diện quản trị của Keycloak, nơi bạn có thể cấu hình và quản lý các tính năng của Keycloak.

7.  **Tạo tài khoản admin**:
    -   Nếu đây là lần đầu bạn khởi động Keycloak, bạn cần tạo tài khoản quản trị (admin). Thực hiện lệnh sau trong thư mục `bin`:
        ```sh
        ./add-user-keycloak.sh -r master -u admin -p admin
        ```
        Thay đổi `admin` thành tên đăng nhập và mật khẩu mà bạn mong muốn. Sau đó, đăng nhập vào giao diện quản trị với tài khoản này.

8.  **Cấu hình cơ bản**:
    -   Sau khi đăng nhập, bạn có thể bắt đầu cấu hình các cài đặt cơ bản như tạo realms, clients, và người dùng. Hãy tham khảo tài liệu của Keycloak để biết thêm chi tiết về các bước cấu hình này.

#### Cài đặt trên macOS

1.  **Tải Keycloak**:
    -   Truy cập [Keycloak Downloads](https://www.keycloak.org/downloads) để tải phiên bản Keycloak mới nhất cho macOS. Chọn phiên bản ổn định và nhấp vào liên kết tải xuống.

2.  **Giải nén Keycloak**:
    -   Sau khi tải về, bạn sẽ có một tệp nén (ZIP hoặc TAR.GZ). Mở Terminal và điều hướng đến thư mục chứa tệp đã tải về. Giải nén tệp vào thư mục mong muốn, ví dụ:
        ```sh
        mkdir ~/keycloak
        tar -xvf keycloak-<version>.tar.gz -C ~/keycloak
        ```
        Thay `<version>` bằng phiên bản cụ thể của Keycloak mà bạn đã tải xuống. Thao tác này sẽ giải nén Keycloak vào thư mục `~/keycloak/keycloak-<version>`.

3.  **Cài đặt Java**:
    -   Keycloak yêu cầu Java 11 hoặc Java 17. Nếu Java chưa được cài đặt trên hệ thống của bạn, tải và cài đặt JDK từ [Oracle JDK Downloads](https://www.oracle.com/java/technologies/javase-jdk11-downloads.html) hoặc [OpenJDK](https://adoptopenjdk.net/).

4.  **Kiểm tra Java**:
    -   Sau khi cài đặt, kiểm tra phiên bản Java bằng cách mở Terminal và chạy lệnh:
        ```sh
        java -version
        ```
        Bạn sẽ thấy thông tin về phiên bản Java hiện tại nếu Java đã được cài đặt đúng cách.

5.  **Thiết lập JAVA_HOME**:

    -   Thiết lập biến môi trường `JAVA_HOME` để chỉ đến thư mục cài đặt JDK. Mở file `~/.bash_profile` bằng trình soạn thảo văn bản như nano hoặc vim:
        ```sh
        nano ~/.bash_profile
        ```

    -   Thêm các dòng sau vào cuối file `~/.bash_profile`:
        ```sh
        export JAVA_HOME=$(/usr/libexec/java_home -v 11)
        export PATH=$JAVA_HOME/bin:$PATH
        ```
        Thay đổi `11` thành `17` nếu bạn cài đặt Java 17.

    -   Lưu file và thoát khỏi trình soạn thảo. Sau đó, tải lại file `~/.bash_profile` để các thay đổi có hiệu lực:
        ```sh
        source ~/.bash_profile
        ```

6.  **Chạy Keycloak**:

    -   Mở Terminal và điều hướng đến thư mục `bin` trong thư mục Keycloak:
        ```sh
        cd ~/keycloak/keycloak-<version>/bin
        ```
        Thay `<version>` bằng phiên bản cụ thể của Keycloak mà bạn đã tải xuống.

    -   Khởi động Keycloak bằng lệnh:
        ```sh
        ./standalone.sh
        ```
        Keycloak sẽ khởi động và bạn sẽ thấy các thông báo khởi động trong Terminal.

7.  **Truy cập Keycloak**:
    -   Mở trình duyệt web và truy cập địa chỉ:
        ```url
        http://localhost:8080/auth
        ```
    -   Đây là giao diện quản trị của Keycloak, nơi bạn có thể cấu hình và quản lý các tính năng của Keycloak.

8.  **Tạo tài khoản admin**:
    -   Nếu đây là lần đầu bạn khởi động Keycloak, bạn cần tạo tài khoản quản trị (admin). Thực hiện lệnh sau trong thư mục `bin`:
        ```sh
        ./add-user-keycloak.sh -r master -u admin -p admin
        ```
        Thay đổi `admin` thành tên đăng nhập và mật khẩu mà bạn mong muốn. Sau đó, đăng nhập vào giao diện quản trị với tài khoản này.

9.  **Cấu hình cơ bản**:
    -   Sau khi đăng nhập, bạn có thể bắt đầu cấu hình các cài đặt cơ bản như tạo realms, clients, và người dùng. Hãy tham khảo tài liệu của Keycloak để biết thêm chi tiết về các bước cấu hình này.

### Cấu hình ban đầu cho Keycloak

#### Cấu hình cơ bản trong file `standalone.xml`

1.  **Mở file cấu hình**:
    -   File `standalone.xml` nằm trong thư mục `keycloak/standalone/configuration`.
    -   Sử dụng trình soạn thảo văn bản như nano, vim, hoặc một trình soạn thảo GUI để mở file này. Ví dụ:
        ```sh
        nano /path/to/keycloak/standalone/configuration/standalone.xml
        ```

2.  **Cấu hình cơ sở dữ liệu**:
    -   Tìm thẻ `<datasources>`. Thêm cấu hình kết nối đến cơ sở dữ liệu của bạn. Ví dụ, để cấu hình PostgreSQL:
        ```xml
        <datasource jta="true" jndi-name="java:jboss/datasources/KeycloakDS" pool-name="KeycloakDS" enabled="true" use-java-context="true">
            <connection-url>jdbc:postgresql://localhost:5432/keycloak</connection-url>
            <driver>postgresql</driver>
            <security>
                <user-name>keycloak</user-name>
                <password>password</password>
            </security>
        </datasource>
        ```
        -   `connection-url`: Địa chỉ URL kết nối đến cơ sở dữ liệu PostgreSQL.
        -   `driver`: Tên driver của cơ sở dữ liệu (ở đây là `postgresql`).
        -   `user-name` và `password`: Thông tin tài khoản của cơ sở dữ liệu.

3.  **Cấu hình khác**:
    -   Cập nhật các thông số khác trong file `standalone.xml` theo yêu cầu của bạn, chẳng hạn như cấu hình cổng, các kết nối khác, hoặc cấu hình bộ nhớ.

#### Cấu hình cơ bản trong file `standalone-ha.xml`

1.  **Mở file cấu hình**:
    -   File `standalone-ha.xml` nằm trong thư mục `keycloak/standalone/configuration`.
    -   Sử dụng trình soạn thảo văn bản để mở file này. Ví dụ:
        ```sh
        nano /path/to/keycloak/standalone/configuration/standalone-ha.xml
        ```

2.  **Cấu hình cụm (Cluster Configuration)**:
    -   Tìm thẻ `<subsystem xmlns="urn:jboss:domain:infinispan:...">`. Đây là phần cấu hình dành cho việc thiết lập cụm (clustering) và phân tán dữ liệu.
    -   Thay đổi và thêm các thông số cấu hình phù hợp với môi trường của bạn. Ví dụ:
        ```xml
        <cache-container name="server" default-cache="default" module="org.wildfly.clustering.server">
            <transport lock-timeout="60000"/>
            <replicated-cache name="default"/>
            <distributed-cache name="sessions"/>
        </cache-container>
        ```

3.  **Cấu hình khác**:
    -   Cập nhật các thông số khác theo yêu cầu của bạn, chẳng hạn như cấu hình cụm, cân bằng tải, hoặc các kết nối mạng khác.

### Tạo người dùng quản trị

#### Lệnh tạo người dùng admin bằng CLI

1.  **Chạy lệnh tạo người dùng**:
    -   Mở Terminal hoặc Command Prompt và điều hướng đến thư mục `bin` trong thư mục Keycloak. Ví dụ:
        ```sh
        cd /path/to/keycloak/bin
        ```
    -   Chạy lệnh sau để tạo người dùng admin:
        ```sh
        ./add-user-keycloak.sh -r master -u admin -p password
        ```
        -   `-r master`: Chỉ định realm là `master`.
        -   `-u admin`: Tên đăng nhập của người dùng admin.
        -   `-p password`: Mật khẩu của người dùng admin.

#### Đăng nhập vào Admin Console

1.  **Truy cập Admin Console**:
    -   Mở trình duyệt web và truy cập URL:
        ```url
        http://localhost:8080/auth
        ```

2.  **Đăng nhập**:
    -   Sử dụng thông tin tài khoản admin vừa tạo (tên đăng nhập và mật khẩu) để đăng nhập vào Admin Console.
    -   Sau khi đăng nhập thành công, bạn sẽ thấy giao diện quản trị của Keycloak, nơi bạn có thể thực hiện các cấu hình và quản lý người dùng, realms, clients, và nhiều tính năng khác.

### Cấu hình bổ sung sau khi đăng nhập

1.  **Tạo Realm mới**:
    -   Trong Admin Console, chọn `Add realm` để tạo một realm mới.
    -   Nhập tên realm và nhấn `Create`.

2.  **Tạo Client mới**:
    -   Trong realm mới tạo, chọn `Clients` và nhấn `Create`.
    -   Nhập Client ID và cấu hình các thông số cần thiết.

3.  **Tạo User mới**:
    -   Trong realm mới tạo, chọn `Users` và nhấn `Add user`.
    -   Nhập thông tin người dùng và nhấn `Save`.
    -   Thiết lập mật khẩu cho người dùng mới trong tab `Credentials`.

### Lưu ý

-   **Sao lưu cấu hình**: Luôn sao lưu các file cấu hình trước khi thực hiện bất kỳ thay đổi nào.
-   **Kiểm tra kết nối cơ sở dữ liệu**: Đảm bảo rằng cơ sở dữ liệu của bạn đang chạy và các thông tin kết nối (URL, user, password) là chính xác.
-   **Chạy Keycloak dưới quyền quản trị**: Đảm bảo rằng bạn có quyền truy cập quản trị khi thực hiện các thay đổi hệ thống hoặc cài đặt phần mềm.

---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-4-installing-ubuntu-server-on-virtualbox.webp
images:
  - /courses/ubuntu-basics-using-virtualbox/lesson-4-installing-ubuntu-server-on-virtualbox.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - ubuntu-basics-using-virtualbox
tags:
  - ubuntu-server
  - virtualbox
title: Lesson 4 - Cài Đặt Ubuntu Server trên VirtualBox
description: Hướng dẫn cài đặt Ubuntu Server trên VirtualBox, bước đầu tiên để tạo máy chủ ảo trên máy tính cá nhân.
date: 2024-06-27T10:23:30-09:00
weight: 4
---

Trong thế giới công nghệ ngày nay, việc nắm vững các kỹ năng liên quan đến quản trị hệ thống và ảo hóa là vô cùng quan trọng. Ubuntu Server là một trong những hệ điều hành máy chủ phổ biến nhất, được sử dụng rộng rãi trong các doanh nghiệp và tổ chức nhờ vào tính ổn định, bảo mật và hiệu suất cao. VirtualBox, một công cụ ảo hóa mạnh mẽ và linh hoạt, cho phép chúng ta dễ dàng tạo và quản lý các máy ảo để thực hành và thử nghiệm mà không ảnh hưởng đến hệ thống chính.

Trong bài viết này, chúng ta sẽ tìm hiểu chi tiết cách cài đặt Ubuntu Server trên VirtualBox. Bài viết sẽ hướng dẫn bạn từ khâu chuẩn bị, tải xuống tệp ISO của Ubuntu Server, tạo máy ảo, đến việc cài đặt hệ điều hành và thực hiện các cấu hình ban đầu. Qua bài viết này, bạn sẽ nắm vững quy trình cài đặt và có thể áp dụng để xây dựng môi trường thực hành hoặc triển khai máy chủ cho các dự án của mình.

### 1. Tải Xuống Ubuntu Server ISO

Truy cập vào trang web chính thức của Ubuntu để tải xuống phiên bản Ubuntu Server mới nhất:

1. Mở trình duyệt web và truy cập trang [Ubuntu Server Downloads](https://ubuntu.com/download/server).
2. Chọn phiên bản LTS (Long Term Support) mới nhất để đảm bảo tính ổn định và hỗ trợ dài hạn.
3. Nhấp vào nút **Download** và lưu tệp ISO vào máy tính của bạn.

{{< figure src="/images/step-01.jpg" >}}

### 2. Tạo Máy Ảo Trong VirtualBox

Xem lại bài viết [Tạo Máy Ảo Trong VirtualBox](/courses/ubuntu-basics-using-virtualbox/lesson-3-creating-a-virtual-machine-in-virtualbox) để biết cách tạo máy ảo cho Ubuntu Server trên VirtualBox.

### 3. Cài Đặt Ubuntu Server từ ISO

1. **Gắn tệp ISO vào máy ảo**:

    {{< figure src="/images/step-02.jpg" >}}

   - Mở VirtualBox và chọn máy ảo Ubuntu Server đã tạo.
   - Chọn máy ảo vừa tạo và nhấp **Settings**.
   - Trong tab **Storage**, chọn **Empty** dưới **Controller: IDE**.
   - Nhấp vào biểu tượng đĩa bên phải và chọn **Choose a disk file**.
   - Chọn tệp ISO của Ubuntu Server đã tải xuống và nhấp **Open**.
   - Nhấp **OK** để lưu cài đặt.

2. **Khởi động máy ảo**:

   {{< figure src="/images/step-03.jpg" >}}

   - Chọn máy ảo và nhấp **Start** để khởi động.
   - Máy ảo sẽ khởi động từ tệp ISO và hiển thị trình cài đặt Ubuntu Server.
   - Sau dòng lệnh `Install Ubuntu Server`, nhấp **Enter** để tiếp tục.

3. **Tiến hành cài đặt**:
     - **Chọn ngôn ngữ**: Chọn ngôn ngữ bạn muốn sử dụng trong quá trình cài đặt và nhấn **Enter**, Ở đây mình chọn **English**.
    {{< figure src="/images/step-04.jpg" >}}
     - **Chọn bố trí bàn phím**: Chọn bố trí bàn phím phù hợp và nhấn **Enter**, Ở đây mình chọn **English (US)**.
        {{< figure src="/images/step-05.jpg" >}}
     - **Chọn kiểu cài đặt**: Chọn **Ubuntu Server** và nhấn **Enter**.
        {{< figure src="/images/step-06.jpg" >}}
    {{< admonition type="info" title="Có 3 Kiểu Cài Đặt" >}}
       1. Ubuntu Server: Cài đặt hệ điều hành Ubuntu Server với giao diện dòng lệnh.
       2. Ubuntu Server Minimal: Cài đặt phiên bản nhẹ của Ubuntu Server với ít gói cài đặt mặc định.
       3. Additional Options: Cài đặt các tùy chọn bổ sung như OpenSSH, LVM, và các gói cài đặt khác.
    {{< /admonition >}}
     - **Cấu hình mạng**: Nếu bạn đang sử dụng kết nối mạng có dây, hệ thống sẽ tự động phát hiện và cấu hình mạng. Nếu không, bạn cần cấu hình thủ công, ở đây mình chọn **Done**.
    {{< figure src="/images/step-07.jpg" >}}
     - **Cấu hình proxy**: Nếu bạn sử dụng proxy để kết nối internet, hãy cấu hình proxy tại đây. Nếu không, hãy bỏ trống và nhấn **Enter**.
     - **Cập nhật và cài đặt gói**: Chọn **Yes** để cập nhật danh sách gói cài đặt từ các kho lưu trữ Ubuntu, sau đó chờ quá trình cập nhật hoàn tất.
     - **Chọn ổ đĩa để cài đặt**: Chọn ổ đĩa ảo mà bạn đã tạo và nhấn **Enter** để xác nhận các thay đổi, ở đây mình chọn **Use an entire disk**, Sau đó chọn **Continue**, Tiếp theo chọn **Done**.
        {{< figure src="/images/step-09.jpg" >}}
        {{< figure src="/images/step-10.jpg" >}}
     - **Đặt tên máy chủ và người dùng**: Nhập tên máy chủ và tên người dùng cho tài khoản quản trị.
        {{< admonition type="info" title="Thông Tin Cài Đặt" >}}
         - Your name: Nhập tên của bạn.
         - Your server's name: Nhập tên máy chủ.
         - Pick a username: Chọn tên người dùng cho tài khoản quản trị.
         - Choose a password: Nhập mật khẩu cho tài khoản quản trị.
         - Confirm your password: Nhập lại mật khẩu để xác nhận.
         {{< /admonition >}}
         {{< figure src="/images/step-11.jpg" >}}
     - **SSH server**: Chọn **Install OpenSSH server** để cài đặt dịch vụ SSH và cho phép kết nối từ xa vào máy chủ, sau đó chọn **Done**.
        {{< figure src="/images/step-12.jpg" >}}
     - **Chờ quá trình cài đặt hoàn tất**: Hệ thống sẽ tự động cài đặt các gói cần thiết và cấu hình hệ thống. Quá trình này có thể mất vài phút.
  
4. **Hoàn tất cài đặt và khởi động lại**:
   - Sau khi cài đặt hoàn tất, hệ thống sẽ yêu cầu bạn khởi động lại máy ảo. Nhấn **Enter** để khởi động lại.
    {{< figure src="/images/step-13.jpg" >}}
   - Máy ảo sẽ khởi động lại và yêu cầu bạn đăng nhập với tài khoản quản trị đã tạo.

### 4. Cấu Hình Ban Đầu Cho Ubuntu Server

Sau khi cài đặt hoàn tất, máy ảo sẽ khởi động lại và yêu cầu bạn đăng nhập. Thực hiện các cấu hình ban đầu sau:

1. **Đăng nhập**:
   - Sử dụng tên người dùng và mật khẩu đã tạo trong quá trình cài đặt để đăng nhập.

2. **Cập nhật hệ thống**:
   - Mở terminal và chạy các lệnh sau để cập nhật hệ thống:

     ```bash
     sudo apt update
     sudo apt upgrade -y
     ```

3. **Cài đặt các gói cần thiết**:
   - Cài đặt các gói cơ bản cho hệ thống:

     ```bash
     sudo apt install -y curl wget vim git
     ```

4. **Cấu hình tường lửa**:
   - Cài đặt và cấu hình tường lửa để bảo vệ máy chủ:

     ```bash
     sudo apt install -y ufw
     sudo ufw allow OpenSSH
     sudo ufw enable
     ```

### 5. Kết Luận

Bạn đã hoàn tất việc cài đặt Ubuntu Server trên VirtualBox. Bây giờ bạn có thể sử dụng máy ảo này để thực hiện các thao tác quản lý và cấu hình máy chủ theo nhu cầu của bạn. Trong các bài viết tiếp theo, chúng ta sẽ khám phá các cấu hình nâng cao và cài đặt các dịch vụ trên Ubuntu Server.

### Lưu Ý

- Đảm bảo bạn có kết nối internet ổn định trong quá trình tải xuống và cài đặt.
- Luôn cập nhật hệ điều hành và các gói phần mềm để đảm bảo tính bảo mật và ổn định.
- Nếu gặp vấn đề trong quá trình cài đặt, hãy kiểm tra lại các bước và tham khảo tài liệu hỗ trợ của Ubuntu hoặc VirtualBox.

Chúc bạn thành công trong việc cài đặt và cấu hình Ubuntu Server trên VirtualBox!

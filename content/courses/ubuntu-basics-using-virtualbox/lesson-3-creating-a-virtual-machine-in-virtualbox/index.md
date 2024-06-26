---
draft: true
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-3-creating-a-virtual-machine-in-virtualbox.webp
images:
  - /courses/ubuntu-basics-using-virtualbox/lesson-3-creating-a-virtual-machine-in-virtualbox.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - ubuntu-basics-using-virtualbox
tags:
  - ubuntu-server
  - virtualbox
title: Lesson 3 - Tao máy ảo trong VirtualBox
description: Trong bài viết này, chúng ta sẽ tìm hiểu cách tạo máy ảo trong Oracle VM VirtualBox để cài đặt hệ điều hành Ubuntu Server.
date: 2024-06-25T10:23:30-09:00
weight: 3
---

Chào mừng bạn đến với loạt bài về thực chiến Linux, từ cơ bản đến nâng cao. Trong bài viết này, chúng ta sẽ khám phá cách tạo một máy ảo mới cho Ubuntu Server trên Oracle VM VirtualBox và cấu hình các thông số quan trọng như RAM, CPU, ổ cứng ảo và cài đặt mạng.

Oracle VM VirtualBox là một công cụ mạnh mẽ và linh hoạt cho phép chúng ta dễ dàng tạo và quản lý các máy ảo. Việc tạo máy ảo cho phép bạn thực hành và thử nghiệm các cấu hình mà không ảnh hưởng đến hệ thống chính của bạn.

## Bước 1: Mở Oracle VM VirtualBox

- Nhấp đúp vào biểu tượng Oracle VM VirtualBox trên màn hình desktop hoặc tìm kiếm trong menu Start.

## Bước 2: Bắt Đầu Tạo Máy Ảo Mới

- Nhấp vào nút "New" trên thanh công cụ. Cửa sổ "Create Virtual Machine" sẽ xuất hiện.

## Bước 3: Đặt Tên và Chọn Hệ Điều Hành

- Trong cửa sổ này, nhập tên cho máy ảo của bạn (ví dụ: "Ubuntu Server").
- Trong phần "Type," chọn "Linux."
- Trong phần "Version," chọn "Ubuntu (64-bit)."
- Nhấp "Next" để tiếp tục.

## Cấu Hình RAM

- Trong cửa sổ "Memory Size," sử dụng thanh trượt hoặc nhập trực tiếp số RAM bạn muốn cấp phát cho máy ảo (khuyến nghị ít nhất 1024 MB).



#### Khuyến Nghị Cấp Phát RAM

- **Ubuntu Server:** Khuyến nghị ít nhất 1024 MB (1 GB) RAM để đảm bảo hệ điều hành Ubuntu Server chạy mượt mà và ổn định. Tuy nhiên, nếu bạn có đủ tài nguyên, việc cấp phát nhiều RAM hơn sẽ cải thiện hiệu suất.
- **Công thức tính RAM:** Nếu bạn không chắc chắn nên cấp phát bao nhiêu RAM cho máy ảo, bạn có thể sử dụng công thức đơn giản sau:

```mermaid
graph LR;
    A[RAM tối thiểu] --> B[= RAM vật lý của máy tính]
    B --> C[× 1/2]
```
  Ví dụ: Nếu máy tính của bạn có 8 GB RAM vật lý, bạn có thể cấp phát tối đa 4 GB (4096 MB) cho máy ảo mà không ảnh hưởng quá nhiều đến hiệu suất của hệ điều hành chính.

#### Ví Dụ Cụ Thể

- **Máy tính với 4 GB RAM:** Khuyến nghị cấp phát 1024 MB RAM cho máy ảo Ubuntu Server.
- **Máy tính với 8 GB RAM:** Khuyến nghị cấp phát 2048 MB RAM cho máy ảo Ubuntu Server.
- **Máy tính với 16 GB RAM:** Khuyến nghị cấp phát 4096 MB RAM cho máy ảo Ubuntu Server.


{{< admonition type="warning" title="Lưu Ý Khi Cấp Phát RAM" >}}


- **Đảm bảo đủ RAM cho hệ điều hành chính:** Khi cấp phát RAM cho máy ảo, bạn cần đảm bảo rằng hệ điều hành chính (máy chủ) vẫn còn đủ RAM để hoạt động bình thường. Ví dụ, nếu máy tính của bạn có 8 GB RAM, cấp phát 6 GB cho máy ảo có thể làm giảm hiệu suất của hệ điều hành chính.
- **Kiểm tra hiệu suất:** Sau khi cấp phát RAM và khởi động máy ảo, bạn nên kiểm tra hiệu suất để đảm bảo rằng máy ảo chạy mượt mà. Nếu thấy máy ảo chạy chậm hoặc hệ điều hành chính bị ảnh hưởng, bạn có thể điều chỉnh lại dung lượng RAM.
{{< /admonition >}}


- Nhấp "Next" để tiếp tục.

## Cấu Hình Ổ Cứng Ảo

- Chọn "Create a virtual hard disk now" và nhấp "Create."

## Chọn Loại Ổ Cứng Ảo

- Chọn "VDI (VirtualBox Disk Image)" và nhấp "Next."

## Loại Lưu Trữ Ổ Cứng

- Chọn "Dynamically allocated" để ổ cứng ảo chỉ sử dụng dung lượng trên ổ cứng vật lý khi cần thiết (tối đa dung lượng cố định). Nhấp "Next."

## Đặt Dung Lượng Ổ Cứng Ảo

- Đặt dung lượng tối đa cho ổ cứng ảo (khuyến nghị ít nhất 10 GB).
- Nhấp "Create" để hoàn tất thiết lập ổ cứng ảo.

## Cấu Hình CPU

- Quay lại giao diện chính của Oracle VM VirtualBox.
- Chọn máy ảo vừa tạo và nhấp "Settings."
- Vào phần "System" và chọn tab "Processor."
- Điều chỉnh số lượng CPU (khuyến nghị ít nhất 1 CPU). Nhấp "OK" để lưu cài đặt.

## Cấu Hình Mạng

- Trong cửa sổ "Settings," vào phần "Network."
- Chọn "Attached to: NAT" để máy ảo có thể truy cập internet thông qua mạng của máy chủ.
- Nhấp "OK" để lưu cài đặt mạng.

Bằng cách làm theo các bước này, bạn đã tạo thành công một máy ảo mới cho Ubuntu Server trên Oracle VM VirtualBox và cấu hình các thông số chính của nó. Thiết lập này cho phép bạn thử nghiệm các cấu hình Linux trong một môi trường an toàn và biệt lập.
---
draft: false
featuredImage: /courses/ubuntu-basics-using-virtualbox/lesson-1-introduction-to-ubuntu-server-and-virtualbox.webp
images:
  - /courses/ubuntu-basics-using-virtualbox/lesson-1-introduction-to-ubuntu-server-and-virtualbox.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - docker-basic-series
tags:
  - ubuntu-server
  - virtualbox
title: Lesson 1 -  Giới Thiệu về Ubuntu Server và VirtualBox
description: Trong bài học này, bạn sẽ tìm hiểu về Ubuntu Server và VirtualBox, cũng như cách cài đặt chúng trên máy tính của bạn. Ubuntu Server là một hệ điều hành Linux dựa trên Debian, được thiết kế để chạy trên máy chủ và máy ảo. VirtualBox là một ứng dụng ảo hóa mã nguồn mở, cho phép bạn tạo và quản lý máy ảo trên máy tính cá nhân của mình.
date: 2024-06-24T10:23:30-09:00
weight: 1
---
### Tổng Quan Về Ubuntu và Ubuntu Server

Trong cõi giang hồ đầy hiểm nguy và kỳ bí, nơi các đại hiệp tranh đấu, môn phái Ubuntu nổi lên như một thế lực mới, khởi nguồn từ tiền bối đại hiệp Debian. Chưởng môn Canonical đã phát triển Ubuntu trở thành một môn phái hùng mạnh, thu hút vô số đệ tử khắp bốn phương. Giống như môn phái Võ Đang nổi tiếng với Thái Cực Quyền, Ubuntu nổi bật với sự thân thiện, dễ sử dụng và khả năng tương thích cao, khiến ai nấy đều ngưỡng mộ và muốn gia nhập.

Trong số các tuyệt kỹ của môn phái Ubuntu, có một bộ tâm pháp thượng thừa, chỉ dành cho những cao thủ chân truyền - đó chính là Ubuntu Server. Bí kíp này tương tự như Cửu Âm Chân Kinh, khi luyện thành sẽ giúp các cao thủ dễ dàng triển khai và quản lý các dịch vụ như web server, database server, và nhiều dịch vụ khác. Những đặc điểm nổi bật của Ubuntu Server gồm có:

1. **Tính bảo mật cao**: Giống như bộ võ công Kim Cương Bất Hoại, Ubuntu Server luôn được cập nhật thường xuyên với các bản vá bảo mật, đảm bảo hệ thống không bị kẻ xấu tấn công.
2. **Hỗ trợ dài hạn (LTS)**: Phiên bản LTS của Ubuntu Server được hỗ trợ trong 5 năm, như một lời thề son sắt của đại hiệp trước giờ ra trận.
3. **Cộng đồng hỗ trợ mạnh mẽ**: Ubuntu có một cộng đồng lớn, như bang hội Cái Bang với những cao thủ luôn sẵn sàng giúp đỡ và chia sẻ kinh nghiệm. Cần gì, cứ lên tiếng là có ngay!
4. **Khả năng tùy biến**: Ubuntu Server không đi kèm với giao diện đồ họa mặc định, cho phép các quản trị viên tùy chỉnh và cài đặt những gì cần thiết, tối ưu hóa hiệu suất. Tựa như các cao thủ luyện võ, mỗi người có thể chọn cho mình một lối đánh riêng biệt, phù hợp nhất với sở trường.

### Giới Thiệu Về VirtualBox và Lý Do Sử Dụng VirtualBox

Trong kho tàng võ học, có một thanh bảo kiếm báu mang tên VirtualBox, do chính tay đại sư Oracle chế tác. Bảo kiếm này, giống như Đồ Long Đao trong truyền thuyết, có thể giúp người sử dụng tung hoành ngang dọc, chạy nhiều hệ điều hành trên cùng một máy tính. VirtualBox hỗ trợ nhiều hệ điều hành khách (guest OS), giống như Đồ Long Đao có thể cắt đứt mọi loại binh khí khác.

#### Lý do sử dụng VirtualBox trong việc luyện tập và vận hành Ubuntu Server:

1. **Tiết kiệm chi phí**: Sử dụng VirtualBox không đòi hỏi phải có nhiều máy tính. Chỉ cần một máy, bạn có thể tạo ra nhiều môi trường ảo khác nhau, tiết kiệm ngân sách như một võ sĩ tiết kiệm nội lực.
2. **An toàn và tiện lợi**: VirtualBox cho phép bạn thử nghiệm các chiêu thức mới mà không lo tổn hại đến hệ thống chính. Nếu có sự cố, bạn có thể dễ dàng khôi phục lại trạng thái trước đó như một cao thủ luyện võ có thể hồi phục nội lực sau khi tập luyện quá sức.
3. **Dễ dàng sao lưu và khôi phục**: Trước khi thực hiện bất kỳ thay đổi nào, bạn có thể tạo snapshot của máy ảo. Điều này cho phép bạn quay lại trạng thái trước đó nếu có lỗi xảy ra, như một chiêu hồi phục trong các trận đấu võ đài.
4. **Linh hoạt trong học tập**: VirtualBox cho phép bạn tạo nhiều máy ảo để mô phỏng các môi trường khác nhau. Bạn có thể thực hành cài đặt và cấu hình Ubuntu Server mà không cần phải lo lắng về phần cứng hoặc ảnh hưởng đến hệ điều hành chính.
5. **Hỗ trợ đa nền tảng**: VirtualBox có thể chạy trên nhiều hệ điều hành chủ (host OS) như Windows, macOS và Linux, cung cấp sự linh hoạt cho người dùng ở mọi môi trường.

Với VirtualBox, bạn có thể luyện tập cách cài đặt, cấu hình và quản lý Ubuntu Server trong một môi trường an toàn và tiết kiệm. Điều này giúp bạn trở thành một quản trị viên hệ thống tài ba, tựa như một võ lâm cao thủ với kỹ năng điêu luyện và kiến thức uyên bác. Cũng như Đoàn Dự học Lăng Ba Vi Bộ, bạn sẽ nắm vững mọi chiêu thức và trở thành cao thủ trong giới quản trị hệ thống.
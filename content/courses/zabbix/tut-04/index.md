---
categories:
  - devops
  - zabbix
date: 2023-03-04T08:00:00+08:00
description: Ở phần cảnh báo của Zabbix mình thích nhất là cảnh báo qua Telegram vì nhanh và bảo mật
draft: false
featuredImage: /series/zabbix/zabbix-agent-2-tren-ubuntu-2204-theo-doi-may-chu-postgresql.webp
images:
  - /series/zabbix/zabbix-agent-2-tren-ubuntu-2204-theo-doi-may-chu-postgresql.webp
  - /zabbix-62-canh-bao-qua-telegram/images/index.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - zabbix-tutorial
tags:
  - zabbix-agent
  - zabbix-server
  - zabbix-agent-6.2
title: Lesstion 4 - Zabbix 6.2 cảnh báo qua Telegram
url: /zabbix-62-canh-bao-qua-telegram
weight: 4
---

## Thiết lập Telegram

1. Đăng ký Bot Telegram mới: Gửi "**/NewBot**" tới **@BotFather** và làm theo hướng dẫn. Mã thông báo được cung cấp bởi **@botfather** trong bước cuối cùng sẽ là cần thiết để định cấu hình **Zabbix Webhook**.

{{< figure src="/images/46fb93cf-6452-4b3a-94a0-4d6ef9178a7c.png" >}}

2. Nếu bạn muốn gửi thông báo cá nhân, bạn cần lấy ID trò chuyện của người dùng mà bot nên gửi tin nhắn.

{{< figure src="/images/edc5dc3c-1e40-4287-af90-629c7bed207e.png" >}}

Yêu cầu người dùng gửi "/start" cho bot, được tạo ở bước 1. Nếu bạn bỏ qua bước này, Bot Telegram sẽ không thể gửi tin nhắn cho người dùng.

{{< figure src="/images/1c762f47-31d6-4c37-994d-50147010f638.png" >}}

## Thiết lập Zabbix

1. Trong phần "Administration > Media types", nhập media_telegram.yaml.
2. Định cấu hình loại phương tiện được thêm vào: Sao chép và dán mã thông báo bot telegram của bạn vào trường "Telegramtoken".

{{< figure src="/images/5cc3c0c2-c6d8-4c4d-8e20-107dc3d26571.png" >}}

Trong tùy chọn tham số **Parsemode** được yêu cầu theo tài liệu của Telegram. Đọc tài liệu API BOT Telegram để tìm hiểu cách định dạng thông báo : [Markdown](https://core.telegram.org/bots/api#markdown-style) / [HTML](https://core.telegram.org/bots/api#html-style) / [MarkDownv2](https://core.telegram.org/bots/api#markdownv2-style).

Lưu ý: Trong trường hợp này, các hành động liên quan đến Telegram của bạn phải được tách ra khỏi các hành động thông báo khác (ví dụ: SMS), nếu không bạn có thể nhận được thông báo văn bản đơn giản với thẻ Raw Markdown/HTML.

Sau khi config xong ta và phần test để test congfig đã ăn chưa bằng id của nhóm hoặc ID cá nhân

{{< figure src="/images/6cc9aebe-e93c-4d87-8114-08f23e12463d.png" >}}

{{< figure src="/images/f7009de6-f711-4327-a166-e2f64782521f.png" >}}

Nếu bạn đã quên gửi '/start' đến bot từ Telegram, bạn sẽ gặp lỗi sau:

{{< figure src="/images/b113c9a7-f38c-49dd-a7fd-30288d6b6d28.png" >}}

3. Để nhận thông báo trong Telegram, bạn cần tạo người dùng Zabbix và thêm phương tiện với loại Telegram. Trong trường "Sent to" Nhập ID người dùng Telegram ID hoặc ID nhóm thu được trong quá trình thiết lập Telegram.

{{< figure src="/images/50a94837-6758-4d9f-b5be-d610a6158ac0.png" >}}

Sau khi sự cố thì sẽ thông báo qua telegram

{{< figure src="/images/ccc48e68-71d1-4307-9db0-de8f251624d4.png" >}}

Bạn có thể tham khảo chi tiết qua : https://www.zabbix.com/integrations/telegram

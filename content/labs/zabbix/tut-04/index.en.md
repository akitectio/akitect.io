---
categories:
  - devops
  - zabbix
date: 2023-03-04T08:00:00+08:00
description: In the Zabbix alert section, I like the Telegram alert the most because it is fast and secure
draft: false
featuredImage: /series/zabbix/zabbix-agent-2-tren-ubuntu-2204-theo-doi-may-chu-postgresql.webp
images:
  - /series/zabbix/zabbix-agent-2-tren-ubuntu-2204-theo-doi-may-chu-postgresql.webp
  - /zabbix-62-alerts-via-telegram/images/index.en.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
series:
  - zabbix-tutorial
tags:
  - zabbix-agent
  - Zabbix Server
  - zabbix-agent-6.2
title: Zabbix 6.2 alerts via Telegram
url: /zabbix-62-alerts-via-telegram
weight: 4
---

## Setting up Telegram

1. Register a new Telegram Bot: Send "**/NewBot**" to **@BotFather** and follow the instructions. The token provided by **@botfather** in the final step will be necessary to configure the **Zabbix Webhook**.

{{< figure src="./images/46fb93cf-6452-4b3a-94a0-4d6ef9178a7c.png" >}}

2. If you want to send personal notifications, you need to get the chat ID of the user that the bot should send messages to.

{{< figure src="./images/edc5dc3c-1e40-4287-af90-629c7bed207e.png" >}}

Ask the user to send "/start" to the bot, created in step 1. If you skip this step, the Telegram Bot will not be able to send messages to the user.

{{< figure src="./images/1c762f47-31d6-4c37-994d-50147010f638.png" >}}

## Setting up Zabbix

1. In the "Administration > Media types" section, enter media_telegram.yaml.
2. Configure the type of media to be added: Copy and paste your Telegram bot token into the "Telegramtoken" field.

{{< figure src="./images/5cc3c0c2-c6d8-4c4d-8e20-107dc3d26571.png" >}}

In the **Parsemode** parameter option required by Telegram documentation. Read the Telegram BOT API documentation to learn how to format notifications: [Markdown](https://core.telegram.org/bots/api#markdown-style) / [HTML](https://core.telegram.org/bots/api#html-style) / [MarkDownv2](https://core.telegram.org/bots/api#markdownv2-style).

Note: In this case, your Telegram-related actions must be separated from other notification actions (e.g. SMS), otherwise you may receive a simple text notification with Raw Markdown/HTML tags.

After configuring, test the configuration by using the ID of the group or individual.

{{< figure src="./images/6cc9aebe-e93c-4d87-8114-08f23e12463d.png" >}}

{{< figure src="./images/f7009de6-f711-4327-a166-e2f64782521f.png" >}}

If you forget to send '/start' to the bot from Telegram, you will encounter the following error:

{{< figure src="./images/b113c9a7-f38c-49dd-a7fd-30288d6b6d28.png" >}}

3. To receive notifications in Telegram, you need to create a Zabbix user and add media with the Telegram type. In the "Sent to" field, enter the Telegram user ID or group ID obtained during the Telegram setup process.

{{< figure src="./images/50a94837-6758-4d9f-b5be-d610a6158ac0.png" >}}

After the incident, the notification will be sent via Telegram.

{{< figure src="./images/ccc48e68-71d1-4307-9db0-de8f251624d4.png" >}}

You can refer to the details at: https://www.zabbix.com/integrations/telegram

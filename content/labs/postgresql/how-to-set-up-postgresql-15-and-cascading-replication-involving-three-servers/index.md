---
categories:
  - database
date: 2024-02-22T08:00:00+08:00
draft: false
featuredImage: /series/postgresql.png
images:
  - /cai-dat-va-bao-mat-postgresql-16-tren-ubuntu-2304/images/index.png
  - /series/postgresql.png
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - Database
  - PostgreSQL
  - Ubuntu
  - PostgreSQL 16
title: Cài đặt replication cho postgresql 15 trên ubuntu 22.04
description : PostgreSQL có tính năng sao chép tầng, cho phép sao chép dữ liệu từ DB này sang DB khác, tạo nhiều bản sao dữ liệu. Tính năng này giúp phân phối dữ liệu, đảm bảo dữ liệu mới nhất và hỗ trợ thay thế máy chủ chính.

---

Trước khi bắt đầu ta cần chuẩn bị 3 máy chủ

| IP           | Hostname          | vCPU   | RAM | DISK |
| ------------ | ----------------- | ------ | --- | ---- |
| 192.168.56.2 | pgpool2 | 1 core | 2G  | 50G  |
| 192.168.56.3 | microk8s-master-2 | 1 core | 2G  | 50G  |
| 192.168.56.4 | microk8s-master-3 | 1 core | 2G  | 50G  |

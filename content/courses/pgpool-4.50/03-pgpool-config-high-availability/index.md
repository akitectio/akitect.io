---
categories:
  - database
date: 2024-02-24T08:00:00+08:00
draft: false
featuredImage: /courses/pgpool-ii/03-pgpool-config-watchdog.png
images:
  - /labs/postgresql/postgresql-pgpool.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - postgresql
  - ubuntu
  - pgpool
title: Lesson 3 - Cấu hình High Availability 
url: /lesson-3-pgpool-config-high-availability
description: Cấu hình High Availability cho PGpool-II 
weight: 3
---

Trong bài học này, chúng ta sẽ cấu hình High Availability cho PGpool-II. PGpool-II là một công cụ giúp chúng ta cân bằng tải và sao chép dữ liệu giữa các máy chủ cơ sở dữ liệu PostgreSQL.


# Kiến trúc cài đặt

{{< figure src="./images/pgpool2-watchdog.png" >}}


Trước khi bắt đầu ta cần chuẩn bị 3 máy chủ

| IP           | Hostname   | vCPU   | RAM | DISK | OS           |
| ------------ | ---------- | ------ | --- | ---- | ------------ |
| 192.168.56.2 | server-01  | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.3 | server-02  | 4 core | 8G  | 50G  | Ubuntu 22.04 |
| 192.168.56.4 | server-03  | 4 core | 8G  | 50G  | Ubuntu 22.04 |

Và có Virtual IP: `192.168.56.10`

#### Bước 3: Cài đặt PostSgreSQL 16 

Trước khi cài đặt PGpool-II, bạn cần cài đặt PostgreSQL 16 trên tất cả các máy chủ. Bạn có thể tham khảo hướng dẫn sau: [Cài đặt PostgreSQL 16](/cai-dat-va-bao-mat-postgresql-16-tren-ubuntu-2204)

Cài dặt PGpool-II trên máy chủ `server-01`, `server-02` và `server-03`,Bạn tham khảo hướng dẫn sau: [Cài đặt PGpool-II](/lesson-1-cai-dat-pgpool-ii-tren-ubuntu-22-04)

##### 3.1 Phiên bản và cấu hình PostgreSQL

| Item           | Value   | Detail   |
| -------------- | ------- | -------- |
| PostgreSQL Version	        | 16      | -         |
| Port	    | 5432    | -         |
| Data Directory	| /var/lib/postgresql/16/main	| -         |
| Archive mode		| on	| /var/lib/pgsql/archivedir        |
| Replication Slots		| Enable	| -         |
| Start automatically			| Enable	| -         |

##### 3.2: Phiên bản và cấu hình PGpool-II

| Item           | Value   | Detail   |
| -------------- | ------- | -------- |
| PGpool-II Version	        | 4.50      | -         |
| Port	    | 9999    | Pgpool-II accepts connections         |
| Port	    | 9898    | PCP process accepts connections         |
| Port	    | 9000    | watchdog accepts connections         |
| Port	    | 9694    | UDP port for receiving Watchdog's heartbeat signal         |
| Config file		| /home/pgpool2/etc/pgpool.conf		| Pgpool-II config file         |
| Pgpool-II start user			| postgres (Pgpool-II 4.1 or later)			| Pgpool-II 4.0 or before, the default startup user is root         |
| Running mode				| streaming replication mode			| -         |
| Watchdog				| on			| Life check method: heartbeat         |
| Start automatically					| Enable				| -       |


##### 3.2: Danh sách Các script cần thiết

| Feature        | Script  | Detail   |
| -------------- | ------- | -------- |
| Failover	| /home/pgpool2/etc/failover.sh.sample	| Run by [failover_command](https://www.pgpool.net/docs/42/en/html/runtime-config-failover.html#GUC-FAILOVER-COMMAND)  to perform failover |
| Failover	| /home/pgpool2/etc/follow_primary.sh.sample	| Run by [follow_primary_command](https://www.pgpool.net/docs/42/en/html/runtime-config-failover.html#GUC-FOLLOW-PRIMARY-COMMAND) to synchronize the Standby with the new Primary after failover. |
| Online recovery		| /home/pgpool2/etc/health_check.sh.sample	| Run by [recovery_1st_stage_command](https://www.pgpool.net/docs/42/en/html/runtime-online-recovery.html#GUC-RECOVERY-1ST-STAGE-COMMAND) to recovery a Standby node |
| Online recovery		| /home/pgpool2/etc/health_check.sh.sample	| Run after [recovery_1st_stage_command](https://www.pgpool.net/docs/42/en/html/runtime-online-recovery.html#GUC-RECOVERY-1ST-STAGE-COMMAND) to start the Standby node |
| Watchdog	| /home/pgpool2/etc/health_check.sh.sample	| Run by [wd_escalation_command](https://www.pgpool.net/docs/42/en/html/runtime-watchdog-config.html#GUC-WD-ESCALATION-COMMAND) to switch the Active/Standby Pgpool-II safely |
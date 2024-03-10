---
categories:
  - database
date: 2024-02-24T08:00:00+08:00
draft: true
featuredImage: /courses/pgpool-ii/03-pgpool-config-watchdog.png
images:
  - /labs/postgresql/postgresql-pgpool.jpeg
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - database
  - postgresql
  - ubuntu
  - pgpool
title: Lesson 3 - Cấu hình High Availability (Watchdog)
url: /lesson-3-cau-hinh-high-availability-voi-pgpool-ii-watchdog
description: Cấu hình High Availability cho PGpool-II (Watchdog)
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


#### Bước 4: Cài đặt ban đầu cho PostgreSQL và tạo User

##### 4.1: Cài đặt ban đầu cho PostgreSQL

Trên 3 máy chủ `server-01`, `server-02` và `server-03`, Thiết lập sao chép phát trực tuyến PostgreSQL trên máy chủ chính. Trong ví dụ này, chúng tôi sử dụng tính năng lưu trữ WAL.

```bash
 su - postgres

mkdir /var/lib/pgsql/archivedir

```

Khởi tạo `PostgreSQL` trên máy chủ `server-01`:


```bash
su - postgres
/usr/pgsql-13/bin/initdb -D $PGDATA
```

Sau đó chúng ta chỉnh sửa file cấu hình `$PGDATA/postgresql.conf` trên server1 (chính) như sau. Bật `wal_log_hint` để sử dụng `pg_rewind`. Vì Sơ cấp có thể trở thành Dự phòng sau này nên chúng tôi đặt `hot_standby = on`.

```bash
listen_addresses = '*'
archive_mode = on
archive_command = 'cp "%p" "/var/lib/pgsql/archivedir/%f"'
max_wal_senders = 10
max_replication_slots = 10
wal_level = replica
hot_standby = on
wal_log_hints = on
```

Sử dụng chức năng `khôi phục trực tuyến (recovery online)` của `PGpool-II` để thiết lập máy chủ dự phòng sau khi máy chủ chính được khởi động.

Vì lý do bảo mật, Mình sẽ tạo một bản thay thế người dùng chỉ được sử dụng cho mục đích sao chép và một `pgpool` người dùng để kiểm tra độ trễ sao chép trực tuyến và kiểm tra tình trạng của `PGpool-II`.


##### 4.2: Tạo User

| Tên tài khoản  | Mật khẩu | Chi tiết |
| -------------- | -------- | -------- |
| repl_user	| repl_pass	| Người dùng sao chép PostgreSQL        |
| pgpool		| pgpool	| Người dùng kiểm tra tình trạng PGpool-II (health_check_user) và kiểm tra độ trễ sao chép (sr_check_user)        |
| postgres		| postgres	| Người dùng đang chạy khôi phục trực tuyến (recovery online)      |

```bash
psql -U postgres -p 5432
SET password_encryption = 'scram-sha-256';
CREATE ROLE pgpool WITH LOGIN;
CREATE ROLE repl WITH REPLICATION LOGIN;
\password pgpool
\password repl
\password postgres
 ```

 Nếu bạn muốn hiển thị cột `replication_state` và `replication_sync_state` trong kết quả lệnh `SHOW POOL NODES,` vai trò pgpool cần phải là siêu người dùng PostgreSQL hoặc hoặc trong nhóm `pg_monitor` (Pgpool-II 4.1 trở lên). Cấp `pg_monitor` cho `pgpool`:

```bash 
GRANT pg_monitor TO pgpool;
```


Lưu ý: Nếu bạn dự định sử dụng Detach_false_primary(Pgpool-II 4.0 trở lên), vai trò "pgpool" cần phải là siêu người dùng PostgreSQL hoặc hoặc trong nhóm "pg_monitor" để sử dụng tính năng này. Cấp "pg_monitor" cho "pgpool":

Giả sử rằng tất cả các máy chủ PGpool-II và máy chủ PostgreSQL đều nằm trong cùng một mạng con và chỉnh sửa pg_hba.conf để kích hoạt phương thức xác thực `scram-sha-256`.

```bash
host    all             all             samenet                 scram-sha-256
host    replication     all             samenet                 scram-sha-256
```


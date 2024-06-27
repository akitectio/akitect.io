
#### Bước 2: Cài đặt `pgpool_recovery`

`PGpool-II` yêu cầu bao gồm các chức năng `pgpool_recovery`, `pgpool_remote_start` và `pgpool_switch_xlog` khi sử dụng khôi phục trực tuyến như được giải thích sau. Hơn nữa, công cụ quản lý pgpoolAdmin còn hỗ trợ khả năng dừng, khởi động lại hoặc tải lại phiên bản PostgreSQL trên màn hình bằng cách sử dụng `pgpool_pgctl`. Việc cài đặt các chức năng này trong template1 ban đầu là đủ, không cần thiết phải cài đặt chúng trong tất cả các cơ sở dữ liệu.

Trước tiên ta cần cài đặt `postgresql-server-dev-16`: 

##### 2.1: Thêm key và repo của PostgreSQL 16

```bash
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
apt update
```

##### 2.2: Cài đặt `postgresql-server-dev-16`

```bash
sudo apt install -y postgresql-server-dev-16 -y
```

##### 2.3: Cài đặt `pgpool_recovery`

Tiếp theo, chúng ta sẽ cài đặt `pgpool_recovery` bằng cách thực hiện các bước sau:

```bash
# di chuyển đến thư mục pgpool-recovery
 cd pgpool-II-4.5.0/src/sql/pgpool-recovery

# cài đặt
make && sudo make install
```

Sau khi build thành công ta sẽ có file `pgpool_recovery.so` trong thư mục `pgpool-II-4.5.0/src/sql/pgpool-recovery`

```bash
ls -la
```

{{< figure src="/images/pgpool-recovery.jpg" >}}

##### 2.4: Cấu hình sao chép `pgpool_recovery` từ máy chủ `pgpool2` tới máy chủ `postgresql-master` bằng lệnh `scp`: 

Sao chép file `pgpool_recovery.so` và `pgpool_recovery.sql` từ máy chủ `pgpool2` tới máy chủ `postgresql-master` bằng lệnh `scp`:

  ```bash
  scp pgpool-recovery.so ubuntu@192.168.56.2:/home/ubuntu 
  scp pgpool-recovery.sql ubuntu@192.168.56.2:/home/ubuntu 
  scp pgpool_recovery.control ubuntu@192.168.56.2:/home/ubuntu 
  ```

Tiếp tục ssh vào máy chủ `postgresql-master`:

```bash
ssh ubuntu@192.168.56.2
```

Sao chép file `pgpool_recovery.so` và `pgpool_recovery.sql` vào thư mục `/usr/lib/postgresql/16/lib` và `/usr/share/postgresql/16/extension`:

```bash
sudo mv pgpool-recovery.so /usr/lib/postgresql/16/lib
sudo mv pgpool-recovery.sql /usr/share/postgresql/16/extension
sudo mv pgpool_recovery.control /usr/share/postgresql/16/extension
```

Sau khi cài đặt, chúng ta sẽ cấu hình `pgpool_recovery` ở máy chủ `postgresql-master` bằng cách thực hiện các bước sau:

```bash
 sudo -u postgres psql -d template1 -f /usr/share/postgresql/16/extension/pgpool-recovery.sql 
 ```

{{< figure src="/images/pgpool-recovery-sql.jpg" >}}

Trong đó `template1` là cơ sở dữ liệu mẫu mà chúng ta cài đặt `pgpool_recovery`


#### Bước 6: Cấu hình PGpool cho PostgreSQL có tính sẵn sàng cao (High Availability)

##### 6.1: Cấu hình `pgpool.conf` trên máy chủ `pgpool2`

Sao chép  `failover.sh` trên máy chủ `pgpool2`:

```bash
sudo cp /home/pgpool2/etc/failover.sh.sample /etc/pgpool2/failover.sh
```

Sau đó, thêm quyền thực thi vào tệp `failover.sh`:

```bash
sudo chmod +x /etc/pgpool2/failover.sh
```

Sau đó mở tệp `pgpool.conf`:

```bash
sudo nano /etc/pgpool2/pgpool.conf
```

Thêm cấu hình sau vào tệp `pgpool.conf`:

```bash
failover_command = '/etc/pgpool2/failover.sh %d  /home/ubuntu/postgresql/master/down.trg'
```

##### 6.1: Cấu hình `postgresql.conf` trên máy chủ `postgresql-master`

Thêm cấu hình sau vào tệp `postgresql.conf`:

```bash
synchonous_commit = remote_apply
```
`synchonous_commit` bao gồm các giá trị sau: 

- `on`: Đảm bảo rằng mỗi giao dịch đã được xác nhận trước khi kết thúc.
<!-- 
```mermaid
sequenceDiagram
    participant A as "Client"
    participant B as "Primary"
    participant C as "Standby"

    A->B: "Bắt đầu giao dịch"
    B->B: "Ghi vào WAL"
    B->C: "Ghi vào WAL"
    C->C: "Ghi vào bộ nhớ đệm"
    C->B: "Xác nhận ghi"
    B->A: "Xác nhận"
``` -->

- `remote_write`: Đảm bảo rằng mỗi giao dịch đã được ghi vào bộ nhớ đệm của máy chủ phụ trước khi kết thúc.
<!-- 
```mermaid
sequenceDiagram
    participant A as "Client"
    participant B as "Primary"
    participant C as "Standby"

    A->B: "Bắt đầu giao dịch"
    B->B: "Ghi vào WAL"
    B->C: "Ghi vào WAL"
    C->C: "Ghi vào bộ nhớ đệm"
    C->B: "Xác nhận ghi"
    B->A: "Xác nhận"
``` -->

- `remote_apply`: Đảm bảo rằng mỗi giao dịch đã được áp dụng trên máy chủ phụ trước khi kết thúc.

<!-- ```mermaid
sequenceDiagram
    participant A as "Client"
    participant B as "Primary"
    participant C as "Standby"

    A->B: "Bắt đầu giao dịch"
    B->B: "Ghi vào WAL"
    B->C: "Ghi vào WAL"
    C->C: "Áp dụng vào cơ sở dữ liệu"
    C->B: "Xác nhận áp dụng"
    B->A: "Xác nhận"
```
 -->


##### 6.2:  Cấu hình  `postgresql.conf` trên máy chủ `postgresql-slave-01`

Thêm cấu hình sau vào tệp `postgresql.conf`:

```bash
promote_trigger_file = '/home/ubuntu/postgresql/master/down.trg'
```

Sau đó, khởi động lại dịch vụ PostgreSQL trên máy chủ `postgresql-slave-01`:

```bash
sudo systemctl restart postgresql
```

##### 6.3:  Cấu hình  `postgresql.conf` trên máy chủ `postgresql-slave-02`

Thêm cấu hình sau vào tệp `postgresql.conf`:

```bash
promote_trigger_file = '/home/ubuntu/postgresql/master/down.trg'
```

Sau đó, khởi động lại dịch vụ PostgreSQL trên máy chủ `postgresql-slave-02`:

```bash
sudo systemctl restart postgresql
```


Như vậy chúng ta đã cấu hình xong PGpool-II cho PostgreSQL có tính sẵn sàng cao (High Availability).

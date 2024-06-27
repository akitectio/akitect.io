---
categories:
  - linux
  - network
date: 2023-12-09T15:16:45+0700
description: TCP/IP là bộ giao thức mạng chính đứng sau Internet, bao gồm TCP (Transmission Control Protocol) và IP (Internet Protocol), đảm bảo truyền tải dữ liệu đáng tin cậy và hiệu quả.
draft: false
featuredImage: /series/network/so-luoc-ve-tcp-ip.webp
images:
  - /series/network/so-luoc-ve-tcp-ip.webp
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags:
  - tcp-ip
  - network-protocol
title: Sơ lược về TCP/IP
url: /so-luoc-ve-tcp-ip
weight: 1
---

## 1. Lịch sử phát triển

Lịch sử của mạng máy tính là một hành trình thú vị, từ những bước đầu tiên của việc kết nối máy tính với nhau đến sự phát triển của Internet như chúng ta biết ngày nay. Dưới đây là một sơ lược về lịch sử phát triển của mạng:

### Giai đoạn Đầu (Cuối những năm 1950 đến 1960)

- Lĩnh vực mạng máy tính bắt đầu như một phần của nghiên cứu về công nghệ viễn thông và máy tính. Các nhà nghiên cứu bắt đầu xem xét cách thức kết nối máy tính với nhau.

### Giai đoạn 2 (Cuối những năm 1960 đến 1970)

- Các nhà nghiên cứu bắt đầu xem xét cách thức kết nối các mạng máy tính với nhau. Các mạng máy tính đầu tiên được phát triển trong giai đoạn này là ARPANET và NPL.

* ARPANET : Advanced Research Projects Agency Network (Mạng Cơ quan Nghiên cứu Dự án Tiên tiến) được phát triển bởi Cơ quan Dự án Nghiên cứu Tiên tiến (ARPA) của Bộ Quốc phòng Hoa Kỳ (DoD) vào năm 1969. ARPANET là mạng máy tính đầu tiên sử dụng giao thức TCP/IP.
* NPL : National Physical Laboratory (Viện Vật lý Quốc gia) của Anh đã phát triển mạng máy tính đầu tiên của Anh vào năm 1969. Mạng này được gọi là NPL Network. NPL Network là mạng máy tính đầu tiên sử dụng giao thức TCP/IP.

### Sự Phát Triển của Mạng LAN (1970s)

- Các mạng LAN (Local Area Network - Mạng Cục bộ) được phát triển trong giai đoạn này. Các mạng LAN đầu tiên được phát triển là Ethernet và Token Ring.

### Sự Ra Đời của Internet (1980s)

- Các mạng máy tính được phát triển trong giai đoạn này là NSFNET, CSNET, BITNET, FidoNet và UUCP. Các mạng này được kết nối với nhau để tạo thành Internet.

### Sự Phát Triển của World Wide Web (1990s)

- World Wide Web (WWW) được phát triển trong giai đoạn này. World Wide Web là một hệ thống thông tin trên Internet cho phép các tài liệu được liên kết với nhau bằng các liên kết siêu văn bản và được truy cập bằng các trình duyệt web.

### Kỷ Nguyên Hiện Đại (2000s và Tiếp Theo)

- Sự Bùng Nổ của Internet: Internet trở thành một phần không thể thiếu trong cuộc sống hàng ngày, với sự phát triển của các dịch vụ trực tuyến, mạng xã hội, và thương mại điện tử.

- Công nghệ Mạng Mới: Sự phát triển của công nghệ không dây, như Wi-Fi và 3G/4G/5G, cùng với việc phát triển của Internet vạn vật (IoT), tiếp tục thúc đẩy sự phát triển của mạng máy tính.

## 2. Mô hình OSI

Mô hình OSI (Open Systems Interconnection) là một mô hình tham chiếu được đề xuất bởi Tổ chức Tiêu chuẩn Hóa Quốc tế (ISO) vào năm 1984. Mô hình OSI được sử dụng để mô tả cách thức một mạng máy tính hoạt động. Mô hình OSI được chia thành 7 lớp, mỗi lớp đại diện cho một tập hợp các chức năng có liên quan. Các lớp này được sắp xếp theo thứ tự từ lớp trên cùng đến lớp dưới cùng.

{{< figure src="/images/osi-model.webp" >}}

### 2.1. Lớp 7: Ứng dụng (Application)

Lớp ứng dụng là lớp trên cùng trong mô hình OSI. Lớp ứng dụng cung cấp các dịch vụ mạng cho người dùng. Các giao thức mạng được sử dụng trong lớp ứng dụng bao gồm HTTP, FTP, SMTP, POP3, IMAP, Telnet, SSH, DNS, DHCP, SNMP, và NTP.

### 2.2. Lớp 6: Trình diễn (Presentation)

Lớp trình diễn định dạng dữ liệu để truyền qua mạng. Các định dạng dữ liệu được sử dụng trong lớp trình diễn bao gồm ASCII, EBCDIC, và JPEG.

### 2.3. Lớp 5: Phiên (Session)

Lớp phiên thiết lập, duy trì, và đóng phiên giữa các máy tính. Các giao thức mạng được sử dụng trong lớp phiên bao gồm NetBIOS, PPTP, và SOCKS.

### 2.4. Lớp 4: Vận chuyển (Transport)

Lớp vận chuyển đảm bảo việc truyền dữ liệu giữa các máy tính. Các giao thức mạng được sử dụng trong lớp vận chuyển bao gồm TCP và UDP.

### 2.5. Lớp 3: Mạng (Network)

Lớp mạng định tuyến dữ liệu giữa các máy tính. Các giao thức mạng được sử dụng trong lớp mạng bao gồm IP, ICMP, ARP, và RARP.

### 2.6. Lớp 2: Liên kết dữ liệu (Data Link)

Lớp liên kết dữ liệu đảm bảo việc truyền dữ liệu giữa các máy tính trên cùng một mạng. Các giao thức mạng được sử dụng trong lớp liên kết dữ liệu bao gồm Ethernet, Token Ring, và FDDI.

### 2.7. Lớp 1: Vật lý (Physical)

Lớp vật lý đảm bảo việc truyền dữ liệu giữa các máy tính. Các giao thức mạng được sử dụng trong lớp vật lý bao gồm RS-232, V.35, và Ethernet.

## 3. Mô hình TCP/IP

Mô hình TCP/IP (Transmission Control Protocol/Internet Protocol) là một mô hình tham chiếu được phát triển bởi Bộ Quốc phòng Hoa Kỳ (DoD) vào năm 1982. Mô hình TCP/IP được sử dụng để mô tả cách thức một mạng máy tính hoạt động. Mô hình TCP/IP được chia thành 4 lớp, mỗi lớp đại diện cho một tập hợp các chức năng có liên quan. Các lớp này được sắp xếp theo thứ tự từ lớp trên cùng đến lớp dưới cùng.

{{< figure src="/images/tcp-ip-model.webp" >}}

### 3.1. Lớp 4: Ứng dụng (Application)

Lớp ứng dụng là lớp trên cùng trong mô hình TCP/IP. Lớp ứng dụng cung cấp các dịch vụ mạng cho người dùng. Các giao thức mạng được sử dụng trong lớp ứng dụng bao gồm HTTP, FTP, SMTP, POP3, IMAP, Telnet, SSH, DNS, DHCP, SNMP, và NTP.

### 3.2. Lớp 3: Vận chuyển (Transport)

Lớp vận chuyển đảm bảo việc truyền dữ liệu giữa các máy tính. Các giao thức mạng được sử dụng trong lớp vận chuyển bao gồm TCP và UDP.

### 3.3. Lớp 2: Mạng (Internet)

Lớp mạng định tuyến dữ liệu giữa các máy tính. Các giao thức mạng được sử dụng trong lớp mạng bao gồm IP, ICMP, ARP, và RARP.

### 3.4. Lớp 1: Liên kết dữ liệu (Link)

Lớp liên kết dữ liệu đảm bảo việc truyền dữ liệu giữa các máy tính trên cùng một mạng. Các giao thức mạng được sử dụng trong lớp liên kết dữ liệu bao gồm Ethernet, Token Ring, và FDDI.

## 4. Các giao thức TCP/IP

### 4.1. IP (Internet Protocol)

IP (Internet Protocol) là một giao thức mạng được sử dụng để định tuyến dữ liệu giữa các máy tính. IP là một giao thức mạng lớp 3 trong mô hình TCP/IP. IP được sử dụng để định tuyến dữ liệu giữa các máy tính trên Internet. Các phiên bản của IP bao gồm IPv4 và IPv6.

### 4.2. ICMP (Internet Control Message Protocol)

ICMP (Internet Control Message Protocol) là một giao thức mạng được sử dụng để gửi thông báo lỗi và trạng thái giữa các máy tính. ICMP là một giao thức mạng lớp 3 trong mô hình TCP/IP. ICMP được sử dụng để gửi thông báo lỗi và trạng thái giữa các máy tính trên Internet.

### 4.3. ARP (Address Resolution Protocol)

ARP (Address Resolution Protocol) là một giao thức mạng được sử dụng để ánh xạ địa chỉ IP sang địa chỉ MAC. ARP là một giao thức mạng lớp 2 trong mô hình TCP/IP. ARP được sử dụng để ánh xạ địa chỉ IP sang địa chỉ MAC.

### 4.4. RARP (Reverse Address Resolution Protocol)

RARP (Reverse Address Resolution Protocol) là một giao thức mạng được sử dụng để ánh xạ địa chỉ MAC sang địa chỉ IP. RARP là một giao thức mạng lớp 2 trong mô hình TCP/IP. RARP được sử dụng để ánh xạ địa chỉ MAC sang địa chỉ IP.

### 4.5. TCP (Transmission Control Protocol)

TCP (Transmission Control Protocol) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. TCP là một giao thức mạng lớp 4 trong mô hình TCP/IP. TCP được sử dụng để truyền dữ liệu giữa các máy tính trên Internet.

### 4.6. UDP (User Datagram Protocol)

UDP (User Datagram Protocol) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. UDP là một giao thức mạng lớp 4 trong mô hình TCP/IP. UDP được sử dụng để truyền dữ liệu giữa các máy tính trên Internet.

### 4.7. DHCP (Dynamic Host Configuration Protocol)

DHCP (Dynamic Host Configuration Protocol) là một giao thức mạng được sử dụng để cấu hình địa chỉ IP cho các máy tính. DHCP là một giao thức mạng lớp 7 trong mô hình TCP/IP. DHCP được sử dụng để cấu hình địa chỉ IP cho các máy tính trên Internet.

### 4.8. DNS (Domain Name System)

DNS (Domain Name System) là một giao thức mạng được sử dụng để ánh xạ tên miền sang địa chỉ IP. DNS là một giao thức mạng lớp 7 trong mô hình TCP/IP. DNS được sử dụng để ánh xạ tên miền sang địa chỉ IP.

### 4.9. FTP (File Transfer Protocol)

FTP (File Transfer Protocol) là một giao thức mạng được sử dụng để truyền tập tin giữa các máy tính. FTP là một giao thức mạng lớp 7 trong mô hình TCP/IP. FTP được sử dụng để truyền tập tin giữa các máy tính trên Internet.

### 4.10. HTTP (Hypertext Transfer Protocol)

HTTP (Hypertext Transfer Protocol) là một giao thức mạng được sử dụng để truyền tài liệu giữa các máy tính. HTTP là một giao thức mạng lớp 7 trong mô hình TCP/IP. HTTP được sử dụng để truyền tài liệu giữa các máy tính trên Internet.

### 4.11. HTTPS (Hypertext Transfer Protocol Secure)

HTTPS (Hypertext Transfer Protocol Secure) là một giao thức mạng được sử dụng để truyền tài liệu giữa các máy tính. HTTPS là một giao thức mạng lớp 7 trong mô hình TCP/IP. HTTPS được sử dụng để truyền tài liệu giữa các máy tính trên Internet.

### 4.12. IMAP (Internet Message Access Protocol)

IMAP (Internet Message Access Protocol) là một giao thức mạng được sử dụng để truyền thư giữa các máy tính. IMAP là một giao thức mạng lớp 7 trong mô hình TCP/IP. IMAP được sử dụng để truyền thư giữa các máy tính trên Internet.

### 4.13. POP3 (Post Office Protocol 3)

POP3 (Post Office Protocol 3) là một giao thức mạng được sử dụng để truyền thư giữa các máy tính. POP3 là một giao thức mạng lớp 7 trong mô hình TCP/IP. POP3 được sử dụng để truyền thư giữa các máy tính trên Internet.

## 5. Các giao thức mạng khác

### 5.1. Ethernet

Ethernet là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. Ethernet là một giao thức mạng lớp 2 trong mô hình OSI. Ethernet được sử dụng để truyền dữ liệu giữa các máy tính trên mạng LAN.

### 5.2. Token Ring

Token Ring là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. Token Ring là một giao thức mạng lớp 2 trong mô hình OSI. Token Ring được sử dụng để truyền dữ liệu giữa các máy tính trên mạng LAN.

### 5.3. FDDI (Fiber Distributed Data Interface)

FDDI (Fiber Distributed Data Interface) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. FDDI là một giao thức mạng lớp 2 trong mô hình OSI. FDDI được sử dụng để truyền dữ liệu giữa các máy tính trên mạng LAN.

### 5.4. PPP (Point-to-Point Protocol)

PPP (Point-to-Point Protocol) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. PPP là một giao thức mạng lớp 2 trong mô hình OSI. PPP được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.5. SLIP (Serial Line Internet Protocol)

SLIP (Serial Line Internet Protocol) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. SLIP là một giao thức mạng lớp 2 trong mô hình OSI. SLIP được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.6. PPPoE (Point-to-Point Protocol over Ethernet)

PPPoE (Point-to-Point Protocol over Ethernet) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. PPPoE là một giao thức mạng lớp 2 trong mô hình OSI. PPPoE được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.7. ISDN (Integrated Services Digital Network)

ISDN (Integrated Services Digital Network) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. ISDN là một giao thức mạng lớp 1 trong mô hình OSI. ISDN được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.8. X.25

X.25 là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. X.25 là một giao thức mạng lớp 1 trong mô hình OSI. X.25 được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.9. Frame Relay

Frame Relay là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. Frame Relay là một giao thức mạng lớp 1 trong mô hình OSI. Frame Relay được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.10. ATM (Asynchronous Transfer Mode)

ATM (Asynchronous Transfer Mode) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. ATM là một giao thức mạng lớp 1 trong mô hình OSI. ATM được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.11. MPLS (Multiprotocol Label Switching)

MPLS (Multiprotocol Label Switching) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. MPLS là một giao thức mạng lớp 1 trong mô hình OSI. MPLS được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.12. SD-WAN (Software-Defined Wide Area Network)

SD-WAN (Software-Defined Wide Area Network) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. SD-WAN là một giao thức mạng lớp 1 trong mô hình OSI. SD-WAN được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.13. VPN (Virtual Private Network)

VPN (Virtual Private Network) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. VPN là một giao thức mạng lớp 1 trong mô hình OSI. VPN được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.14. GRE (Generic Routing Encapsulation)

GRE (Generic Routing Encapsulation) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. GRE là một giao thức mạng lớp 1 trong mô hình OSI. GRE được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.15. IPSec (Internet Protocol Security)

IPSec (Internet Protocol Security) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. IPSec là một giao thức mạng lớp 1 trong mô hình OSI. IPSec được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.16. SSL (Secure Sockets Layer)

SSL (Secure Sockets Layer) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. SSL là một giao thức mạng lớp 1 trong mô hình OSI. SSL được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.17. TLS (Transport Layer Security)

TLS (Transport Layer Security) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. TLS là một giao thức mạng lớp 1 trong mô hình OSI. TLS được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.18. SSH (Secure Shell)

SSH (Secure Shell) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. SSH là một giao thức mạng lớp 1 trong mô hình OSI. SSH được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.19. Telnet

Telnet là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. Telnet là một giao thức mạng lớp 1 trong mô hình OSI. Telnet được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.20. SNMP (Simple Network Management Protocol)

SNMP (Simple Network Management Protocol) là một giao thức mạng được sử dụng để quản lý mạng. SNMP là một giao thức mạng lớp 1 trong mô hình OSI. SNMP được sử dụng để quản lý mạng trên mạng WAN.

### 5.21. NTP (Network Time Protocol)

NTP (Network Time Protocol) là một giao thức mạng được sử dụng để đồng bộ hóa thời gian giữa các máy tính. NTP là một giao thức mạng lớp 1 trong mô hình OSI. NTP được sử dụng để đồng bộ hóa thời gian giữa các máy tính trên mạng WAN.

### 5.22. RIP (Routing Information Protocol)

RIP (Routing Information Protocol) là một giao thức mạng được sử dụng để định tuyến dữ liệu giữa các máy tính. RIP là một giao thức mạng lớp 1 trong mô hình OSI. RIP được sử dụng để định tuyến dữ liệu giữa các máy tính trên mạng WAN.

### 5.23. OSPF (Open Shortest Path First)

OSPF (Open Shortest Path First) là một giao thức mạng được sử dụng để định tuyến dữ liệu giữa các máy tính. OSPF là một giao thức mạng lớp 1 trong mô hình OSI. OSPF được sử dụng để định tuyến dữ liệu giữa các máy tính trên mạng WAN.

### 5.24. BGP (Border Gateway Protocol)

BGP (Border Gateway Protocol) là một giao thức mạng được sử dụng để định tuyến dữ liệu giữa các máy tính. BGP là một giao thức mạng lớp 1 trong mô hình OSI. BGP được sử dụng để định tuyến dữ liệu giữa các máy tính trên mạng WAN.

### 5.25. EIGRP (Enhanced Interior Gateway Routing Protocol)

EIGRP (Enhanced Interior Gateway Routing Protocol) là một giao thức mạng được sử dụng để định tuyến dữ liệu giữa các máy tính. EIGRP là một giao thức mạng lớp 1 trong mô hình OSI. EIGRP được sử dụng để định tuyến dữ liệu giữa các máy tính trên mạng WAN.

### 5.26. VRRP (Virtual Router Redundancy Protocol)

VRRP (Virtual Router Redundancy Protocol) là một giao thức mạng được sử dụng để định tuyến dữ liệu giữa các máy tính. VRRP là một giao thức mạng lớp 1 trong mô hình OSI. VRRP được sử dụng để định tuyến dữ liệu giữa các máy tính trên mạng WAN.

### 5.27. HSRP (Hot Standby Router Protocol)

HSRP (Hot Standby Router Protocol) là một giao thức mạng được sử dụng để định tuyến dữ liệu giữa các máy tính. HSRP là một giao thức mạng lớp 1 trong mô hình OSI. HSRP được sử dụng để định tuyến dữ liệu giữa các máy tính trên mạng WAN.

### 5.27. IPX (Internetwork Packet Exchange)

IPX (Internetwork Packet Exchange) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. IPX là một giao thức mạng lớp 1 trong mô hình OSI. IPX được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.28. SPX (Sequenced Packet Exchange)

SPX (Sequenced Packet Exchange) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. SPX là một giao thức mạng lớp 1 trong mô hình OSI. SPX được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.29. NetBEUI (NetBIOS Extended User Interface)

NetBEUI (NetBIOS Extended User Interface) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. NetBEUI là một giao thức mạng lớp 1 trong mô hình OSI. NetBEUI được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.30. IPX/SPX (Internetwork Packet Exchange/Sequenced Packet Exchange)

IPX/SPX (Internetwork Packet Exchange/Sequenced Packet Exchange) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. IPX/SPX là một giao thức mạng lớp 1 trong mô hình OSI. IPX/SPX được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.31. AppleTalk

AppleTalk là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. AppleTalk là một giao thức mạng lớp 1 trong mô hình OSI. AppleTalk được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

### 5.32. NetBIOS (Network Basic Input/Output System)

NetBIOS (Network Basic Input/Output System) là một giao thức mạng được sử dụng để truyền dữ liệu giữa các máy tính. NetBIOS là một giao thức mạng lớp 1 trong mô hình OSI. NetBIOS được sử dụng để truyền dữ liệu giữa các máy tính trên mạng WAN.

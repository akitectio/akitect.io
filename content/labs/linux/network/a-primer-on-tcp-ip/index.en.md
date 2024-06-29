---
categories: [linux, network]
date: 2023-12-09T15:16:45+0700
description: TCP/IP is the main network protocol suite behind the Internet, including TCP (Transmission Control Protocol) and IP (Internet Protocol), ensuring reliable and efficient data transmission.
draft: false
featuredImage: /series/network/so-luoc-ve-tcp-ip.webp
images: [/series/network/so-luoc-ve-tcp-ip.webp]
license: <a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>
tags: [tcp-ip, network-protocol]
title: An Overview of TCP/IP
weight: 1
---

## 1. History of Development

The history of computer networking is a fascinating journey, from the early steps of connecting computers together to the development of the Internet as we know it today. Below is a brief overview of the history of network development:

### Early Stage (Late 1950s to 1960s)

-   The field of computer networking began as part of research into telecommunications and computers. Researchers began to consider how to connect computers together.

### Stage 2 (Late 1960s to 1970s)

-   Researchers began to consider how to connect computer networks together. The first computer networks developed during this period were ARPANET and NPL.


-   ARPANET: Advanced Research Projects Agency Network was developed by the Advanced Research Projects Agency (ARPA) of the United States Department of Defense (DoD) in 1969. ARPANET was the first computer network to use the TCP/IP protocol.
-   NPL: The National Physical Laboratory of the UK developed the UK's first computer network in 1969. This network was called the NPL Network. The NPL Network was the first computer network to use the TCP/IP protocol.

### Development of LANs (1970s)

-   Local Area Networks (LANs) were developed during this period. The first LANs developed were Ethernet and Token Ring.

### Birth of the Internet (1980s)

-   The computer networks developed during this period were NSFNET, CSNET, BITNET, FidoNet, and UUCP. These networks were interconnected to form the Internet.

### Development of the World Wide Web (1990s)

-   The World Wide Web (WWW) was developed during this period. The World Wide Web is an information system on the Internet that allows documents to be linked together by hyperlinks and accessed by web browsers.

### Modern Era (2000s and Beyond)

-   Internet Explosion: The Internet has become an indispensable part of daily life, with the development of online services, social networks, and e-commerce.

-   New Network Technology: The development of wireless technology, such as Wi-Fi and 3G/4G/5G, along with the development of the Internet of Things (IoT), continues to drive the development of computer networking.

## 2. OSI Model

The OSI (Open Systems Interconnection) model is a reference model proposed by the International Organization for Standardization (ISO) in 1984. The OSI model is used to describe how a computer network operates. The OSI model is divided into 7 layers, each representing a set of related functions. These layers are arranged in order from the top layer to the bottom layer.

{{< figure src="./images/osi-model.webp" >}}

### 2.1. Layer 7: Application

The application layer is the top layer in the OSI model. The application layer provides network services to users. Network protocols used in the application layer include HTTP, FTP, SMTP, POP3, IMAP, Telnet, SSH, DNS, DHCP, SNMP, and NTP.

### 2.2. Layer 6: Presentation

The presentation layer formats data for transmission over the network. Data formats used in the presentation layer include ASCII, EBCDIC, and JPEG.

### 2.3. Layer 5: Session

The session layer establishes, maintains, and closes sessions between computers. Network protocols used in the session layer include NetBIOS, PPTP, and SOCKS.

### 2.4. Layer 4: Transport

The transport layer ensures data transmission between computers. Network protocols used in the transport layer include TCP and UDP.

### 2.5. Layer 3: Network

The network layer routes data between computers. Network protocols used in the network layer include IP, ICMP, ARP, and RARP.

### 2.6. Layer 2: Data Link

The data link layer ensures data transmission between computers on the same network. Network protocols used in the data link layer include Ethernet, Token Ring, and FDDI.

### 2.7. Layer 1: Physical

The physical layer ensures data transmission between computers. Network protocols used in the physical layer include RS-232, V.35, and Ethernet.

## 3. TCP/IP Model

The TCP/IP (Transmission Control Protocol/Internet Protocol) model is a reference model developed by the United States Department of Defense (DoD) in 1982. The TCP/IP model is used to describe how a computer network operates. The TCP/IP model is divided into 4 layers, each representing a set of related functions. These layers are arranged in order from the top layer to the bottom layer.

{{< figure src="./images/tcp-ip-model.webp" >}}

### 3.1. Layer 4: Application

The application layer is the top layer in the TCP/IP model. The application layer provides network services to users. Network protocols used in the application layer include HTTP, FTP, SMTP, POP3, IMAP, Telnet, SSH, DNS, DHCP, SNMP, and NTP.

### 3.2. Layer 3: Transport

The transport layer ensures data transmission between computers. Network protocols used in the transport layer include TCP and UDP.

### 3.3. Layer 2: Internet

The internet layer routes data between computers. Network protocols used in the internet layer include IP, ICMP, ARP, and RARP.

### 3.4. Layer 1: Link

The link layer ensures data transmission between computers on the same network. Network protocols used in the link layer include Ethernet, Token Ring, and FDDI.

## 4. TCP/IP Protocols

### 4.1. IP (Internet Protocol)

IP (Internet Protocol) is a network protocol used to route data between computers. IP is a layer 3 network protocol in the TCP/IP model. IP is used to route data between computers on the Internet. Versions of IP include IPv4 and IPv6.

### 4.2. ICMP (Internet Control Message Protocol)

ICMP (Internet Control Message Protocol) is a network protocol used to send error messages and status between computers. ICMP is a layer 3 network protocol in the TCP/IP model. ICMP is used to send error messages and status between computers on the Internet.

### 4.3. ARP (Address Resolution Protocol)

ARP (Address Resolution Protocol) is a network protocol used to map IP addresses to MAC addresses. ARP is a layer 2 network protocol in the TCP/IP model. ARP is used to map IP addresses to MAC addresses.

### 4.4. RARP (Reverse Address Resolution Protocol)

RARP (Reverse Address Resolution Protocol) is a network protocol used to map MAC addresses to IP addresses. RARP is a layer 2 network protocol in the TCP/IP model. RARP is used to map MAC addresses to IP addresses.

### 4.5. TCP (Transmission Control Protocol)

TCP (Transmission Control Protocol) is a network protocol used to transmit data between computers. TCP is a layer 4 network protocol in the TCP/IP model. TCP is used to transmit data between computers on the Internet.

### 4.6. UDP (User Datagram Protocol)

UDP (User Datagram Protocol) is a network protocol used to transmit data between computers. UDP is a layer 4 network protocol in the TCP/IP model. UDP is used to transmit data between computers on the Internet.

### 4.7. DHCP (Dynamic Host Configuration Protocol)

DHCP (Dynamic Host Configuration Protocol) is a network protocol used to configure IP addresses for computers. DHCP is a layer 7 network protocol in the TCP/IP model. DHCP is used to configure IP addresses for computers on the Internet.

### 4.8. DNS (Domain Name System)

DNS (Domain Name System) is a network protocol used to map domain names to IP addresses. DNS is a layer 7 network protocol in the TCP/IP model. DNS is used to map domain names to IP addresses.

### 4.9. FTP (File Transfer Protocol)

FTP (File Transfer Protocol) is a network protocol used to transfer files between computers. FTP is a layer 7 network protocol in the TCP/IP model. FTP is used to transfer files between computers on the Internet.

### 4.10. HTTP (Hypertext Transfer Protocol)

HTTP (Hypertext Transfer Protocol) is a network protocol used to transmit documents between computers. HTTP is a layer 7 network protocol in the TCP/IP model. HTTP is used to transmit documents between computers on the Internet.

### 4.11. HTTPS (Hypertext Transfer Protocol Secure)

HTTPS (Hypertext Transfer Protocol Secure) is a network protocol used to transmit documents between computers. HTTPS is a layer 7 network protocol in the TCP/IP model. HTTPS is used to transmit documents between computers on the Internet.

### 4.12. IMAP (Internet Message Access Protocol)

IMAP (Internet Message Access Protocol) is a network protocol used to transmit mail between computers. IMAP is a layer 7 network protocol in the TCP/IP model. IMAP is used to transmit mail between computers on the Internet.

### 4.13. POP3 (Post Office Protocol 3)

POP3 (Post Office Protocol 3) is a network protocol used to transmit mail between computers. POP3 is a layer 7 network protocol in the TCP/IP model. POP3 is used to transmit mail between computers on the Internet.

## 5. Other Network Protocols

### 5.1. Ethernet

Ethernet is a network protocol used to transmit data between computers. Ethernet is a layer 2 network protocol in the OSI model. Ethernet is used to transmit data between computers on a LAN network.

### 5.2. Token Ring

Token Ring is a network protocol used to transmit data between computers. Token Ring is a layer 2 network protocol in the OSI model. Token Ring is used to transmit data between computers on a LAN network.

### 5.3. FDDI (Fiber Distributed Data Interface)

FDDI (Fiber Distributed Data Interface) is a network protocol used to transmit data between computers. FDDI is a layer 2 network protocol in the OSI model. FDDI is used to transmit data between computers on a LAN network.

### 5.4. PPP (Point-to-Point Protocol)

PPP (Point-to-Point Protocol) is a network protocol used to transmit data between computers. PPP is a layer 2 network protocol in the OSI model. PPP is used to transmit data between computers on a WAN network.

### 5.5. SLIP (Serial Line Internet Protocol)

SLIP (Serial Line Internet Protocol) is a network protocol used to transmit data between computers. SLIP is a layer 2 network protocol in the OSI model. SLIP is used to transmit data between computers on a WAN network.

### 5.6. PPPoE (Point-to-Point Protocol over Ethernet)

PPPoE (Point-to-Point Protocol over Ethernet) is a network protocol used to transmit data between computers. PPPoE is a layer 2 network protocol in the OSI model. PPPoE is used to transmit data between computers on a WAN network.

### 5.7. ISDN (Integrated Services Digital Network)

ISDN (Integrated Services Digital Network) is a network protocol used to transmit data between computers. ISDN is a layer 1 network protocol in the OSI model. ISDN is used to transmit data between computers on a WAN network.

### 5.8. X.25

X.25 is a network protocol used to transmit data between computers. X.25 is a layer 1 network protocol in the OSI model. X.25 is used to transmit data between computers on a WAN network.

### 5.9. Frame Relay

Frame Relay is a network protocol used to transmit data between computers. Frame Relay is a layer 1 network protocol in the OSI model. Frame Relay is used to transmit data between computers on a WAN network.

### 5.10. ATM (Asynchronous Transfer Mode)

ATM (Asynchronous Transfer Mode) is a network protocol used to transmit data between computers. ATM is a layer 1 network protocol in the OSI model. ATM is used to transmit data between computers on a WAN network.

### 5.11. MPLS (Multiprotocol Label Switching)

MPLS (Multiprotocol Label Switching) is a network protocol used to transmit data between computers. MPLS is a layer 1 network protocol in the OSI model. MPLS is used to transmit data between computers on a WAN network.

### 5.12. SD-WAN (Software-Defined Wide Area Network)

SD-WAN (Software-Defined Wide Area Network) is a network protocol used to transmit data between computers. SD-WAN is a layer 1 network protocol in the OSI model. SD-WAN is used to transmit data between computers on a WAN network.

### 5.13. VPN (Virtual Private Network)

VPN (Virtual Private Network) is a network protocol used to transmit data between computers. VPN is a layer 1 network protocol in the OSI model. VPN is used to transmit data between computers on a WAN network.

### 5.14. GRE (Generic Routing Encapsulation)

GRE (Generic Routing Encapsulation) is a network protocol used to transmit data between computers. GRE is a layer 1 network protocol in the OSI model. GRE is used to transmit data between computers on a WAN network.

### 5.15. IPSec (Internet Protocol Security)

IPSec (Internet Protocol Security) is a network protocol used to transmit data between computers. IPSec is a layer 1 network protocol in the OSI model. IPSec is used to transmit data between computers on a WAN network.

### 5.16. SSL (Secure Sockets Layer)

SSL (Secure Sockets Layer) is a network protocol used to transmit data between computers. SSL is a layer 1 network protocol in the OSI model. SSL is used to transmit data between computers on a WAN network.

### 5.17. TLS (Transport Layer Security)

TLS (Transport Layer Security) is a network protocol used to transmit data between computers. TLS is a layer 1 network protocol in the OSI model. TLS is used to transmit data between computers on a WAN network.

### 5.18. SSH (Secure Shell)

SSH (Secure Shell) is a network protocol used to transmit data between computers. SSH is a layer 1 network protocol in the OSI model. SSH is used to transmit data between computers on a WAN network.

### 5.19. Telnet

Telnet is a network protocol used to transmit data between computers. Telnet is a layer 1 network protocol in the OSI model. Telnet is used to transmit data between computers on a WAN network.

### 5.20. SNMP (Simple Network Management Protocol)

SNMP (Simple Network Management Protocol) is a network protocol used to manage networks. SNMP is a layer 1 network protocol in the OSI model. SNMP is used to manage networks on a WAN network.

### 5.21. NTP (Network Time Protocol)

NTP (Network Time Protocol) is a network protocol used to synchronize time between computers. NTP is a layer 1 network protocol in the OSI model. NTP is used to synchronize time between computers on a WAN network.

### 5.22. RIP (Routing Information Protocol)

RIP (Routing Information Protocol) is a network protocol used to route data between computers. RIP is a layer 1 network protocol in the OSI model. RIP is used to route data between computers on a WAN network.

### 5.23. OSPF (Open Shortest Path First)

OSPF (Open Shortest Path First) is a network protocol used to route data between computers. OSPF is a layer 1 network protocol in the OSI model. OSPF is used to route data between computers on a WAN network.

### 5.24. BGP (Border Gateway Protocol)

BGP (Border Gateway Protocol) is a network protocol used to route data between computers. BGP is a layer 1 network protocol in the OSI model. BGP is used to route data between computers on a WAN network.

### 5.25. EIGRP (Enhanced Interior Gateway Routing Protocol)

EIGRP (Enhanced Interior Gateway Routing Protocol) is a network protocol used to route data between computers. EIGRP is a layer 1 network protocol in the OSI model. EIGRP is used to route data between computers on a WAN network.

### 5.26. VRRP (Virtual Router Redundancy Protocol)

VRRP (Virtual Router Redundancy Protocol) is a network protocol used to route data between computers. VRRP is a layer 1 network protocol in the OSI model. VRRP is used to route data between computers on a WAN network.

### 5.27. HSRP (Hot Standby Router Protocol)

HSRP (Hot Standby Router Protocol) is a network protocol used to route data between computers. HSRP is a layer 1 network protocol in the OSI model. HSRP is used to route data between computers on a WAN network.

### 5.27. IPX (Internetwork Packet Exchange)

IPX (Internetwork Packet Exchange) is a network protocol used to transmit data between computers. IPX is a layer 1 network protocol in the OSI model. IPX is used to transmit data between computers on a WAN network.

### 5.28. SPX (Sequenced Packet Exchange)

SPX (Sequenced Packet Exchange) is a network protocol used to transmit data between computers. SPX is a layer 1 network protocol in the OSI model. SPX is used to transmit data between computers on a WAN network.

### 5.29. NetBEUI (NetBIOS Extended User Interface)

NetBEUI (NetBIOS Extended User Interface) is a network protocol used to transmit data between computers. NetBEUI is a layer 1 network protocol in the OSI model. NetBEUI is used to transmit data between computers on a WAN network.

### 5.30. IPX/SPX (Internetwork Packet Exchange/Sequenced Packet Exchange)

IPX/SPX (Internetwork Packet Exchange/Sequenced Packet Exchange) is a network protocol used to transmit data between computers. IPX/SPX is a layer 1 network protocol in the OSI model. IPX/SPX is used to transmit data between computers on a WAN network.

### 5.31. AppleTalk

AppleTalk is a network protocol used to transmit data between computers. AppleTalk is a layer 1 network protocol in the OSI model. AppleTalk is used to transmit data between computers on a WAN network.

### 5.32. NetBIOS (Network Basic Input/Output System)

NetBIOS (Network Basic Input/Output System) is a network protocol used to transmit data between computers. NetBIOS is a layer 1 network protocol in the OSI model. NetBIOS is used to transmit data between computers on a WAN network.

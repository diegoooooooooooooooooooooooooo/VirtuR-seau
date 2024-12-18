# Partie II. Router-on-a-stick

### A. VLANs

🌞 **Tests de `ping`**

```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.3.1.1/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20018
RHOST:PORT  : 127.0.0.1:20019
MTU         : 1500

PC1> ping 10.3.1.2

84 bytes from 10.3.1.2 icmp_seq=1 ttl=64 time=6.110 ms
84 bytes from 10.3.1.2 icmp_seq=2 ttl=64 time=5.640 ms
84 bytes from 10.3.1.2 icmp_seq=3 ttl=64 time=5.102 ms
84 bytes from 10.3.1.2 icmp_seq=4 ttl=64 time=4.249 ms
84 bytes from 10.3.1.2 icmp_seq=5 ttl=64 time=2.328 ms

PC1> ping 10.3.2.1

host (255.255.255.0) not reachable
```

### B. Routeur

➜ **On passe sur le routeur**

🌞 **Tests de `ping`**

```
R1#ping 10.3.1.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.1.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 56/62/68 ms
R1#ping 10.3.2.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.3.2.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 32/56/64 ms
```

```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.3.1.1/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20018
RHOST:PORT  : 127.0.0.1:20019
MTU         : 1500

PC1> ping 10.3.1.254

84 bytes from 10.3.1.254 icmp_seq=1 ttl=255 time=18.295 ms
84 bytes from 10.3.1.254 icmp_seq=2 ttl=255 time=15.464 ms
84 bytes from 10.3.1.254 icmp_seq=3 ttl=255 time=8.216 ms
84 bytes from 10.3.1.254 icmp_seq=4 ttl=255 time=12.209 ms
84 bytes from 10.3.1.254 icmp_seq=5 ttl=255 time=15.884 ms
```

```
PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.3.2.1/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20020
RHOST:PORT  : 127.0.0.1:20021
MTU         : 1500

PC2> ping 10.3.2.254

84 bytes from 10.3.2.254 icmp_seq=1 ttl=255 time=20.794 ms
84 bytes from 10.3.2.254 icmp_seq=2 ttl=255 time=10.131 ms
84 bytes from 10.3.2.254 icmp_seq=3 ttl=255 time=10.896 ms
84 bytes from 10.3.2.254 icmp_seq=4 ttl=255 time=3.406 ms
84 bytes from 10.3.2.254 icmp_seq=5 ttl=255 time=11.707 ms
```

➜ **Configuration du NAT**

- configurez du NAT sur le routeur pour que les clients aient un accès Internet

🌞 **Tests de `ping`**

```
R1#ping 8.8.8.8

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 60/81/104 ms
```
```
PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.3.1.1/24
GATEWAY     : 10.3.1.254
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20018
RHOST:PORT  : 127.0.0.1:20019
MTU         : 1500

PC1> ping 8.8.8.8

84 bytes from 8.8.8.8 icmp_seq=1 ttl=116 time=42.776 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=116 time=26.749 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=116 time=44.577 ms
84 bytes from 8.8.8.8 icmp_seq=4 ttl=116 time=29.036 ms
84 bytes from 8.8.8.8 icmp_seq=5 ttl=116 time=47.411 ms
```
```
PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.3.2.1/24
GATEWAY     : 10.3.2.254
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20020
RHOST:PORT  : 127.0.0.1:20021
MTU         : 1500

PC2> ping 8.8.8.8

84 bytes from 8.8.8.8 icmp_seq=1 ttl=116 time=44.163 ms
84 bytes from 8.8.8.8 icmp_seq=2 ttl=116 time=41.025 ms
84 bytes from 8.8.8.8 icmp_seq=3 ttl=116 time=38.821 ms
84 bytes from 8.8.8.8 icmp_seq=4 ttl=116 time=42.750 ms
84 bytes from 8.8.8.8 icmp_seq=5 ttl=116 time=41.994 ms
```

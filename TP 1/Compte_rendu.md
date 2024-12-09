# TP1

🌞 **Déterminer l'adresse MAC de vos deux machines**


```
pc1> show ip

NAME        : pc1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20002
RHOST:PORT  : 127.0.0.1:20003
MTU         : 1500
```


```
pc2> show ip

NAME        : pc2[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20004
RHOST:PORT  : 127.0.0.1:20005
MTU         : 1500
```



🌞 **Définir une IP statique sur les deux machines**

Clique droit sur la machine 1 -> Edit config -> Ajout de la ligne
```
ip 10.1.1.1 255.255.255.0
```
Clique droit sur la machine 2 -> Edit config -> Ajout de la ligne
```
ip 10.1.1.2 255.255.255.0
```
```
pc1> show ip

NAME        : pc1[1]
IP/MASK     : 10.1.1.1/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20002
RHOST:PORT  : 127.0.0.1:20003
MTU         : 1500
```


```
pc2> show ip

NAME        : pc2[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20004
RHOST:PORT  : 127.0.0.1:20005
MTU         : 1500
```

 

🌞 **Effectuer un `ping` d'une machine à l'autre**

```
pc1> ping 10.1.1.2

84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=2.068 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=1.429 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=2.576 ms
84 bytes from 10.1.1.2 icmp_seq=4 ttl=64 time=0.840 ms
84 bytes from 10.1.1.2 icmp_seq=5 ttl=64 time=0.534 ms
```

```
pc2> ping 10.1.1.1

84 bytes from 10.1.1.1 icmp_seq=1 ttl=64 time=1.340 ms
84 bytes from 10.1.1.1 icmp_seq=2 ttl=64 time=0.731 ms
84 bytes from 10.1.1.1 icmp_seq=3 ttl=64 time=0.636 ms
84 bytes from 10.1.1.1 icmp_seq=4 ttl=64 time=0.379 ms
84 bytes from 10.1.1.1 icmp_seq=5 ttl=64 time=0.458 ms
```


🌞 **Wireshark !**

[Wireshark Protocole ICMP](./ws_ping.pcapng)


🌞 **ARP**

```
pc1> arp

00:50:79:66:68:01  10.1.1.2 expires in 27 seconds
```
```
pc2> arp

00:50:79:66:68:00  10.1.1.1 expires in 113 seconds
```

# II. Ajoutons un switch


🌞 **Déterminer l'adresse MAC de vos trois machines**
```
pc1> show ip

NAME        : pc1[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20002
RHOST:PORT  : 127.0.0.1:20003
MTU         : 1500
```


```
pc2> show ip

NAME        : pc2[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20004
RHOST:PORT  : 127.0.0.1:20005
MTU         : 1500
```
```
pc3> show ip

NAME        : pc3[1]
IP/MASK     : 0.0.0.0/0
GATEWAY     : 0.0.0.0
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 20010
RHOST:PORT  : 127.0.0.1:20011
MTU         : 1500
```

🌞 **Définir une IP statique sur les trois machines**

```
pc1> show ip

NAME        : pc1[1]
IP/MASK     : 10.1.1.1/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:00
LPORT       : 20002
RHOST:PORT  : 127.0.0.1:20003
MTU         : 1500
```


```
pc2> show ip

NAME        : pc2[1]
IP/MASK     : 10.1.1.2/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:01
LPORT       : 20004
RHOST:PORT  : 127.0.0.1:20005
MTU         : 1500
```
```
pc3> show ip

NAME        : pc3[1]
IP/MASK     : 10.1.1.3/24
GATEWAY     : 255.255.255.0
DNS         :
MAC         : 00:50:79:66:68:02
LPORT       : 20010
RHOST:PORT  : 127.0.0.1:20011
MTU         : 1500
```

🌞 **Effectuer des `ping` d'une machine à l'autre**

```
pc1> ping 10.1.1.2

84 bytes from 10.1.1.2 icmp_seq=1 ttl=64 time=0.699 ms
84 bytes from 10.1.1.2 icmp_seq=2 ttl=64 time=0.615 ms
84 bytes from 10.1.1.2 icmp_seq=3 ttl=64 time=0.457 ms
84 bytes from 10.1.1.2 icmp_seq=4 ttl=64 time=0.813 ms
84 bytes from 10.1.1.2 icmp_seq=5 ttl=64 time=1.734 ms
```

```
pc2> ping 10.1.1.3

84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=0.198 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=0.400 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=2.004 ms
84 bytes from 10.1.1.3 icmp_seq=4 ttl=64 time=0.309 ms
84 bytes from 10.1.1.3 icmp_seq=5 ttl=64 time=1.474 ms
```
```
pc1> ping 10.1.1.3

84 bytes from 10.1.1.3 icmp_seq=1 ttl=64 time=1.530 ms
84 bytes from 10.1.1.3 icmp_seq=2 ttl=64 time=0.893 ms
84 bytes from 10.1.1.3 icmp_seq=3 ttl=64 time=1.489 ms
84 bytes from 10.1.1.3 icmp_seq=4 ttl=64 time=1.498 ms
84 bytes from 10.1.1.3 icmp_seq=5 ttl=64 time=0.906 ms
```


# III. Serveur DHCP

## 1. Legit

🌞 **Donner un accès Internet à la machine `dhcp.tp1.efrei`**

```
[diego@efrei-xmg5afau6~]$ ping google.com
PING google.com (216.58.214.174) 56(84) bytes of data.
64 bytes from mad01s26-in-f174.1e100.net (216.58.214.174): icmp_seq q = 1 ttl I = 63 time 38.0 ms
64 bytes from par10s42-in-f14.1e100.net (216.58.214.174): icmp_seq=2 ttl I = 63 time=28.4 ms
```

🌞 **Installer et configurer un serveur DHCP**

Fixer une IP fixe :
```
DEVICE=enp0s3
NAME=rocky

ONBOOT=yes
BOOTPROTO=static

IPADDR=10.1.1.253
NETMASK=255.255.255.0
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
subnet 10.1.1.0 netmask 255.255.255.0 {
    range 10.1.1.10 10.1.1.50;
    option routers 10.1.1.1;
    option broadcast-address 10.1.1.255;
}
```


🌞 **Récupérer une IP automatiquement depuis les 3 nodes**

```
pc1> dhcp
DORA IP 10.1.1.11/24 GW 10.1.1.1

pc1> show ip

NAME        : pc1[1]
IP/MASK     : 10.1.1.11/24
GATEWAY     : 10.1.1.1
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 43195, 43200/21600/37800
MAC         : 00:50:79:66:68:00
LPORT       : 20009
RHOST:PORT  : 127.0.0.1:20010
MTU         : 1500
```
```
pc2> dhcp
DORA IP 10.1.1.12/24 GW 10.1.1.1

pc2> show ip

NAME        : pc2[1]
IP/MASK     : 10.1.1.12/24
GATEWAY     : 10.1.1.1
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 43195, 43200/21600/37800
MAC         : 00:50:79:66:68:01
LPORT       : 20011
RHOST:PORT  : 127.0.0.1:20012
MTU         : 1500
```
```
pc3> dhcp
DORA IP 10.1.1.10/24 GW 10.1.1.1

pc3> show ip

NAME        : pc3[1]
IP/MASK     : 10.1.1.10/24
GATEWAY     : 10.1.1.1
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 43176, 43180/21590/37782
MAC         : 00:50:79:66:68:02
LPORT       : 20007
RHOST:PORT  : 127.0.0.1:20008
MTU         : 1500
```

🌞 **Wireshark !**

[Wireshark échange DHCP](./ws_dhcp.pcapng)

## 2. DHCP spoofing


🌞 **Configurez dnsmasq**

```
dnf install -y dnsmasq
nano /etc/dnsmasq.conf
sudo systemctl restart dnsmasq
sudo systemctl enable dnsmasq
```
Dans le fichier : /etc/dnsmasq.conf
```
port=0
interface=enp0s3
dhcp-range=10.1.1.210,10.1.1.250,255.255.255.0,12h
dhcp-authoritative
```

🌞 **Test !**

```
PC1> dhcp
DORA IP 10.1.1.247/24 GW 10.1.1.252

PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.247/24
GATEWAY     : 10.1.1.252
DNS         :
DHCP SERVER : 10.1.1.252
DHCP LEASE  : 43197, 43200/21600/37800
MAC         : 00:50:79:66:68:02
LPORT       : 20006
RHOST:PORT  : 127.0.0.1:20008
MTU         : 1500
```

🌞 **Now race !**

```
DORA IP 10.1.1.10/24 GW 10.1.1.1


PC1> show ip

NAME        : PC1[1]
IP/MASK     : 10.1.1.10/24
GATEWAY     : 10.1.1.1
DNS         :
DHCP SERVER : 10.1.1.253
DHCP LEASE  : 43197, 43200/21600/37800
MAC         : 00:50:79:66:68:00
LPORT       : 20008
RHOST:PORT  : 127.0.0.1:20009
MTU         : 1500
```
```
DDORA IP 10.1.1.210/24 GW 10.1.1.252


PC2> show ip

NAME        : PC2[1]
IP/MASK     : 10.1.1.210/24
GATEWAY     : 10.1.1.252
DNS         :
DHCP SERVER : 10.1.1.252
DHCP LEASE  : 43131, 43200/21600/37800
MAC         : 00:50:79:66:68:06
LPORT       : 20010
RHOST:PORT  : 127.0.0.1:20011
MTU         : 1500
```
```
DDORA IP 10.1.1.211/24 GW 10.1.1.252


PC3> show ip

NAME        : PC3[1]
IP/MASK     : 10.1.1.211/24
GATEWAY     : 10.1.1.252
DNS         :
DHCP SERVER : 10.1.1.252
DHCP LEASE  : 43136, 43200/21600/37800
MAC         : 00:50:79:66:68:07
LPORT       : 20012
RHOST:PORT  : 127.0.0.1:20013
MTU         : 1500
```


🌞 **Wireshark !**

[Wireshark race DHCP](./ws_race.pcapng)


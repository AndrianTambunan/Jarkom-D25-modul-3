# Jarkom-D25-modul-3

| **No** | **Nama** | **NRP** | 
| ------------- | ------------- | --------- |
| 1 | Andrian Tambunan  | 5025211018 | 
| 2 | Sandhika Surya Ardyanto | 5025211022 |

## Topologi dan Konfigurasi Node
![image](https://github.com/AndrianTambunan/Jarkom-D25-modul-3/assets/100081922/c1be771a-534a-4e59-a14a-c6d64beea362)
### Aura (DHCP Relay)
```
# DHCP config for eth0
auto eth0
iface eth0 inet dhcp

# Static config for eth1
auto eth1
iface eth1 inet static
	address 10.34.1.100
	netmask 255.255.255.0

# Static config for eth2
auto eth2
iface eth2 inet static
	address 10.34.2.200
	netmask 255.255.255.0

# Static config for eth3
auto eth3
iface eth3 inet static
	address 10.34.3.200
	netmask 255.255.255.0

# Static config for eth4
auto eth4
iface eth4 inet static
	address 10.34.4.200
	netmask 255.255.255.0
```
### Himmel (DHCP Server)
```
auto eth0
iface eth0 inet static
	address 10.34.1.1
	netmask 255.255.255.0
	gateway 10.34.1.200
```
### Heiter (DNS Server)
```
auto eth0
iface eth0 inet static
	address 10.34.1.2
	netmask 255.255.255.0
	gateway 10.34.1.200
```
### Denken (Database Server)
```
auto eth0
iface eth0 inet static
	address 10.34.2.1
	netmask 255.255.255.0
	gateway 10.34.2.200
```
### Eisen (Load Balancer)
```
auto eth0
iface eth0 inet static
	address 10.34.2.2
	netmask 255.255.255.0
	gateway 10.34.2.200
```
## Soal 0
Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa `riegel.canyon.yyy.com` untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki `IP [prefix IP].x.1.`

### Pada Heiter di /root/.bashrc
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install bind9 -y

cp named.conf.options /etc/bind/named.conf.options

echo '
zone "granz.channel.d25.com" {
    type master;
    file "/etc/bind/worker/granz.channel.d25.com";
};' >> /etc/bind/named.conf.local

mkdir -p /etc/bind/worker

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     granz.channel.d25.com. root.granz.channel.d25.com. (
                        2023131101      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      granz.channel.d25.com.
@               IN      A       10.34.3.1       
www             IN      CNAME   granz.channel.d25.com.
' > /etc/bind/worker/granz.channel.d25.com

echo '
zone "riegel.canyon.d25.com" {
    type master;
    file "/etc/bind/worker/riegel.canyon.d25.com";
};' >> /etc/bind/named.conf.local

echo '
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     riegel.canyon.d25.com. root.riegel.canyon.d25.com. (
                        2023131101      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@               IN      NS      riegel.canyon.d25.com.
@               IN      A       10.34.4.1       
www             IN      CNAME   riegel.canyon.d25.com.
' >  /etc/bind/worker/riegel.canyon.d25.com

service bind9 restart
```

## Soal 1
Semua CLIENT harus menggunakan konfigurasi dari DHCP Server

Membuat konfigurasi pada semua node client yaitu `Stark, Sein, Revolte, dan Richter` di `/etc/network/interfaces`
```
auto eth0
iface eth0 inet dhcp
```

## Soal 2
Client yang melalui Switch3 mendapatkan range IP dari `[prefix IP].3.16 - [prefix IP].3.32` dan `[prefix IP].3.64 - [prefix IP].3.80`

```
echo 'subnet 10.34.1.0 netmask 255.255.255.0 {
}

subnet 10.34.2.0 netmask 255.255.255.0 {
}

subnet 10.34.3.0 netmask 255.255.255.0 {
    range 10.34.3.16 10.34.3.32;
    range 10.34.3.64 10.34.3.80;
    option routers 10.34.3.200;
    option broadcast-address 10.34.3.255;
    option domain-name-servers 10.34.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}' > /etc/dhcp/dhcpd.conf
```
## Soal 3
Client yang melalui Switch4 mendapatkan range IP dari `[prefix IP].4.12 - [prefix IP].4.20` dan `[prefix IP].4.160 - [prefix IP].4.168`
```
echo 'subnet 10.34.1.0 netmask 255.255.255.0 {
}

subnet 10.34.2.0 netmask 255.255.255.0 {
}

subnet 10.34.3.0 netmask 255.255.255.0 {
    range 10.34.3.16 10.34.3.32;
    range 10.34.3.64 10.34.3.80;
    option routers 10.34.3.200;
    option broadcast-address 10.34.3.255;
    option domain-name-servers 10.34.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.34.4.0 netmask 255.255.255.0 {
    range 10.34.4.12 10.34.4.20;
    range 10.34.4.160 10.34.4.168;
    option routers 10.34.4.200;
    option broadcast-address 10.34.4.255;
    option domain-name-servers 10.34.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}' > /etc/dhcp/dhcpd.conf
```

## Soal 4
Client mendapatkan DNS dari `Heiter` dan dapat terhubung dengan internet melalui DNS tersebut

### Pada Himmel (DHCP Server) di `/root/.bashrc`
```
echo 'nameserver 192.168.122.1' > /etc/resolv.conf
apt-get update
apt-get install isc-dhcp-server -y

echo 'INTERFACES="eth0" ' > /etc/default/isc-dhcp-server

echo 'default-lease-time 600;
max-lease-time 7200;

authoritative;

subnet 10.34.1.0 netmask 255.255.255.0 {
}

subnet 10.34.2.0 netmask 255.255.255.0 {
}

subnet 10.34.3.0 netmask 255.255.255.0 {
    range 10.34.3.16 10.34.3.32;
    range 10.34.3.64 10.34.3.80;
    option routers 10.34.3.200;
    option broadcast-address 10.34.3.255;
    option domain-name-servers 10.34.1.2;
    default-lease-time 180;
    max-lease-time 5760;
}

subnet 10.34.4.0 netmask 255.255.255.0 {
    range 10.34.4.12 10.34.4.20;
    range 10.34.4.160 10.34.4.168;
    option routers 10.34.4.200;
    option broadcast-address 10.34.4.255;
    option domain-name-servers 10.34.1.2;
    default-lease-time 720;
    max-lease-time 5760;
}' > /etc/dhcp/dhcpd.conf

service isc-dhcp-server restart
service isc-dhcp-server status
```

### Pada Aura (DHCP Relay) di `/root/.bashrc`
```
apt-get update
apt-get install isc-dhcp-relay -y
service isc-dhcp-relay start

echo 'SERVERS="10.34.1.1"
INTERFACES="eth3 eth4"
OPTIONS=""' > /etc/default/isc-dhcp-relay

echo 'net.ipv4.ip_forward=1' > /etc/sysctl.conf

service isc-dhcp-relay restart
```

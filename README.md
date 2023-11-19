# Jarkom-D25-modul-3

| **No** | **Nama** | **NRP** | 
| ------------- | ------------- | --------- |
| 1 | Andrian Tambunan  | 5025211018 | 
| 2 | Sandhika Surya Ardyanto | 5025211022 |

## Topologi dan Konfigurasi Node
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
Setelah mengalahkan Demon King, perjalanan berlanjut. Kali ini, kalian diminta untuk melakukan register domain berupa riegel.canyon.yyy.com untuk worker Laravel dan granz.channel.yyy.com untuk worker PHP (0) mengarah pada worker yang memiliki IP [prefix IP].x.1.

### Pada Heiter di /root/.bashrc
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
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


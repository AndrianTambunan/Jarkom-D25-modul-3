# Jarkom-D25-modul-3

| **No** | **Nama** | **NRP** | 
| ------------- | ------------- | --------- |
| 1 | Andrian Tambunan  | 5025211018 | 
| 2 | Sandhika Surya Ardyanto | 5025211022 |

## Topologi dan Konfigurasi Node
### Aura (DHCP Relay)

### Himmel (DHCP Server)
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
### Heiter (DNS Server)
### Denken (Database Server)
### Eisen (Load Balancer)
### 

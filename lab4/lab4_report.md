University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)

Year: 2023/2024

Group: K33212

Author: Shagvalieva Ekaterina Albertovna

Lab: Lab4

Date of create: 16.12.2023

Date of finished: XX.12.2023

# Лабораторная работ №4 "Эмуляция распределенной корпоративной сети связи, настройка iBGP, организация L3VPN, VPLS"

**Цель работы:** 
Изучить протоколы BGP, MPLS и правила организации L3VPN и VPLS.

Текст файла конфигурации lab4.yaml:

```
name: lab4

mgmt:
  network: statics
  ipv4_subnet: 192.168.40.0/24

topology:
  nodes:
    R01.NY:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.168.40.2

    R01.LND:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.168.40.3

    R01.HKI:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.168.40.4

    R01.SPB:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.168.40.5

    R01.LBN:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.168.40.6

    R01.SVL:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.168.40.7

    PC1:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.168.40.8

    PC2:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.168.40.9

    PC3:
      kind: vr-mikrotik_ros
      image: vrnetlab/vr-routeros:6.47.9
      mgmt_ipv4: 192.168.40.10

  links: 
    - endpoints: ["R01.NY:eth4","PC2:eth4"]
    - endpoints: ["R01.NY:eth3","R01.LND:eth3"]
    - endpoints: ["R01.LND:eth1","R01.HKI:eth2"]
    - endpoints: ["R01.LND:eth2","R01.LBN:eth1"]
    - endpoints: ["R01.LBN:eth2","R01.HKI:eth1"]
    - endpoints: ["R01.LBN:eth3","R01.SVL:eth3"]
    - endpoints: ["R01.SVL:eth4","PC3:eth4"]
    - endpoints: ["R01.HKI:eth3","R01.SPB:eth3"]
    - endpoints: ["R01.SPB:eth4","PC1:eth4"]
```

С помощью файла конфигурации была развернута сеть:


![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/274be5b3-ea32-48b3-82e0-ae888eef65a7)


Схема получившейся сети:


![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/34032878-ba8a-46e6-9ebf-a01928dc8d24)


Тексты конфигураций устройств:


R01.NY:

```
/interface bridge
add name=Lo
add name=VPLS

/interface vpls
add disabled=no l2mtu=1500 mac-address=02:3B:11:73:24:F9 name=VPLS3 remote-peer=10.10.10.6 vpls-id=10:0
add disabled=no l2mtu=1500 mac-address=02:CB:23:D2:D0:7C name=vpls1 remote-peer=10.10.10.4 vpls-id=10:0

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/routing bgp instance
set default router-id=10.10.10.1

/routing ospf instance
set [ find default=yes ] router-id=10.10.10.1

/interface bridge port
add bridge=VPLS interface=ether2
add bridge=VPLS interface=vpls1
add bridge=VPLS interface=VPLS3

/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.40.19/24 interface=ether2 network=192.168.40.0
add address=10.10.10.1 interface=Lo network=10.10.10.1
add address=10.10.1.1/30 interface=ether4 network=10.10.1.0

/ip dhcp-client
add disabled=no interface=ether1

/mpls ldp
set enabled=yes

/mpls ldp interface
add interface=ether4

/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=10.10.10.2 remote-as=65530 update-source=Lo

/routing ospf network
add area=backbone

/system identity
set name=R01.NY
```

R01.LND:

```
/interface bridge
add name=Lo0

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/routing bgp instance
set default router-id=10.10.10.2

/routing ospf instance
set [ find default=yes ] name=ospf0 router-id=10.10.10.2

/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.40.11/24 interface=ether2 network=192.168.40.0
add address=10.10.1.2/30 interface=ether4 network=10.10.1.0
add address=10.10.2.1/30 interface=ether2 network=10.10.2.0
add address=10.10.4.1/30 interface=ether3 network=10.10.4.0
add address=10.10.10.2 interface=Lo0 network=10.10.10.2

/ip dhcp-client
add disabled=no interface=ether1

/mpls ldp
set enabled=yes transport-address=10.10.10.2

/mpls ldp interface
add interface=ether2
add interface=ether3
add interface=ether4

/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer3 remote-address=10.10.10.1 remote-as=65530 update-source=Lo0
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=10.10.10.3 remote-as=65530 route-reflect=yes \
    update-source=Lo0
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer2 remote-address=10.10.10.5 remote-as=65530 route-reflect=yes \
    update-source=Lo0

/routing ospf network
add area=backbone

/system identity
set name=R01.LND
```


R01.HKI:

```
/interface bridge
add name=Lo0

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/routing bgp instance
set default router-id=10.10.10.3

/routing ospf instance
set [ find default=yes ] name=ospf0 router-id=10.10.10.3

/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.40.12/24 interface=ether2 network=192.168.40.0
add address=10.10.10.3 interface=Lo0 network=10.10.10.3
add address=10.10.2.2/30 interface=ether3 network=10.10.2.0
add address=10.10.5.1/30 interface=ether2 network=10.10.5.0
add address=10.10.3.1/30 interface=ether4 network=10.10.3.0

/ip dhcp-client
add disabled=no interface=ether1

/mpls ldp interface
add interface=ether2
add interface=ether3
add interface=ether4

/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=10.10.10.2 remote-as=65530 route-reflect=yes \
    update-source=Lo0
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer2 remote-address=10.10.10.5 remote-as=65530 route-reflect=yes \
    update-source=Lo0
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer3 remote-address=10.10.10.4 remote-as=65530 update-source=Lo0

/routing ospf network
add area=backbone

/system identity
set name=R01.HKI
```


R01.LBN:

```
/interface bridge
add name=Lo0

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/routing bgp instance
set default router-id=10.10.10.5

/routing ospf instance
set [ find default=yes ] name=ospf0 router-id=10.10.10.5

/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.40.13/24 interface=ether2 network=192.168.40.0
add address=10.10.10.5 interface=Lo0 network=10.10.10.5
add address=10.10.4.2/30 interface=ether2 network=10.10.4.0
add address=10.10.5.2/30 interface=ether3 network=10.10.5.0
add address=10.10.6.1/30 interface=ether4 network=10.10.6.0

/ip dhcp-client
add disabled=no interface=ether1

/mpls ldp
set enabled=yes transport-address=10.10.10.5

/mpls ldp interface
add interface=ether2
add interface=ether3
add interface=ether4

/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=10.10.10.2 remote-as=65530 route-reflect=yes update-source=Lo0
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer2 remote-address=10.10.10.3 remote-as=65530 route-reflect=yes update-source=Lo0
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer3 remote-address=10.10.10.6 remote-as=65530 update-source=Lo0

/routing ospf network
add area=backbone

/system identity
set name=R01.LBN
```


R01.SPB:

```
/interface bridge
add name=Lo
add name=VPLS

/interface vpls
add disabled=no l2mtu=1500 mac-address=02:5E:29:75:91:55 name=vpls1 remote-peer=10.10.10.1 vpls-id=10:0
add disabled=no l2mtu=1500 mac-address=02:B5:27:55:42:F5 name=vpls2 remote-peer=10.10.10.6 vpls-id=10:0

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/routing bgp instance
set default router-id=10.10.10.4

/routing ospf instance
set [ find default=yes ] router-id=10.10.10.4

/interface bridge port
add bridge=VPLS interface=ether2
add bridge=VPLS interface=vpls1
add bridge=VPLS interface=vpls2

/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.40.20/24 interface=ether2 network=192.168.40.0
add address=10.10.3.2/30 interface=ether4 network=10.10.3.0
add address=10.10.10.4 interface=Lo network=10.10.10.4

/ip dhcp-client
add disabled=no interface=ether1

/mpls ldp
set enabled=yes

/mpls ldp interface
add interface=ether4

/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=10.10.10.3 remote-as=65530 update-source=Lo

/routing ospf network
add area=backbone

/system identity
set name=R01.SPB
```


R01.SVL:

```
/interface bridge
add name=Lo
add name=VPLS

/interface vpls
add disabled=no l2mtu=1500 mac-address=02:3F:4F:5F:C7:41 name=VPLS3 remote-peer=10.10.10.1 vpls-id=10:0
add disabled=no l2mtu=1500 mac-address=02:C0:D7:89:7A:9C name=vpls2 remote-peer=10.10.10.4 vpls-id=10:0

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/routing bgp instance
set default router-id=10.10.10.6

/routing ospf instance
set [ find default=yes ] router-id=10.10.10.6

/interface bridge port
add bridge=VPLS interface=ether2
add bridge=VPLS interface=VPLS3
add bridge=VPLS interface=vpls2

/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.40.21/24 interface=ether2 network=192.168.40.0
add address=10.10.6.2/30 interface=ether4 network=10.10.6.0
add address=10.10.10.6 interface=Lo network=10.10.10.6

/ip dhcp-client
add disabled=no interface=ether1

/mpls ldp
set enabled=yes

/mpls ldp interface
add interface=ether4

/routing bgp peer
add address-families=ip,l2vpn,l2vpn-cisco,vpnv4 name=peer1 remote-address=10.10.10.5 remote-as=65530 update-source=Lo

/routing ospf network
add area=backbone

/system identity
set name=R01.SVL
```


PC1:

```
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.40.16/24 interface=ether2 network=192.168.40.0  # Обновленный IP-адрес

/ip dhcp-client
add disabled=no interface=ether1

/system identity
set name=PC1
```


PC2:

```
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.40.17/24 interface=ether2 network=192.168.40.0  # Обновленный IP-адрес

/ip dhcp-client
add disabled=no interface=ether1

/system identity
set name=PC2
```


PC3:

```
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.40.18/24 interface=ether2 network=192.168.40.0  # Обновленный IP-адрес

/ip dhcp-client
add disabled=no interface=ether1

/system identity
set name=PC3
```

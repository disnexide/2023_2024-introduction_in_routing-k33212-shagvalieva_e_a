University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)

Year: 2023/2024

Group: K33212

Author: Shagvalieva Ekaterina Albertovna

Lab: Lab2

Date of create: 27.11.2023

Date of finished: 10.12.2023

# Лабораторная работ №2 "Эмуляция распределенной корпоративной сети связи, настройка статической маршрутизации между филиалами"

**Цель работы:** 
Ознакомиться с принципами планирования IP адресов, настройке статической маршрутизации и сетевыми функциями устройств.

Текст файла конфигурации rogaikopyta.yaml:

```
   name: network2

   mgmt:
     network: statics2
     ipv4_subnet: 190.168.10.0/24

   topology:
     nodes:
       R01.BRL:
         kind: vr-ros
         image: vrnetlab/vr-routeros:6.47.9
         mgmt_ipv4: 190.168.10.2

       R01.FRT:
         kind: vr-ros
         image: vrnetlab/vr-routeros:6.47.9
         mgmt_ipv4: 190.168.10.3

       R01.MSK:
         kind: vr-ros
         image: vrnetlab/vr-routeros:6.47.9
         mgmt_ipv4: 190.168.10.4

       PC1:
         kind: vr-ros
         image: vrnetlab/vr-routeros:6.47.9
         mgmt_ipv4: 190.168.10.5

       PC2:
         kind: vr-ros
         image: vrnetlab/vr-routeros:6.47.9
         mgmt_ipv4: 190.168.10.6

       PC3:
         kind: vr-ros
         image: vrnetlab/vr-routeros:6.47.9
         mgmt_ipv4: 190.168.10.7

     links:
       - endpoints: ["R01.BRL:eth1" , "R01.MSK:eth1"]
       - endpoints: ["R01.BRL:eth2" , "R01.FRT:eth1"]
       - endpoints: ["R01.MSK:eth2" , "R01.FRT:eth2"]
       - endpoints: ["R01.BRL:eth3" , "PC3:eth1"]
       - endpoints: ["R01.FRT:eth3" , "PC2:eth1"]
       - endpoints: ["R01.MSK:eth3" , "PC1:eth1"]
```

С помощью файла была развернута сеть:

![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/b4b55471-01e6-4b5b-8e89-1ad92e81b83e)

Схема получившейся сети:

![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/289842b5-e8f8-4e32-8b8d-a85e050a76fc)


Тексты конфигураций для устройств:

**R01.BRL:**
```
/interface vlan
add interface=ether2 name=vlan10 vlan-id=10
add interface=ether2 name=vlan20 vlan-id=20

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/ip pool
add name=pool10 ranges=192.168.10.10-192.168.10.254
add name=pool20 ranges=192.168.20.10-192.168.20.254

/ip dhcp-server
add address-pool=pool10 disabled=no interface=vlan10 name=dhcp10
add address-pool=pool20 disabled=no interface=vlan20 name=dhcp20

/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.10.1/24 interface=vlan10 network=192.168.10.0
add address=192.168.20.1/24 interface=vlan20 network=192.168.20.0

/ip dhcp-client
add disabled=no interface=ether1

/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1
add address=192.168.20.0/24 gateway=192.168.20.1

/system identity
set name=R01.BRL
```

**R01.MSK:**
```
/interface vlan
add interface=ether2 name=vlan10 vlan-id=10
add interface=ether2 name=vlan20 vlan-id=20

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/ip pool
add name=pool10 ranges=192.168.10.10-192.168.10.254
add name=pool20 ranges=192.168.20.10-192.168.20.254

/ip dhcp-server
add address-pool=pool10 disabled=no interface=vlan10 name=dhcp10
add address-pool=pool20 disabled=no interface=vlan20 name=dhcp20

/ip address
add address=172.31.255.30/30 interface=ether1 network=172.31.255.28
add address=192.168.10.1/24 interface=vlan10 network=192.168.10.0
add address=192.168.20.1/24 interface=vlan20 network=192.168.20.0

/ip dhcp-client
add disabled=no interface=ether1

/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1
add address=192.168.20.0/24 gateway=192.168.20.1

/system identity
set name=R01.MSK
```

**R01.FRT:**
```
/interface vlan
add interface=ether1 name=vlan10 vlan-id=10
add interface=ether2 name=vlan20 vlan-id=20
add interface=ether3 name=vlan30 vlan-id=30

/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik

/ip pool
add name=pool10 ranges=192.168.10.10-192.168.10.254
add name=pool20 ranges=192.168.20.10-192.168.20.254
add name=pool30 ranges=192.168.30.10-192.168.30.254

/ip dhcp-server
add address-pool=pool10 disabled=no interface=vlan10 name=dhcp10
add address-pool=pool20 disabled=no interface=vlan20 name=dhcp20
add address-pool=pool30 disabled=no interface=vlan30 name=dhcp30

/ip address
add address=172.31.255.30/30 interface=ether4 network=172.31.255.28
add address=192.168.10.1/24 interface=vlan10 network=192.168.10.0
add address=192.168.20.1/24 interface=vlan20 network=192.168.20.0
add address=192.168.30.1/24 interface=vlan30 network=192.168.30.0

/ip dhcp-client
add disabled=no interface=ether4

/ip dhcp-server network
add address=192.168.10.0/24 gateway=192.168.10.1
add address=192.168.20.0/24 gateway=192.168.20.1
add address=192.168.30.0/24 gateway=192.168.30.1

/system identity
set name=R01.FRT
```

**PC1:**
```
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip address
add address=192.168.10.2/24 interface=eth1 network=192.168.10.0
/ip dhcp-client
add disabled=no interface=eth1
/system identity
set name=PC1
```

**PC2:**
```
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip address
add address=192.168.20.2/24 interface=eth1 network=192.168.20.0
/ip dhcp-client
add disabled=no interface=eth1
/system identity
set name=PC2
```

**PC3:**
```
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip address
add address=192.168.30.2/24 interface=eth1 network=192.168.30.0
/ip dhcp-client
add disabled=no interface=eth1
/system identity
set name=PC3
```

Результаты проверки связности:

![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/2fa84bc8-6a75-42d2-a799-7214f0e8acbd)
![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/8ab17060-5b9b-41cb-9943-6e07cd37ccfb)


**Вывод:**
В ходе выполнения лабораторной работы я ознакомилась с принципами планирования IP адресов, настройкой статической маршрутизации и сетевыми функциями устройств.


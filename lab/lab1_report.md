University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)

Year: 2023/2024

Group: K33212

Author: Shagvalieva Ekaterina Albertovna

Lab: Lab1

Date of create: 02.10.2023

Date of finished: 27.11.2023

# Лабораторная работ №1 "Установка ContainerLab и развертывание тестовой сети связи"

## Цель работы

Ознакомиться с инструментом ContainerLab и методами работы с ним, изучить работу VLAN, IP адресации и т.д.

## Ход работы

С помощью текста файла network.yml была развернута сеть: 

![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/01c4e7da-0049-4d8b-a434-41d87da3ce8e)

После этого с помощью команд ssh admin@192.168.32.3 (с изменением адреса конкретного устройства) были изменены конфигурации устройств.

R01.TEST:
/interface vlan add interface=ether1 name=vlan10 vlan-id=10
/interface vlan add interface=ether1 name=vlan20 vlan-id=20
/ip pool add name=pool10 ranges=192.168.32.10-192.168.32.20
/ip pool add name=pool20 ranges=192.168.32.30-192.168.32.40
/ip dhcp-server add name=dhcp10 interface=vlan10 address-pool=pool10 disabled=no
/ip dhcp-server add name=dhcp20 interface=vlan20 address-pool=pool20 disabled=no
/ip address add address=192.168.32.3/24 interface=vlan10 network=192.168.32.0
/ip address add address=192.168.32.4/24 interface=vlan20 network=192.168.32.0
/system identity set name=R01.TEST

SW02.L3.01.TEST:
/interface vlan
add interface=ether2 name=vlan10 vlan-id=10
add interface=ether2 name=vlan20 vlan-id=20
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=pool10 ranges=192.168.32.10-192.168.32.254
add name=pool20 ranges=192.168.42.10-192.168.42.254
/ip dhcp-server
add address-pool=pool10 disabled=no interface=vlan10 name=dhcp10
add address-pool=pool20 disabled=no interface=vlan20 name=dhcp20
/ip address
add address=172.31.255.34/30 interface=ether1 network=172.31.255.32
add address=192.168.32.5/24 interface=vlan10 network=192.168.32.0
add address=192.168.42.1/24 interface=vlan20 network=192.168.42.0
/ip dhcp-client
add disabled=no interface=ether1
/ip dhcp-server network
add address=192.168.32.0/24 gateway=192.168.32.5
add address=192.168.42.0/24 gateway=192.168.42.1
/system identity
set name=SW02.L3.01.TEST

SW02.L3.01.TEST:
/interface vlan
add interface=ether2 name=vlan10 vlan-id=10
add interface=ether2 name=vlan20 vlan-id=20
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=pool10 ranges=192.168.32.10-192.168.32.254
add name=pool20 ranges=192.168.42.10-192.168.42.254
/ip dhcp-server
add address-pool=pool10 disabled=no interface=vlan10 name=dhcp10
add address-pool=pool20 disabled=no interface=vlan20 name=dhcp20
/ip address
add address=172.31.255.36/30 interface=ether1 network=172.31.255.32
add address=192.168.32.6/24 interface=vlan10 network=192.168.32.0
add address=192.168.42.2/24 interface=vlan20 network=192.168.42.0
/ip dhcp-client
add disabled=no interface=ether1
/ip dhcp-server network
add address=192.168.32.0/24 gateway=192.168.32.6
add address=192.168.42.0/24 gateway=192.168.42.2
/system identity
set name=SW02.L3.01.TEST

SW02.L3.02.TEST:
/interface vlan
add interface=ether2 name=vlan20 vlan-id=20
add interface=ether2 name=vlan30 vlan-id=30
/interface wireless security-profiles
set [ find default=yes ] supplicant-identity=MikroTik
/ip pool
add name=pool20 ranges=192.168.42.10-192.168.42.254
add name=pool30 ranges=192.168.52.10-192.168.52.254
/ip dhcp-server
add address-pool=pool20 disabled=no interface=vlan20 name=dhcp20
add address-pool=pool30 disabled=no interface=vlan30 name=dhcp30
/ip address
add address=172.31.255.40/30 interface=ether1 network=172.31.255.36
add address=192.168.42.6/24 interface=vlan20 network=192.168.42.0
add address=192.168.52.2/24 interface=vlan30 network=192.168.52.0
/ip dhcp-client
add disabled=no interface=ether1
/ip dhcp-server network
add address=192.168.42.0/24 gateway=192.168.42.6
add address=192.168.52.0/24 gateway=192.168.52.2
/system identity
set name=SW02.L3.02.TEST


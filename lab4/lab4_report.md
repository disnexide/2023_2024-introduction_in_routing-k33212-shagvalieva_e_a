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


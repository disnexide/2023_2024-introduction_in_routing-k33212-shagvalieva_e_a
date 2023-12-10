University: [ITMO University](https://itmo.ru/ru/)

Faculty: [FICT](https://fict.itmo.ru)

Course: [Introduction in routing](https://github.com/itmo-ict-faculty/introduction-in-routing)

Year: 2023/2024

Group: K33212

Author: Shagvalieva Ekaterina Albertovna

Lab: Lab2

Date of create: 27.11.2023

Date of finished: XX.XX.2023

# Лабораторная работ №2 "Эмуляция распределенной корпоративной сети связи, настройка статической маршрутизации между филиалами"

Текст файла конфигурации rogaikopyta.yaml:


`name: lab2

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
    - endpoints: ["R01.MSK:eth3" , "PC1:eth1"]`

    

![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/b4b55471-01e6-4b5b-8e89-1ad92e81b83e)




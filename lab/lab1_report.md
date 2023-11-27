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
![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/56d17be7-58fa-4e0f-98ae-cd01409f5d04)




SW02.L3.01.TEST:
![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/dfdbdbce-43d4-41ae-afb9-183eb9a9db6d)




SW02.L3.01.TEST:
![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/043b982b-2bae-4934-a62e-cdea255df13a)




SW02.L3.02.TEST:
![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/653494e2-971b-402c-aeca-b8f927fc0ff3)



### Результаты пингов:

![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/6b14f14a-3983-457c-8812-8862ff4672da)

![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/edfcb357-396d-44eb-a857-c2ddfd7e947e)

![image](https://github.com/disnexide/2023_2024-introduction_in_routing-k33212-shagvalieva_e_a/assets/90693992/8215a960-ce29-43e2-a7f5-4e9cb954099a)


# Вывод:
В ходе лабораторной работы были выполнены все поставленные задачи: был на практике изучен процесс развертывания трехуровневой сети, в результате которого было осуществлено соединение между устройствами в разных локальных сетях, а также настроена автоматическая раздача IP-адресов через DHCP-сервер.



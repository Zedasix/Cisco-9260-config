# Cisco-9260-config
Config d'un switch Cisco 9260 avec un routeur Cisco en utilsant deux VLAN.
Un PC et un serveur Proxmox sur le Vlan 10
Un second PC sur le Vlan 20
La connexion entre le switch (1/0/1) et le routeur G0/0
Le NAT sort vers internet sur le port G0/1

Cette page est en public, c'est plus simple qu'un pastebin (et ça fait de l'activité sur mon compte :D)
![Alt Text](https://media.tenor.com/tQVHeKhEgF8AAAAC/jdg-computer.gif)
# Partie switch
```
vlan 10
name VLAN10
exit
interface gigabitEthernet 1/0/10
switchport mode access
switchport access vlan 10

vlan 20
name VLAN20
exit
interface gigabitEthernet 1/0/20
switchport mode access
switchport access vlan 20

interface gigabitEthernet 1/0/1
switchport mode trunk
```
# Partie routeur
```
interface gigabitEthernet0/0
no shutdown
exit

interface gigabitEthernet0/1
ip nat outside
no shutdown
exit

access-list 1 permit 192.168.10.0 0.0.0.255
access-list 2 permit 192.168.20.0 0.0.0.255

ip nat inside source list 1 interface gigabitEthernet0/1 overload
ip nat inside source list 2 interface gigabitEthernet0/1 overload

interface gigabitEthernet 0/0.10
description VLAN10
ip nat inside
encapsulation dot1q 10
ip address 192.168.10.254 255.255.255.0
exit

interface gigabitEthernet 0/0.20
description VLAN20
ip nat inside
encapsulation dot1q 20
ip address 192.168.20.254 255.255.255.0
exit
```

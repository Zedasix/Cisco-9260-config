# Cisco-9260-config
Config d'un switch Cisco 9260 avec un routeur Cisco en utilsant deux VLAN

Cette page est en public, c'est plus simple qu'un pastebin (et ça fait de l'activité sur mon compte :D)
Partie switch
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

access-list 1 permit 192.168.1.0 0.0.0.255
access-list 2 permit 192.168.2.0 0.0.0.255

ip nat inside source list 1 interface gigabitEthernet0/1 overload
ip nat inside source list 2 interface gigabitEthernet0/1 overload

interface gigabitEthernet 0/0.10
description VLAN10
ip nat inside
encapsulation dot1q 10
ip address 192.168.1.1 255.255.255.0
exit

interface gigabitEthernet 0/0.20
description VLAN20
ip nat inside
encapsulation dot1q 20
ip address 192.168.2.1 255.255.255.0
exit
```

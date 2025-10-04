# Contenido Semana 9

- [Enrutamiento Dinámico](#enrutamiento-dinámico)
  - [RIP](#rip)
  - [EIGRP](#eigrp)
  - [BGP](#bgp)

# Enrutamiento Dinámico

## RIP

```bash
!RIP1
enable
configure terminal
hostname RIP1
interface GigabitEthernet 0/0/1
ip address 192.168.1.1 255.255.255.0
no shutdown
exit
interface serial 0/1/0
ip address 10.0.1.1 255.255.255.252
no shutdown
exit
! rutas dinamicas con rip
router rip
version 2
network 192.168.1.0
network 10.0.1.0
no auto-summary
do write
exit
exit


!RIP2
enable
configure terminal
hostname RIP2
interface GigabitEthernet 0/0/1
ip address 192.168.2.1 255.255.255.0
no shutdown
exit
interface serial 0/1/0
ip address 10.0.2.1 255.255.255.252
no shutdown
exit
! rutas dinamicas con rip
router rip
version 2
network 192.168.2.0
network 10.0.2.0
no auto-summary
do write
exit
exit


!RIP3
enable
configure terminal
hostname RIP3
interface serial 0/1/0
ip address 10.0.1.2 255.255.255.252
no shutdown
exit
interface serial 0/1/1
ip address 10.0.2.2 255.255.255.252
no shutdown
exit
! rutas dinamicas con rip
router rip
version 2
network 10.0.1.0
network 10.0.2.0
no auto-summary
do write
exit
exit
```

## OSPF

```bash
!OSPF1
enable
configure terminal
hostname OSPF1
interface GigabitEthernet 0/0/1
ip address 192.168.3.1 255.255.255.0
no shutdown
exit
interface serial 0/1/0
ip address 10.0.3.1 255.255.255.252
no shutdown
exit
! rutas dinamicas con ospf
router ospf 1
network 192.168.3.0 0.0.0.255 area 0
network 10.0.3.0 0.0.0.3 area 0
do write
exit
exit


!OSPF2
enable
configure terminal
hostname OSPF2
interface GigabitEthernet 0/0/1
ip address 192.168.4.1 255.255.255.0
no shutdown
exit
interface serial 0/1/0
ip address 10.0.4.1 255.255.255.252
no shutdown
exit
! rutas dinamicas con ospf
router ospf 1
network 192.168.4.0 0.0.0.255 area 0
network 10.0.4.0 0.0.0.3 area 0
do write
exit
exit


!OSPF3
enable
configure terminal
hostname OSPF3
interface serial 0/1/0
ip address 10.0.3.2 255.255.255.252
no shutdown
exit
interface serial 0/1/1
ip address 10.0.4.2 255.255.255.252
no shutdown
exit
! rutas dinamicas con ospf
router ospf 1
network 10.0.3.0 0.0.0.3 area 0
network 10.0.4.0 0.0.0.3 area 0
do write
exit
exit
```

## EIGRP

```bash
!EIGRP1
enable
configure terminal
hostname EIGRP1
interface GigabitEthernet 0/0/1
ip address 192.168.5.1 255.255.255.0
no shutdown
exit
interface serial 0/1/0
ip address 10.0.5.1 255.255.255.252
no shutdown
exit
! rutas dinamicas con eigrp
router eigrp 10
network 192.168.5.0 0.0.0.255
network 10.0.5.0 0.0.0.3
no auto-summary
do write
exit
exit


!EIGRP2
enable
configure terminal
hostname EIGRP2
interface GigabitEthernet 0/0/1
ip address 192.168.6.1 255.255.255.0
no shutdown
exit
interface serial 0/1/0
ip address 10.0.6.1 255.255.255.252
no shutdown
exit
! rutas dinamicas con eigrp
router eigrp 10
network 192.168.6.0 0.0.0.255
network 10.0.6.0 0.0.0.3
no auto-summary
do write
exit
exit


!EIGRP3
enable
configure terminal
hostname EIGRP3
interface serial 0/1/0
ip address 10.0.5.2 255.255.255.252
no shutdown
exit
interface serial 0/1/1
ip address 10.0.6.2 255.255.255.252
no shutdown
exit
! rutas dinamicas con eigrp
router eigrp 10
network 10.0.5.0 0.0.0.3
network 10.0.6.0 0.0.0.3
no auto-summary
do write
exit
exit
```

## BGP

```bash
!BGP1
enable
configure terminal
hostname BGP1
interface GigabitEthernet 0/0/1
ip address 192.168.100.1 255.255.255.0
no shutdown
exit
interface serial 0/1/0
ip address 11.0.0.1 255.255.255.252
no shutdown
exit
interface serial 0/1/1
ip address 13.0.0.1 255.255.255.252
no shutdown
exit
! rutas dinamicas con bgp
router bgp 64501
network 192.168.100.0 mask 255.255.255.0
network 11.0.0.0 mask 255.255.255.252
network 13.0.0.0 mask 255.255.255.252
neighbor 11.0.0.2 remote-as 64500
neighbor 13.0.0.2 remote-as 64502
do write
exit
exit


!BGP2
enable
configure terminal
hostname BGP2
interface GigabitEthernet 0/0/1
ip address 192.168.200.1 255.255.255.0
no shutdown
exit
interface serial 0/1/0
ip address 12.0.0.1 255.255.255.252
no shutdown
exit
interface serial 0/1/1
ip address 13.0.0.2 255.255.255.252
no shutdown
exit
! rutas dinamicas con bgp
router bgp 64502
network 192.168.200.0 mask 255.255.255.0
network 12.0.0.0 mask 255.255.255.252
network 13.0.0.0 mask 255.255.255.252
neighbor 12.0.0.2 remote-as 64500
neighbor 13.0.0.1 remote-as 64501
do write
exit
exit


!BGP3
enable
configure terminal
hostname BGP3
interface serial 0/1/0
ip address 11.0.0.2 255.255.255.252
no shutdown
exit
interface serial 0/1/1
ip address 12.0.0.2 255.255.255.252
no shutdown
exit
! rutas dinamicas con bgp
router bgp 64500
network 11.0.0.0 mask 255.255.255.252
network 12.0.0.0 mask 255.255.255.252
neighbor 12.0.0.1 remote-as 64501
neighbor 13.0.0.1 remote-as 64502
do write
exit
exit
```

Video de referencia

[![Video Enrutamiento Dinamico](https://img.youtube.com/vi/dtvfvQMstXQ/0.jpg)](https://www.youtube.com/watch?v=dtvfvQMstXQ)


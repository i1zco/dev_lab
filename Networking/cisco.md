# Cisco Network Configuration

Repository for documenting essential Cisco IOS network configurations.

## Table Content

1. [Routing (Static & Dynamic)](#routing-static--dynamic)
2. [VLANs & Inter-VLAN Routing](#vlans--inter-vlan-routing)
3. [Access Control Lists (ACLs)](#access-control-lists-acls)
4. [Cisco Command Cheat Sheet](#cisco-command-cheat-sheet)


# Routing (Static & Dynamic)

## Static Routing

Commands to configure routers to reach remote networks using a next-hop IP address.

### Network Topology Example

| Router A | Router B | Router C |
| :--- | :--- | :--- |
| 192.168.1.1 - 10.0.1.1 | 10.0.1.2 - 192.168.2.1 - 10.0.2.1 | 10.0.2.2 - 192.168.3.1 |


## Router A

```cisco
RouterA> enable
RouterA# configure terminal

RouterA(config)# ip route 192.168.2.0 255.255.255.0 10.0.1.2
RouterA(config)# ip route 192.168.3.0 255.255.255.0 10.0.1.2
```

## Router B

```cisco
RouterB> enable
RouterB# configure terminal

RouterB(config)# ip route 192.168.1.0 255.255.255.0 10.0.1.1
RouterB(config)# ip route 192.168.3.0 255.255.255.0 10.0.2.2
```

## Router C

```cisco
RouterC> enable
RouterC# configure terminal

RouterC(config)# ip route 192.168.1.0 255.255.255.0 10.0.2.1
RouterC(config)# ip route 192.168.2.0 255.255.255.0 10.0.2.1
```


---

## Dynamic Routing (OSPF)

OSPF allows routers to automatically exchange routing information and learn available networks.

## Router A

```cisco
RouterA> enable
RouterA# configure terminal

RouterA(config)# router ospf 1
RouterA(config-router)# network 192.168.1.0 0.0.0.255 area 0
RouterA(config-router)# network 10.0.1.0 0.0.0.3 area 0
```

## Router B

```cisco
RouterB> enable
RouterB# configure terminal

RouterB(config)# router ospf 1
RouterB(config-router)# network 192.168.2.0 0.0.0.255 area 0
RouterB(config-router)# network 10.0.1.0 0.0.0.3 area 0
RouterB(config-router)# network 10.0.2.0 0.0.0.3 area 0
```

## Router C

```cisco
RouterC> enable
RouterC# configure terminal

RouterC(config)# router ospf 1
RouterC(config-router)# network 192.168.3.0 0.0.0.255 area 0
RouterC(config-router)# network 10.0.2.0 0.0.0.3 area 0
```


---

# VLANs & Inter-VLAN Routing

## Creating VLANs

Example:

| VLAN ID | Name |
| :--- | :--- |
| 10 | SALES |
| 20 | IT |


## Switch Configuration

```cisco
Switch> enable
Switch# configure terminal

Switch(config)# vlan 10
Switch(config-vlan)# name SALES

Switch(config)# vlan 20
Switch(config-vlan)# name IT
```


## Assign Access Ports

```cisco
Switch(config)# interface fastEthernet0/1
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 10


Switch(config)# interface fastEthernet0/2
Switch(config-if)# switchport mode access
Switch(config-if)# switchport access vlan 20
```


## Configure Trunk Port Between Two Switches

```cisco
Switch(config)# interface gigabitEthernet0/1
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20
```


## Inter-VLAN Routing (Router-on-a-Stick)

### Router Configuration

```cisco
Router> enable
Router# configure terminal

Router(config)# interface gigabitEthernet0/0
Router(config-if)# no shutdown


Router(config)# interface gigabitEthernet0/0.10
Router(config-subif)# encapsulation dot1Q 10
Router(config-subif)# ip address 192.168.10.1 255.255.255.0


Router(config)# interface gigabitEthernet0/0.20
Router(config-subif)# encapsulation dot1Q 20
Router(config-subif)# ip address 192.168.20.1 255.255.255.0
```


---

# Access Control Lists (ACLs)

ACLs are used to control network traffic by allowing or denying packets.

## Standard ACL

Filters traffic based on source IP address.

Example: Block network `192.168.10.0/24`

```cisco
Router> enable
Router# configure terminal

Router(config)# access-list 10 deny 192.168.10.0 0.0.0.255
Router(config)# access-list 10 permit any


Router(config)# interface gigabitEthernet0/0
Router(config-if)# ip access-group 10 in
```


## Extended ACL

Filters traffic based on:

- Source IP
- Destination IP
- Protocol
- Port Number


Example: Block HTTP traffic from VLAN 10 to VLAN 20.

```cisco
Router> enable
Router# configure terminal

Router(config)# access-list 100 deny tcp 192.168.10.0 0.0.0.255 192.168.20.0 0.0.0.255 eq 80

Router(config)# access-list 100 permit ip any any


Router(config)# interface gigabitEthernet0/0
Router(config-if)# ip access-group 100 in
```


## Verify ACLs

```cisco
show access-lists

show ip interface

show running-config
```


---

# Cisco Command Cheat Sheet


## Basic Configuration

```cisco
enable

configure terminal

hostname Router1

exit

copy running-config startup-config

write memory

reload
```


## Interface Configuration

```cisco
interface gigabitEthernet0/0

description LAN Interface

ip address 192.168.1.1 255.255.255.0

no shutdown

shutdown
```


## Routing Commands

```cisco
show ip route

show ip protocols

show ip ospf neighbor

show ip ospf interface
```


## VLAN Commands

```cisco
show vlan brief

show interfaces trunk

show interfaces switchport
```


## Interface Verification

```cisco
show ip interface brief

show interfaces

show running-config interface gigabitEthernet0/0
```


## ACL Commands

```cisco
show access-lists

show ip interface
```


## Network Testing

```cisco
ping 192.168.1.1

traceroute 192.168.3.1
```


## Configuration Files

```cisco
show running-config

show startup-config

copy running-config startup-config

copy startup-config running-config

erase startup-config
```


## Device Information

```cisco
show version

show inventory

show clock

show hosts
```


## Troubleshooting Commands

```cisco
show cdp neighbors

show arp

show mac address-table

show logging

debug ip ospf events

undebug all
```

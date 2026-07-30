# Cisco Network Configuration

Repo To documenting essential Cisco IOS network configurations

## Table Content

1. [Routing (Static & Dynamic OSPF)](#1-routing)
2. [VLANs & Inter-VLAN Routing](#2-vlans)
3. [Access Control Lists (ACLs)](#3-acls)
4. [Cisco Command Cheat Sheet](#4-cheat-sheet)




## Routing (Static & Dynamic)

### Static Routing
Commands to configure the first router to reach a remote network via a next-hop IP:
Example:

| Router A | Router B | Router C |
| :--- | :--- | :--- |
| 192.168.1.1 - 10.0.1.1 | 10.0.1.2 - 192.168.2.1 - 10.0.2.1 | 10.0.2.2 - 192.168.3.1 |


#### Router A

```
RouterA> configure terminal
Router(#config) ip route 192.168.2.0 255.255.255.0 10.0.1.2
Router(#config) ip route 192.168.3.0 255.255.255.0 10.0.1.2

```

#### Router B

```
RouterB> configure terminal
RouterB(#config) ip route 192.168.1.0 255.255.255.0 10.0.1.1

RouterB(#config) ip route 192.168.3.0 255.255.255.0 10.0.2.2

```

#### Router C

```
RouterC> configure terminal
Router(#config) ip route 192.168.1.0 255.255.255.0 10.0.2.1
Router(#config) ip route 192.168.2.0 255.255.255.0 10.0.2.1

```


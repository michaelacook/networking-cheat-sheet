# CCNA Routing & Switching Commands Cheat Sheet

## Table of contents
- [CCNA Routing \& Switching Commands Cheat Sheet](#ccna-routing--switching-commands-cheat-sheet)
  - [Table of contents](#table-of-contents)
  - [General Show Commands](#general-show-commands)
  - [Basic Configuration](#basic-configuration)
  - [VLAN](#vlan)
    - [Trunking](#trunking)
    - [VTP](#vtp)
  - [Spanning Tree Protocol Configuration](#spanning-tree-protocol-configuration)
    - [Rapid Spanning Tree Protocol](#rapid-spanning-tree-protocol)
    - [Multiple Spanning Tree Protocol](#multiple-spanning-tree-protocol)
  - [Etherchannel](#etherchannel)
  - [Routing](#routing)
    - [Static Routing](#static-routing)
    - [Dynamic Routing](#dynamic-routing)
      - [RIP](#rip)
      - [EIGRP](#eigrp)
      - [OSPF](#ospf)

## General Show Commands 
```show running-config```

```show ip interface brief```

```show ipv6 interface brief```

```show vlan brief```

```show interface trunk```

```show interface [number] switchport```

```show interface [type][number]```

```show ipv6 interface [type][number]```

```show ip protocols```

[Back to top](#table-of-contents)

## Basic Configuration
Assign a hostname 

```
(config)# hostname [hostname]
```

Add a banner message 

```
(config)# banner motd $
```

Configure synchronous logging 

```
(config)# line console 0
(config-line)# logging synchronous
```

Set a password on the console line 

```
(config)# line console 0
(config-line)# password [password]
(config-line)# end
```

Assign an IP to a port 

```
(config)# interface [type][number]
(config-if)# ip address [IP] [subnet mask]
```

shutdown a port 

```
(config-if)# shutdown 
```

activate a port 
  
```
(config-if)# no shutdown
```

save current configuration to memory 

```
copy running-config startup-config

# or 

wr
```

[Back to top](#table-of-contents)

## VLAN
Create a vlan 

```
(config)# vlan [number]
(config-vlan)# name [name]
(config-vlan)# end
```

Assign ports to a vlan 

```
(config)# interface range [port range]

# configure port statically as an access port 

(config-if-range)# switchport mode access 
(config-if-range)# switchport access vlan [number]
```

Verify a vlan 

```
show vlan brief

# or 

show vlan 

# to view details about a port on a vlan

show interface [number] switchport
```

Remove a vlan

```
no vlan [number]
```

Set native VLAN for a port 

```
(config-if)# switchport trunk native vlan [number]
```

Create an interface vlan

```
(config)# int vlan [number]
(config-if)# ip address [IP] [subnet mask]
```

[Back to top](#table-of-contents)

### Trunking
Statically create a trunk port 

```
(config-if)# switchport mode trunk
```
Set encapsulation type 

```
# needed if the switch supports ISL encapsulation 

(config-if)# switchport trunk encapsulation [dot1q]
```

Verify trunking 

```
show interface trunk
```

Enable DTP

```
(config-if)# switchport mode dynamic [auto|desirable]
```

Disable DTP 

```
(config-if)# switchport nonegotiate
```

Restrict VLANs on a trunk

```
(config-if)# switchport trunk allowed vlan [vlan number(s)]
```

[Back to top](#table-of-contents)

### VTP 

Set VTP domain name for switch 

```
(config)# vtp domain [name]
```

Set VTP version

```
(config)# vtp version [1/2/3]
```

Set the VTP password 

```
(config)# vtp password [password]
```

Set role 

```
(config)# vtp mode [server/client/transparent/off]
```

Verify VTP

```
show vtp status

# on client switches 

show vlan brief
```

Assign switch as primary server

```
(config)# vtp primary

# follow prompts
```
[Back to top](#table-of-contents)

## Spanning Tree Protocol Configuration
Configure switch as root primary 

```
(config)# spanning-tree vlan [vlan number] priority [number]
```

```
(config)# spanning-tree vlan [vlan #] root primary
```
(#show-commands)
Verify spanning tree

```
show spanning-tree
```

Configure switch as root secondary

```
(config)# spanning-tree vlan [vlan #] root secondary
```

Change port cost 

```
(config-if)# spanning-tree vlan [vlan number] cost [cost]
```

Configure PortFast on a port 

```
(config-if)# spanning-tree portfast
(config-if)# spanning-tree bpduguard [enable|disable] // if using BPDU Guard with PortFast
```

Configure Root Guard on a port 

```
(config-if)# spanning-tree guard root
```

Modify timer 

```
(config)# spanning-tree vlan [vlan number] [hello-time|max-Age|forward-Time] [value]
```

Spanning Tree show commands 

```
show spanning-tree [vlan number]

# display information about the local switch - ID, timers, etc.
show spanning-tree bridge

# display information about the switch that the local switch considers root
show spanning-tree root

# display STP information abou the specified interface - role, status, priority, cost
show spanning-tree interface [int name/number]
```
### Rapid Spanning Tree Protocol

Enable Rapid Spanning Tree Protocol

```
(config)# spanning-tree mode rapid-pvst
```

Configure switch as RST root primary for a VLAN

```
(config)# spanning-tree vlan [vlan number] root primary
```

Configure switch as RST root secondary for a VLAN

```
(config)# spanning-tree vlan [vlan number] root secondary
```

### Multiple Spanning Tree Protocol

Enable Multiple Spanning Tree Protocol

```
(config)# spanning-tree mode mst
```

MST basic configuration

```
(config)# spanning-tree mst configuration
(config-mst)# name [name]
(config-mst)# revision [revision number]

# map VLANs to the instance
(config-mst)# instance [instance ID] vlan [vlan1[,vlan2,...,vlanN]]

# verify
(config-mst)# show pending

# commit changes 
(config-mst)# exit
```

[Back to top](#table-of-contents)

## Etherchannel
Aggregate a range of ports as an etherchannel 

```
(config-if-range)# shutdown 
(config-if-range)# channel-group [number] mode [active|passive|desirable|auto]
(config-if-range)# no shutdown
```

Verify an etherchannel 

```
# port channel should be SU (layer 2 and up)
# should have individual link status of P for bundled in the Port Channel

show etherchannel summary
```

[Back to top](#table-of-contents)

## Routing

### Static Routing

Assign an IPv4 address to a router interface 

```
(config)# interface [type][number]
(config-if)# ip address [address] [subnet mask]

# ensure interface is not administratively shut down 
(config-if)# no shutdown
```

Enable IPv6 globally 

```
(config)# ipv6 unicast-routing
```

Assign an IPv6 address to a router interface

```
# add link-local IPv6 address

(config-if)# ipv6 address fe80::[individual IP for the router] link-local

# configure a global unicast address

(config-if)# ipv6 address [address]/[prefix-length]
```

Create a loopback interface globally

```
(config)# interface loopback [loopback number]
``` 

Enable routing on a multi-layer switch

```
(config)# ip routing
```

Show IPv4 route table

```
show ip route 
```

Show IPv6 route table 

```
show ipv6 route
```

Statically add a route to the routing table 

```
(config)# ip route [destination subnet] [destination subnet mask] [next hop IP|local exit interface]
```

Configure a static default route 

```
(config)# ip route 0.0.0.0 0.0.0.0 [next hop IP|exit interface]
```

[Back to top](#table-of-contents)

### Dynamic Routing

Disable DNS lookup

```
(config)# no ip domain-lookup
```

#### RIP

Enable RIP protocol

```
(config)# router rip

(config)# version [version #]
```

Disable auto-summarization

```
(config-router)# no auto-summary
```

Advertise a network

```
(config-router)# network [subnet]
```

Configure passive interface

```
(config-router)# passive-interface [interface]
```

[Back to top](#table-of-contents)

#### EIGRP

Activate EIGRP on a router:

```
(config-router)# router eigrp [autonomous system number]
```

Assign a router ID

```
eigrp router-id [IPv4 address]

# Then configure networks to advertise
```

Assign EIGRP router ID

```
(config-router)# eigrp router-id [subnet]
```

Advertise route into EIGRP

```
(config-router)# network [subnet] [inverse mask]
```

Verify EIGRP neighbours

```
# show ip eigrp neigh
```

Verify EIGRP neighbours (verbose)

```
# show ip protocols
```

Verify router has learned EIGRP routes

```
# show ip route eigrp
```

Show neighbour table

```
# show ip eigrp neighbors
```

Show topology table - including successor routes

```
# show ip eigrp topology
```

Configure unequal cost load balancing 

```
(config-router)# variance [metric variance multiplier]
```

Configure passive interface

```
(configure-router)# passive-interface [interface name/number]
```

Change Hello & Dead timers 

```
(config-if)# ip hello-interval eigrp [1-65535]

(config-if)# ip hold-time eigrp [1-65535]
```

Configure bandwidth & delay values

```
(config-if)# bandwidth ?

(config-if)# delay ?
```

Disable automatic summarization

```
(config-router)# no auto-summary
```

Configure manual summarization

```
(config-if)# ip summary-address eigrp [AS number] [summary network] [summary netmask]
```

Configure router as stub router

```
(config-router)# eigrp stub [?]
```

[Back to top](#table-of-contents)

#### OSPF

Configure an OSPF instance on a router
- requires setting a router id, as in EIGRP

```
(config)# router ospf [process number]

(config-router)# router-id [x.x.x.x]
```

Advertise a network to an OSPF area

```
(router-config)# router ospf [process number]
(router-config)# network [subnet] [inverse mask] area [area number]
```

Remove OSPF process

```
(config)# clear ip ospf processes
```

Change reference bandwidth

```
(config-router)# auto-cost reference-bandwidth [number]
```

Change hello & dead timers

```
(config-if)# ip ospf hello-interval [interval]

(config-if)# ip ospf dead-interval [interval]
```

Show Link State Database

```
show ip ospf database
```

Show OSPF neighbors for a router

```
show ip ospf neighbor
```

Show routing protocols
- local router ID
- networks included in OSPF
- stub area configurations

```
(config)# show ip protocols
```

Show OSPF routing table

```
show ip route ospf
```

Summarize routes on Area Border Router

```
(config-router)# area [area number] range [subnet of summarized routes] [subnet mask of summarized route]
```

Configure stub area
- must be done for the area on all routers in the area

```
(config)# router ospf [process number]
(config-router)# area [area number] stub
```

Configure total stub area

```
(config)# router ospf [process number]
(config-router)# area [area number] stub no-summary
```

Configure passive interface

```
(config)# router ospf [process number]
(configure-router)# passive-interface [interface name/number]
```

Set all interfaces as passive by default

```
(config-router)# passive-interface default
```

Configure virtual link
- configure on both ends of the link

```
(config)# router ospf [process number]
(config-router)# area [transit area number] [peer/destination router ID]
```

[Back to top](#table-of-contents)

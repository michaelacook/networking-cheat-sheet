 Routing & Switching Commands Cheat Sheet

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
  - [Switch Security](#switch-security)
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

```show line```
- to view parameters for active lines that are in use such as the vty lines

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

## Switch Virtual Interface
- used for remote management of the device

```
# conf t
(config)# int vlan 1
(config-if)# ip address ip mask
(config-if)# no shut
(config-if)# exit
(config)# ip default-gateway ip
```

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

Change port priority

```
(config-if)# spanning-tree vlan [vlan id] cost [cost between 1-200000000]
```

Configure PortFast

```
# on an interface
(config-if)# spanning-tree portfast
(config-if)# spanning-tree bpduguard [enable|disable] // if using BPDU Guard with PortFast

# globally on all access ports running PortFast
(config)# spanning-tree portfast bpduguard default
```

Configure BPDU Guard
```
# On an interface
(config-if)# spanning-tree bpduguard enable

# Globally
(config)# spanning-tree portfast bpduguard default

# Disable BPDU Guard on an interface
(config-if)# spanning-tree bpduguard disable
```

Configure Root Guard on a port 

```
(config-if)# spanning-tree guard root
```

Modify timer 

```
(config)# spanning-tree vlan [vlan number] [hello-time|max-Age|forward-Time] [value]
```

Enable BackboneFast

```
# should be enabled for all switches in topology

(config)# spanning-tree backbonefast
```

Enable UplinkFast

```
# use on non-root access layer switches
# configured for all vlans

(config)# spanning-tree uplinkfast
```

Enable Loop Guard

```
# enable on all switches and ports
(config)# spanning-tree loopguard default

# enable on a specific interface
(config-if)# spanning-tree guard loop
```

Enable UDLD

```
(config-if)# udld [enable|aggressive|disable]
```

Enable BPDU filtering

```
# globally
(config)# spanning-tree portfast bpdufilter default

# per port
(config-if)# spanning-tree bpdufilter enable
```

Set STP link type

```
(config-if)# spanning-tree link-type [point-to-point|shared]
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

# display disabled ports
show spanning-tree inconsistentports
```

Enable Storm Control

```
# enable storm control based on traffic type and level
(config-if)# storm-control [broadcast|multicast|unicast] [level|bps|pps]

# specify action when storm control rule is violated
(config-if)# storm-control action [shutdown|trap]
```

### Rapid PVST

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

## Switch Security

Configure 802.1X port-based authentication

```
# enable AAA auth
(config)# aaa new-model

# define RADIUS server(s)
(config)# radius-server host [name/IP] key BigSecret

# define 802.1x auth method
(config)# aaa authentication dot1x default group radius

# enable 802.1x on the switch
(config)# dot1x system-auth-control

# enter ports to use authentication
(config)# int range [interface range]

# configure ports to use 802.1x
(config-if)# switchport access vlan [vlan number]
(config-if)# switchport mode access
(config-if)# dot1x port-control [auto|force-authorized|force-unauthorized]

# allow multiple hosts to access the port where required
(config-if)# dot1x host-mode multi-host
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

Activate EIGRP IPv6

```
(config-router)# ipv6 router eigrp [AS]
```

Configure IPv6 EIGRP on an interface

```
(config-if)# ipv6 eigrp [AS]
(config-if)# no shutdown
```

Assign a router ID

```
eigrp router-id [x.x.x.x]

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

Show all routes include those that are not successor or feasible successor

```
# show ip eigrp topology all-links
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

Change EIGRP K values

```
(config-router)# metric weights [tos=0] [k1] [k2] [k3] [k4] [k5]
```

Configure load sharing

```
(config-router)# maximum-paths [number]
```

Change active timer 

```
(config-router)# active-time [time]
```

Disable split horizon on a hub router

```
(config-if)# no ip split-horizon eigrp [AS number]
```

Configure bandwidth for EIGRP updates

```
(config)# interface [int.subint] [multipoint|point-to-point]
(config-subif)# ip bandwidth-percent eigrp [AS num] [%]
```

Create an offset list

```
(config)# access-list [num] permit [IP]
(config)# router eigrp [AS]
(config-router)# offset-list [acl num] [in|out] [offset value]
```

Configure route filtering

```
# create a deny acl
(config)# access-list [number] [permit|deny] [subnet] [inverse mask]

# apply filtering in EIGRP config
(config)# router eigrp [AS]
(config-router)# distribute-list [acl number] [in|out]
```

Create a static default route and inject into EIGRP

```
(config)# ip route 0.0.0.0 0.0.0.0 [next hop IP|exit interface]
(config)# redistribute static
```

Configure Named EIGRP

```
(config)# router eigrp [name]
(config-router)# address-family [family] autonomous-system [AS]
(config-router-af)# network [network eg. 0.0.0.0]
```

Enter and exit toplogy configuration

```
# this may not be the correct exec level - double check
(config)# topology base
```

[Back to top](#table-of-contents)

#### OSPF

View OSPF instance

```
# show ip ospf
```

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

Change interface bandwidth

```
(config-if)# bandwidth [num]
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
(config-router)# area [area number] range [subnet of summarized routes] [subnet mask of summarized route] [cost [num]]
```

Summarize routes redistributed into OSPF on an ASBR

```
(router-config)# summary-address [subnet & mask]
```

View details & cost of summary route

```
# show ip ospf database summary [subnet]
```

Create a default route on the ASBR

```
(config-router)# default-information originate always [metric {value}] [metric-type {type}] [route-map {name}]
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

View default routes known on a router

```
# show ip ospf database summary 0.0.0.0
```

View statistics on types of LSAs received

```
# show ip ospf database database-summary
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

View OSPF cost

```
# show ip ospf interface brief
# show ip ospf interface [type/num]
```

Filter Type 3 LSAs on ABR

```
(config)# router ospf [AS]
(router-config)$ area [number] filter-list prefix name [in|out]
```

Configure OSPF cost directly

```
(config-if)# ip ospf cost [value]
```

OSPFv3 Configuration

```
(config)# ipv6 unicast-routing
(config# ipv6 router ospf [ID]
(config-router)# router-id [x.x.x.x]

# advertise
(config-router)# ipv6 ospf [process ID] area [number] 
```

Verify OSPFv3 configuration

```
# show ipv6 ospf interface brief
# show ipv6 ospf neighbor
```

[Back to top](#table-of-contents)

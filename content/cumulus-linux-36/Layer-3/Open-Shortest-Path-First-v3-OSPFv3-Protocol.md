---
title: Open Shortest Path First v3 - OSPFv3 - Protocol
author: Cumulus Networks
weight: 179
aliases:
 - /display/CL36/Open+Shortest+Path+First+v3+++OSPFv3+++Protocol
 - /display/CL36/Open+Shortest+Path+First+v3+-+OSPFv3+-+Protocol
 - /display/CL36/Open+Shortest+Path+First+v3+OSPFv3+Protocol
 - /pages/viewpage.action?pageId=8362394
pageID: 8362394
---
OSPFv3 is a revised version of OSPFv2 to support the IPv6 address
family. Refer to 
[Open Shortest Path First (OSPF) Protocol](/cumulus-linux-36/Layer-3/Open-Shortest-Path-First-OSPF-Protocol)
for a discussion on the basic concepts, which remain the same between
the two versions.

OSPFv3 has changed the formatting in some of the packets and LSAs either
as a necessity to support IPv6 or to improve the protocol behavior based
on OSPFv2 experience. Most notably, v3 defines a new LSA, called
intra-area prefix LSA to separate out the advertisement of stub networks
attached to a router from the router LSA. It is a clear separation of
node topology from prefix reachability and lends itself well to an
optimized SPF computation.

{{%notice note%}}

IETF has defined extensions to OSPFv3 to support multiple address
families (that is, both IPv6 and IPv4).
[FRR](/cumulus-linux-36/Layer-3/FRRouting-Overview/) does not
support it yet.

{{%/notice%}}

## Configuring OSPFv3

Configuring OSPFv3 involves the following tasks:

1.  Enabling the `zebra` and `ospf6` daemons, as described in
    [Configuring FRRouting](/cumulus-linux-36/Layer-3/Configuring-FRRouting/)
    then start the FRRouting service:
    
        cumulus@switch:~$ sudo systemctl enable frr.service
        cumulus@switch:~$ sudo systemctl start frr.service

2.  Enabling OSPFv3 and map interfaces to areas:
    
        cumulus@switch:~$ net add ospf6 router-id 0.0.0.1
        cumulus@switch:~$ net add ospf6 interface swp1 area 0.0.0.0
        cumulus@switch:~$ net add ospf6 interface swp2 area 0.0.0.1

3.  Defining (custom) OSPFv3 parameters on the interfaces, such as:
    
    1.  Network type (such as point-to-point, broadcast)
    
    2.  Timer tuning (for example, hello interval)
    
        cumulus@switch:~$ net add interface swp1 ospf6 network point-to-point
        cumulus@switch:~$ net add interface swp1 ospf6 hello-interval 5
    
    The OSPFv3 configuration is saved in `/etc/frr/ospf6d.conf`.

{{%notice note%}}

Unlike OSPFv2, OSPFv3 intrinsically supports unnumbered interfaces.
Forwarding to the next hop router is done entirely using IPv6 link local
addresses. Therefore, you are not required to configure any global IPv6
address to interfaces between routers.

{{%/notice%}}

## Configuring the OSPFv3 Area

Different areas can be used to control routing. You can:

  - Limit an OSPFv3 area from reaching another area
  - Manage the size of the routing table by creating a summary route for
    all the routes in a particular address range.

The following example command removes the `3:3::/64` route from the
routing table. Without a route in the table, any destinations in that
network are not reachable.

    cumulus@switch:~$ net add ospf6 area 0.0.0.0 range 3:3::/64 not-advertise 

The following example command creates a summary route for all the routes
in the range 2001::/64:

    cumulus@switch:~$ net add ospf6 area 0.0.0.0 range 2001::/64 advertise

You can also configure the cost for a summary route, which is used to
determine the shortest paths to the destination. For example:

    cumulus@switch:~$ net add ospf6 area 0.0.0.0 range 11.1.1.1/24 cost 160

The value for cost must be between 0 and 16777215.

## Configuring the OSPFv3 Distance

Cumulus Linux provides several commands to change the administrative
distance for OSPF routes.

This example command sets the distance for an entire group of routes,
rather than a specific route.

    cumulus@switch:~$ net add ospf6 distance 254
    cumulus@switch:~$ net pending
    cumulus@switch:~$ net commit

This example command changes the OSPF administrative distance to 150 for
internal routes and 220 for external routes:

    cumulus@switch:~$ net add ospf6 distance ospf6 intra-area 150 inter-area 150 external 220
    cumulus@switch:~$ net pending
    cumulus@switch:~$ net commit

This example command changes the OSPF administrative distance to 150 for
internal routes:

    cumulus@switch:~$ net add ospf6 distance ospf6 intra-area 150 inter-area 150
    cumulus@switch:~$ net pending
    cumulus@switch:~$ net commit

This example command changes the OSPF administrative distance to 220 for
external routes:

    cumulus@switch:~$ net add ospf6 distance ospf6 external 220
    cumulus@switch:~$ net pending
    cumulus@switch:~$ net commit

This example command changes the OSPF administrative distance to 150 for
internal routes to a subnet or network inside the same area as the
router:

    cumulus@switch:~$ net add ospf6 distance ospf6 intra-area 150
    cumulus@switch:~$ net pending
    cumulus@switch:~$ net commit

This example command changes the OSPF administrative distance to 150 for
internal routes to a subnet in an area of which the router is *not* a
part:

    cumulus@switch:~$ net add ospf6 distance ospf6 inter-area 150
    cumulus@switch:~$ net pending
    cumulus@switch:~$ net commit

## Configuring OSPFv3 Interfaces

You can configure an interface, a bond interface, or a VLAN with an
advertise prefix list.  
The following example command configures interface swp3s1 with the IPv6
OSPF6 advertise prefix-list 192.168.0.0/24:

    cumulus@switch:~$ net add interface swp3s1 ospf6 advertise prefix-list 192.168.0.0/24
    cumulus@switch:~$ net pending
    cumulus@switch:~$ net commit

You can also configure the cost for a particular interface, bond
interface, or VLAN.  
The following example command configures the cost for the bond interface
swp2.

    cumulus@switch:~$ net add bond swp2 ospf6 cost 1
    cumulus@switch:~$ net pending
    cumulus@switch:~$ net commit

## Debugging OSPF

See [Debugging OSPF](/cumulus-linux-36/Layer-3/Open-Shortest-Path-First-OSPF-Protocol/#span-id-src-8362392-openshortestpathfirst-ospf-protocol-ospf-debug-class-confluence-anchor-link-debugging-ospf)
for OSPFv2 for the troubleshooting discussion. The equivalent commands
are:

    cumulus@switch:~$ net show ospf6 neighbor [detail|drchoice]
    cumulus@switch:~$ net show ospf6 database [adv-router|detail|dump|internal|linkstate-id|self-originated]
    cumulus@switch:~$ net show route ospf6

Another helpful command is `net show ospf6 spf tree`. It dumps the node
topology as computed by SPF to help visualize the network view.

## Related Information

  - [Bidirectional forwarding detection](/cumulus-linux-36/Layer-3/Bidirectional-Forwarding-Detection-BFD)
    (BFD) and OSPF
  - [en.wikipedia.org/wiki/Open\_Shortest\_Path\_First](http://en.wikipedia.org/wiki/Open_Shortest_Path_First)
  - [FRR OSPFv3](https://frrouting.org/user-guide/ospf6d.html)
  - [RFC 2740 OSPFv3 OSPF for IPv6](https://tools.ietf.org/html/rfc2740)
  - [Auto-cost reference bandwidth](/cumulus-linux-36/Layer-3/Open-Shortest-Path-First-OSPF-Protocol/#auto-cost-reference-bandwidth)
    (OSPFv2 chapter)

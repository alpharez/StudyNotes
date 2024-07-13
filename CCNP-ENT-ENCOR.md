# Encor notes

## Switching

### 802.1D

The original STP version.

#### port states

- disabled
- blocking
- listening
- learning
- forwarding
- broken

#### port types

- root port
- designated port
- blocking port

#### STP terms

- root bridge
- BPDU
- Root path cost
- system priority
- system id extension
- root bridge id
- local bridge id
- max age
- hello time
- forward delay

#### STP port costs

| Link Speed | Short-Mode STP Cost | Long-Mode STP Cost |
| --- | ------- | ------- |
| 10 Mbps | 100 | 2,000,000 |
| 100 Mbps | 19 | 200,000 |
| 1 Gbps | 4 | 20,000 |
| 10 Gbps | 2 | 2,000 |
| 20 Gbps | 1 | 1,000 |
| 100 Gbps | 1 | 200 |
| 1 Tbps | 1 | 20 |
| 10 Tbps | 1 | 2 |

Devices can be configured with the long mode interface cost with the command

`spanning-tree pathcost method long`

#### Root bridge election

STP deems a switch more preferable if the priority of the bridge ID is lower, if the priority is the same, then the switch prefers the BPDU with the lower system MAC.

#### Convergence

15 seconds listening state, 15 seconds learning state.

### PVST

### PVST+

### 802.1W RSTP

Time to Convergence?

#### Port States

- Discarding
- Learning
- Forwarding

#### Port Roles

- Root Port
- DP
- Alternate Port
- Backup Port

#### Port Types

- Edge Port
- Root Port
- Point-to-Point Port

### 802.1S MST

```ios
spanning-tree mode mst
spanning-tree mst 0 root primary
spanning-tree mst 1 root primary
spanning-tree mst 2 root primary
spanning-tree mst configuration
    name ENTERPRISE_CORE
    revision 2
    instance 1 vlan 10,20
```

### Etherchannel and Trunks

- [x] did labs in Eve-NG

#### Dynamic Link Agg Protocols

##### PAgP Port Modes

- Auto
- Desirable

#### LACP Port Modes

- Passive
- Active

## Routing

### Networks

- Class A - First bit of first octet is set to 0
- Class B - First 2 bits of first octet are always 1 0
- Class C - First 3 bits of the first octet are always 1 1 0

### Admin Distance

| Type | Distance |
| --- | --- |
| Connected | 0 |
| Static | 1 |
| EIGRP Summary | 5 |
| eBGP | 20 |
| EIGRP int | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| EIGRP ext | 170 |
| iBGP | 200 |

### EIGRP

EIGRP runs using protocol 88.

One of the only routing protocols that can do unequal cost load balancing.

DUAL - diffusing update algorithm

Uses bandwidth and delay metrics.

Successor route  - route with lowest path metric to reach a destination
Successor - the first next-hop router for the successor route
Feasible Distance (FD) - metric value for lowest metric path to a destination
Reported Distance (RD) - distance reported by a router to reach a prefix.  FD for advertising router.
Feasibility condition - for a route to be a backup route, the RD for that route must be less than the FD calculated locally.  This guarantees a loop-free path.

- [ ] do EVE-NG labs

#### packet types to communicate with other routers

| Type | Msg | Description |
| --- | --- | --- |
| 1 | Hello | Used for discovery of EIGRP neighbors and detecting when neighbor is not available |
| 2 | Request | Used to get info from one or more neighbors |
| 3 | Update | Used to transmit routing & reachability info |
| 4 | Query | Sent out to search for another path during convergence |
| 5 | Reply | Sent in response to a query |

#### metrics

| Link | Speed | Delay | Metric |
| --- | --- | --- | --- |
| Serial | 64 kbps | 20,000 &micro;s | 40,512,000 |
| T1 | 1544 | 20,000 &micro;s | 2,170,031 |
| Ethernet | 10,000 | 1000 &micro;s | 281,600 |
| FaEthernet | 100,000 | 100 &micro;s | 28,160 |
| GigEth | 1,000,000 | 10 &micro;s | 2816 |
| 10 Gig | 10,000,000 | 10 &micro;s | 512 |

#### wide metrics

#### load balancing

### OSPF

OSPF runs using protocol 89.  It uses multicast where possible to reduce unnecessary traffic.

- AllSPFRouters 224.0.0.5 or MAC 01:00:5E:00:00:05
- AllDRouters 224.0.0.6 or MAC 01:00:5E:00:00:06 for communication with designated routers

#### OSPF Packet types

| Type | Name | Function |
| --- | --- | --- |
| 1 | Hello | for discovering and maintaining neighbors |
| 2 | DB description | summarizing database contents |
| 3 | LSR | Link state request for DB downloads |
| 4 | LSU | Link state update for DB updates, normally a response to a LSR |
| 5 | LSA | Link state ack, flooding of acknowledgments |

| LSA Type | Description |
| --- | --- |
| Type 1 LSA | Advertises directly connected networks |
| Type 2 LSA | created for each transit network in an area where a DR is elected |
| Type 3 LSA | summary LSA sent from one area to another, used to advertise a network in the source area |
| Type 4 LSA | Summary ASBR LSA, created by an ABR to tell members of an area how to reach an ASBR |
| Type 5 LSA | AS External LSA, created by an ASBR to advertise networks in a different AS |
| Type 7 LSA | NSSA LSA from an ASBR into an NSSA to advertise networks from a different AS |

- ABR : Area Border Router
- ASBR : Autonomous System Boundary Router
- Stub Area : Only connects to Area 0
- Totally Stubby Area : Area that only needs a summary default route to get out
- Not-So-Stubby Area (NSSA) : Stub Area that connects to a different AS
- Totally NSSA : NSSA that sends a summary advertisement out

#### Hello Packet Fields

- Router ID         32 bit ID
- Auth Options
- Area ID
- Interface Address Mask
- Interface priority
- hello interval
- dead interval
- DR/BDR
- Active Neighbor

#### Neighbor States

- Down
- Attempt       relevant to NBMA networks
- Init          hello packet received from another router
- 2-Way         bidirectional communication
- ExStart       first state in forming adjacency
- Exchange      routers are exchanging links states by using DBD packets
- Loading       LSR packets are sent to the neighbor
- Full          neighbors fully adjacent

#### router ID

By default the RID is dynamically allocated using the highest IP of any up loopback interfaces, then highest IP of any active up phy interfaces.

you can set a static RID. with the command router-id *router-id*

Neighbors on a Broadcast network with elect DR/BDRs.  DR has the highest RID.

#### Link costs

Default reference BW is 100 Mbps.

OSPF Interface Costs Using Default Settings

| Link | Cost |
| --- | ---|
| T1 | 64 |
| Ethernet | 10 |
| Fast Eth | 1 |
| Gig | 1 |
| 10 Gig | 1 |

### OSPFv3

### BGP

### Multicast

Multicast uses Class D addresses (1110).

Range assignments
| Description | Range |
| --- | --- |
| Reserved Link Local | 224.0.0.0/24 |
| Globally scoped | 224.0.1.0 - 238.255.255.255 |
| Source Scoped | 232.0.0.0/8 |
| GLOP addresses | 233.0.0.0/8 |
| Limited Scope addresses | 239.0.0.0/8 |

Link Local Addresses
| IP Address | Usage |
| --- | --- |
| 224.0.0.1 | All systems on this subnet |
| 224.0.0.2 | All routers on this subnet |
| 224.0.0.5 | OSPF routers |
| 224.0.0.6 | OSPF designated routers |
| 224.0.0.12 | DHCP server/relay agent |

224.0.1.0 through 238.255.255.255 are called globally scoped addresses, used to multicast data between orgs and across the Internet.

224.0.1.1 - reserved for NTP

Source scoped addresses are reserved for Source Specific Mutlicast (SSM), an extension of the PIM protocol.

GLOP addresses reserved for statically defined addresses by orgs that have an AS number reserved.

Limited Scope addresses or administratively scoped addresses are constrained to a local group or an org.

#### L2 multicast

IANA owns a block of MAC addresses that start with 01:00:5E, half of this block is allocated for multicast: 0100.5e00.0000 through 0100.5e7f.ffff.  This allows for 23 bits in ethernet address to correspond to the IP multicast group address.

Because the upper 5 bits of the IP multicast address are dropped in the mapping, the resulting address is not unique.

## Services

### QoS

#### QoS Components

#### QoS Policy

- DSCP ?

### IP Services

#### NTP and PTP

#### NAT/PAT

#### HSRP and VRRP

#### Multicast protocols, such as RPF check, PIM and IGMP v2/v3

IGMP is used to dynamically register individual hosts in a multicast group on a particular LAN. Hosts identify group memberships by sending IGMP messages to their local multicast router. Under IGMP, routers listen to IGMP messages and periodically send out queries to discover which groups are active or inactive on a particular subnet.

In Version 2, the following four types of IGMP messages exist:

- Membership query
- Version 1 membership report
- Version 2 membership report
- Leave group

CGMP is a Cisco-developed protocol that allows Catalyst switches to leverage IGMP information on Cisco routers to make Layer 2 forwarding decisions. You must configure CGMP on the multicast routers and the Layer 2 switches. The result is that, with CGMP, IP multicast traffic is delivered only to those Catalyst switch ports that are attached to interested receivers. All other ports that have not explicitly requested the traffic will not receive it unless these ports are connected to a multicast router. Multicast router ports must receive every IP multicast data packet.

IGMP Snooping is an IP multicast constraining mechanism that runs on a Layer 2 LAN switch. IGMP Snooping requires the LAN switch to examine, or "snoop," some Layer 3 information (IGMP join/leave messages) in the IGMP packets sent between the hosts and the router. When the switch hears the IGMP host report from a host for a particular multicast group, the switch adds the port number of the host to the associated multicast table entry. When the switch hears the IGMP leave group message from a host, the switch removes the table entry of the host.

PIM is IP routing protocol-independent and can leverage whichever unicast routing protocols are used to populate the unicast routing table, including Enhanced Interior Gateway Routing Protocol (EIGRP), Open Shortest Path First (OSPF), Border Gateway Protocol (BGP), and static routes. PIM uses this unicast routing information to perform the multicast forwarding function. Although PIM is called a multicast routing protocol, it actually uses the unicast routing table to perform the RPF check function instead of building up a completely independent multicast routing table. Unlike other routing protocols, PIM does not send and receive routing updates between routers.

PIM Dense Mode (PIM-DM)
PIM-DM uses a push model to flood multicast traffic to every corner of the network. This push model is a brute force method for delivering data to the receivers. This method would be efficient in certain deployments in which there are active receivers on every subnet in the network.

PIM-DM initially floods multicast traffic throughout the network. Routers that have no downstream neighbors prune back the unwanted traffic. This process repeats every 3 minutes.

PIM Sparse Mode (PIM-SM)
PIM-SM uses a pull model to deliver multicast traffic. Only network segments with active receivers that have explicitly requested the data will receive the traffic.

PIM-SM distributes information about active sources by forwarding data packets on the shared tree. Because PIM-SM uses shared trees (at least, initially), it requires the use of a rendezvous point (RP). The RP must be administratively configured in the network.

Pragmatic General Multicast (PGM)
PGM is a reliable multicast transport protocol for applications that require ordered, duplicate-free, multicast data delivery from multiple sources to multiple receivers. PGM guarantees that a receiver in a multicast group either receives all data packets from transmissions and retransmissions or can detect unrecoverable data packet loss.

## Overlay

## Wireless

### Layer 1 concepts such as RF, power, RSSI, SNR, interference, noise, bands, channels, and wireless client devices capabilities

Power changes and their corresponding dB Values

| Power Change | dB Value |
| --- | --- |
| = | 0 dB |
| x2 | +3 dB |
| /2 | -3 dB |
| x10 | +10 dB |
| /10 | -10 dB |

RSSI - Received Signal Strength Indicator.  relative value 0-255 where 0 is weakest and 255 is strongest.

SNR - Signal to Noise ratio

Summary of common 802.11 standard amendments

| Standard | 2.4 GHz? | 5 GHz? | Data Rates Supported | Channel Widths Supported |
| --- | --- | --- | --- | --- |
| 802.11b | Yes | No | 1, 2, 5.5, 11 Mbps | 22 MHz |
| 802.11g | Yes | No | 6, 9, 12, 18, 24, 36, 48, 54 Mbps | 22 MHz |
| 802.11a | No | Yes | 6, 9, 12, 18, 24 ,36, 48, 54 Mbps | 20 MHz |
| 802.11n | Yes | Yes | Up to 150 Mbps per spatial stream, up to 4 spatial streams | 20 or 40 MHz |
| 802.11ac | No | Yes | Up to 866 Mbps per spatial stream, up to 4 spatial streams | 20, 40, 80 or 160 MHz |
| 802.11ax | Yes | Yes | Up to 1.2 Gbps per spatial stream, up to 8 spatial streams | 20, 40, 80, 160 MHz |

### AP modes and atennae types

### AP discovery and join process (discovery algorithms and WLC selection)

### Describe main principles and use cases for Layer 2 and Layer 3 roaming

### Troubleshoot using GUI only

### Describe wireless segmentation with groups, profiles and tags

## Architecture

### HW and SW switching mechanisms

#### CEF

CEF uses separate reachability info in the CEF table and the forwarding information in the adjacency table.  It can point directly to the forwarding info rather than to the recursed next hop in order to resolve recursive routes.

dCEF allows line card forwarding

#### CAM

#### TCAM

#### FIB

CEF Forwarding Information Base (FIB) - Layer 3 Info

#### RIB

#### Adjacency tables

CEF Adjacency Table - info on next hop devices

### Enterprise Network Design

### Wireless Network Design

#### Wireless Deployment models (centralized, distributed, controller-less, controller-based, cloud, remote branch)

#### Location services in a WLAN design

#### client density

### Cisco SD-WAN

#### SD-WAN control and data planes elements

#### Benefits and limitations of SD-WAN solutions

### Cisco SD-Access

#### SD-Access control and data planes elements

#### Traditional campus interoperating with SD-Access

### QoS Configurations

## Security

### Configure and verify device access control

### Configure and verify infrastructure security features

### Describe REST API security

### Configure and verify wireless security features

#### 802.1x

#### WebAuth

#### PSK

#### EAPOL (4-way handshake)

### Network Security Design

#### Threat Defense

#### Endpoint Security

#### Next Gen Firewall

#### TrustSec and MACsec

#### NAC with 802.1x, MAB and Webauth

## SDN

## Virtualization

### VRF, GRE and IPsec tunneling

### LISP and VXLAN

### Automation

## Network Assurance

### SNMP, debugs, syslog

### NetFlow

### SPAN

### IPSLA

### DNA Center

### NETCONF and RESTCONF

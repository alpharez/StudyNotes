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

Class A - First bit of first octet is set to 0
Class B - First 2 bits of first octet are always 1 0
Class C - First 3 bits of the first octet are always 1 1 0

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

Type        Name                Function
1           Hello               for discovering and maintaining neighbors
2           DB description      summarizing database contents
3           LSR                 Link state request for DB downloads
4           LSU                 Link state update for DB updates, normally a response to a LSR
5           LSA                 Link state ack, flooding of acknowledgments

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

## Services

### QoS

### IP Services

## Overlay

## Wireless

## Architecture

### Enterprise Network Design

### Wireless Network Design

### Cisco SD-WAN

### Cisco SD-Access

### QoS Configurations

## Security

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

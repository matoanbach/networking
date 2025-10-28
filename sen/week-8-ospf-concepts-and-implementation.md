# Understanding OSPF Concepts
## Objectives
- Comparing Dynamic Routing Protocol Features
- OSPF Concepts and Operation
- OSPF Areas and LSAs

## Comparing Dynamic Routing Protocol Features
- Routing protocols - These are used by routers to learn and share routes automatically. They exchange information with other routes to find the best path for data. Examples are `RIP`, `EIGRP`, `OSPF`, `BGP`.
- Routed (or Routable) Protocols - These are the actual procols that carry user data and have addressing built in so routers can foward the packets. Examples are `IPv4` and `IPv6`
- In shorts:
    - Routing protocols = how routers learn paths
    - Routed protolcols = what routes move data for (actual traffic)

## Routing Protocol Functions
1. Learn routing information about IP subnets from neighboring routers.
2. Advertise routing information about IP subnets to neighboring routers.
3. If more than one possible route exists to reach one subnet, pick the best route based on a metric.
4. If the network topology changes - for example, a link fails - react by advertising that some routes have failed and have failed and pick a new currently best route. (This process is called convergence)

<img src="https://github.com/matoanbach/networking/blob/main/pics/w8.1.png"/>

- Step 1. R2 advertises a route to the lower right subnet - 172.16.3.0/24 - to both router R1 and R3.
- Step 2. After R3 learns about the route to 172.16.3.0/24 from R2, R3 advertises that route to R1
- Step 3. R1 must make a decision about the two routes it learned about for reaching subnet 172.16.3.0/24 - one with metric 1 from R2 and one with metric 2 from R3. R1 chooses the lower metric route through R2 (function 3).

## Interior and Exterior Routing Protocols
- Ip routing protocols fall into one of two major categories: interior gateway protocols (IGP) or exterior gateway protocols (EGP). The definitions of each are as follows:
    - IGP: a routing protocol that was designed and intended for use inside a single autonomous system (AS)
    - EGP: A routing protocol that was designed and intended for use between different autonomous systems.

- An autonomous system (AS) is a network under the administrative control of a single organization.
- Some routing protocols work best inside a single AS by design, these are called IGPs
- Other routing protocols work best between ASs by design, these are called EGPs

## Comparing IGPs
### IGP Routing Protocol Algorithms
- Three main branches of routing protocol algorithms exist for IGP routing protocols:
    1. Distance vector (sometimes called Bellman-Ford after its creators)
    2. Advanced distance vector (sometimes called `balanced hybrid`)
    3. Link-state
- Distance vector protocols were invented first (RIP and IGRP)
- Link state protocols (OSPF and IS-IS) and advanced distance vector protocols (EIGRP) came later.

### IGP Routing Protocol Metrics
- Routing protocols choose the best route to reach a subnet by choosing the route with the lowest metric.
| IGP   | Metric                                   | Description                                                                                                                  |
|-------|------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
| RIPv2 | Hop count                                | The number of routers (hops) between a router and the destination subnet                                                     |
| OSPF  | Cost                                     | The sum of all interface cost settings for all links in a route, with the cost defaulting to be based on interface bandwidth |
| EIGRP | Calculation based on bandwidth and delay | Calculated based on the route's slowest link and the cumulative delay associated with each interface in the route            |

### Administrative Distance
- When two different routing protocols learn routes to the same subnet, IOS cannot compare the metrics.
- When IOS must choose between routes learned using different routing protocols it uses a concept called `administrative distance`.
- `administrative distance` is a number that denotes how believable an entire routing protocol is, on a single router.

| Route Type                   | Administrative Distance |
|------------------------------|-------------------------|
| Connected                    | 0                       |
| Static                       | 1                       |
| BGP (External routes - eBGP) | 20                      |
| EIGRP (Internal routes)      | 90                      |
| IGRP                         | 100                     |
| OSPF                         | 110                     |
| IS-IS                        | 115                     |
| RIP                          | 120                     |
| EIFRP (External routes)      | 170                     |
| BGP (Internal routes - iBGP) | 200                     |
| DHCP default route           | 254                     |
| Unusable                     | 255                     |

## OSPF Overview
- Link State protocols build IP routes with a couple of major steps:
    - The routers learn a lot of information about the network (Routers, links, IP addresses, status information, etc.)
    - The routers then flood this information so that all routers know the same information.
    - Each router can then calculate routes to all subnets from their own perspectives.

### Topology Information and LSAs
- OSPF organizes topology information using LSAs and the link-state database (LSDB)
- Each LSA is a data structure with some specific information about the network topology.
- The LSDB is a collection of all the LSAs known to a router.
- The flooding process has a way to prevent loops so that the LSAs do not get flooded around in circles:
    - Before sending an LSA to yet another neighbor, routers communicate which LSAs they already have.
    - Only LSAs that are not know are flooded.
- Routers reflood LSA information based on the LSAs separate aging timer (default 30 minutes)

### Applying Dijkstra Math to find the best routes
- To build routes, link state routes must do some math based on the topological information found in the LSDB.
- All link state protocols use a math algorithm called Dijkstra Shortest Path First (SPF) to process the LSDB.
- The algorithm analyzes the LSDB and builds the routes that the local route will add to the IP routing table.

### Becoming OSPF Neighbors
- OSPF neighbors are routers that both use OSPF, and both sit on the same data link.
- OSPF routers introduce themselves by sending Hello messages.
- Assuming the two neighbors have compatible OSPF parameters, the two will form a neighbor relationship.
- The OSPF neighbor relationship lets OSPF know when a neighbor might not be a good option for routing packets.
- The OSPF neighbor model allows new routers to by dynamically discovered.

### Meeting Neighbors and Learning Their Router-ID
- When OSPF exchanges `Hello` messsage they lust each router's `router-id` (RID).
- This RID serves as each router's unique name or identifier for OSPF.
- OSPF RIDs are `32-bit` numbers.
- IOS chooses its OSPF RID based on an active interface IPv4 address.
- The OSPF RID can also be directly configured.
- As soon as a router has choosen its OSPF RID's and some interfaces come up. the router is ready to meet its OSPF neighbors.
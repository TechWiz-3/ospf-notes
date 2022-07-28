# OSPF - Open Shortest Path First

OSPF - link state routing protocol. This means that it sends out the routing tables through link state info (LSAs).  

LSDB = Linkstate database = Topology map for the topology  
LSA = information, however the actual packet data is the LSU
LSU = Link State Update  
LSR = Link State Request = request for route information from info - neighbour responds with LSU - acknowledges with LSack  

Path selection for OSPF uses the Shortest Path First algo (Dijkstra's algo)  
Link state information is flooded through the network allowing the network ospf routers to be in sync.  

## OSPF Areas

A network where routers all have the same routing information = OSPF area - i.e. network updates are localised and router share topology information.

Big networks are split up into different 'areas' to make things more manageable and reduce network traffic  

![Image](./media/a.png)

Area 0 is the backbone - all other areas need to be connected to area 0 resulting in a **heirarchical design**  

Size? How many routers in each area? The old recommendation was if you get to 50, break the area up.  

## OSPF Parameters

OSPF routers send 'hello' packets to eachother, like good neighbours ought to. This:
* confirms that they are neighbours
* confirms they are reachable
* info that results in an evaluation of whether or not the neighbour relationship should be built on (no house warming gifts though)

To meet neighbour requirements and join the club:
* must be in the same area (ok doh)
* identical fields
  * hello timer (you wouldn't want your neighbour saying hello to you at 1 am)
  * dead timer (who wants to die before their neighbour)
  * authentication (vet your neighbour, don't give em the house keys unless they're trustworthy)
  * maximum transmission unit
  * stub flags

### Hello Timer

> Interval in seconds at which a router sends hello message out of an OSPF interface.  

By default 10 seconds on broadcast/p2p, 30 seconds on non broadcast multi access networks

### Dead Timer

> Time in seconds an OSPF will wait to receive a hello message before deciding the neighbour is down  

4x hello timer by default  

##### In exec mode `show ip space ospf interface`


### Wait Timer

> Number of seconds a router waits for a designated router before beginning an 'election'  

(Info on designated routers coming up)

### Retransmit time

> Number of seconds a router waits before retransmitting an OSPF packet that hasn't been acknowledged

### Change Timers

In interface config `ip ospf ?`  

Example:  
`ip ospf hello-interval ?` to see accepted values

### Back to the neighbours

Other things that need to match as mentioned above (no, above that, nah nah, more, yeahh)  
* Authentication - if it enabled
* MTU (no explanation provided :thonk:)  
* Stub flags (^^)  

When sending hello packets to all other routers -> multicast address `224.0.0.5` or `FF02::5` (IPv6)

## OSPF DR/BDR election (be responsible, never skip elections, your vote counts {and some such words})

So just because these routers have established they are neighbours, doesn't mean they're all gonna be close with eachother (Gina Gateway across the street's parties are too loud, Mary Modem next door plays violin at 3 am and Robert Router smokes 10 times a day)  

> Router adjacencies (neighbour best buddies): neighbour routers that share LSUs (Link State Update - topology info update) and database description packates  

*Think of it like having daily LSU parties where you gossip with your router adjacencies about the latest routers and topology changes*  

### OSPF Adjacencies Formula (the formula for best neighbours)

**[n*(n-1)]/2**
n = number of routers

`4 routers = 6 adjacencies`  
`10 routers = 45 adjacencies`  

To reduce the number of adjacencies, ospf routers elect a designated router (DR) and a backup designated router (BDR)  
So routers in a network only need to form adjacencies with the DR OR the BDR. Less ~~neighbours~~ adjacencies less headaches right?


`show ip ospf neighbour` - shows neighbours (not adjacencies tho)  

### BR/DBR Election

Factors:
1. Priority level (default is 1) - `sh ip ospf priority <#>` to set a router's priority
  * Candidacy destroyer - set priority level to 0 if you don't want the router to join the election
2. Router ID - `router ospf 1` `router-id <id>`
  * Router ID is in IP address format
  * the router will apply the highest interface IP adderss as it's router ID
3. Loopback interface IP address - highest loopback address wins
4. Interface IP address - `sh ip ospf topology-info` - `show ip int br`
  * the router will apply the highest interface IP adderss as it's router ID

## OSPF Network Types

Since OSPF is so widely used it supports many different types of networks.  

**Point to Point**  
Link between only two points i.e. two router connecting to eachother  
Default ospf network type on a non ospf network type on a non frame relay serial interface  
No need for elections as the neighbourhood is too small for a president xD (since there are only two routers and two candidate for DR and DBR)

**Broadcast Network**  
Default ospf network type for ethernet interfaces.  
Broadcast message received by the entire subnet, thus DR and DBR are required.  

**Non broadcast multiple access**  
Default on a physical Frame Relay serial interface  
Hello packets are transmitted as unicast packets to each of their adjacent neighbours  
There is DR/BDR election  
You need to explicitly specify the neighbour's IP address in this network  

**Point to Multipoint**  
The permanent virtual circuits are organised into a collection of point-to-point-networks  
No DR/DBR election in a point-to-multipoint network type  

![Image](./media/b.png)  

## ABR vs ASBR Routers

ABR = area border router - more processing power to support multiple areas and routing tables - handle info for two different areas  
ASBR = autonmous system boundary router - used to connect an ospf network to a non-ospf network  

ABR's and ASBR's are like bridges between two networks. Instead of sending full route information, they send summarised information to other areas, this is called router summarisation.  
ABR's and ASBR's are not configured to be so, they automatically become so when placed between areas.

## LSA Types

The LSA (link-state advertisement) has a number of types:
* Router LSA - created by each router, used to advertise directly attached networks  
* Network LSA - created in transit networks for DR and BDR elections
* Summary LSA - created by Area Border Routers (ABRs) to advertise networks/areas to eachther
* Type 4 and 5 are created by ASBR's for advertisement to a non-ospf network

To check in exec mode `sh ip ospf database`  

## 

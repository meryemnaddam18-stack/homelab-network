=== HQ-R1: show ip ospf neighbor ===
Proves: adjacency with BR-R1 (router ID 2.2.2.2) is in FULL state


HQ-R1#show ip ospf neighbor 


Neighbor ID     Pri   State           Dead Time   Address         Interface
2.2.2.2           0   FULL/  -        00:00:39    10.0.0.2        GigabitEthernet0/0/1
HQ-R1#

=== BR-R1: show ip ospf neighbor ===
Proves: adjacency with HQ-R1 (router ID 1.1.1.1) is in FULL state

BR-R1#show ip ospf neighbor 


Neighbor ID     Pri   State           Dead Time   Address         Interface
1.1.1.1           0   FULL/  -        00:00:36    10.0.0.1        GigabitEthernet0/0/1
BR-R1#

=== HQ-R1: show ip protocols ===
Proves: router ID 1.1.1.1, networks advertised, passive interfaces on the user VLANs

HQ-R1#show ip protocols

Routing Protocol is "ospf 1"
  Outgoing update filter list for all interfaces is not set 
  Incoming update filter list for all interfaces is not set 
  Router ID 1.1.1.1
  It is an autonomous system boundary router
  Redistributing External Routes from,
  Number of areas in this router is 1. 1 normal 0 stub 0 nssa
  Maximum path: 4
  Routing for Networks:
    192.168.10.0 0.0.0.255 area 0
    192.168.20.0 0.0.0.255 area 0
    192.168.99.0 0.0.0.255 area 0
    10.0.0.0 0.0.0.3 area 0
  Passive Interface(s): 
    GigabitEthernet0/0/0.10
    GigabitEthernet0/0/0.20
    GigabitEthernet0/0/0.99
  Routing Information Sources:  
    Gateway         Distance      Last Update 
    1.1.1.1              110      00:09:02
    2.2.2.2              110      00:09:39
  Distance: (default is 110)

=== HQ-R1: show ip route ===
Proves: AFTER OSPF, HQ has an O route to 192.168.30.0/24 and a static default route to the ISP

HQ-R1#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is 203.0.113.1 to network 0.0.0.0

     10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C       10.0.0.0/30 is directly connected, GigabitEthernet0/0/1
L       10.0.0.1/32 is directly connected, GigabitEthernet0/0/1
     192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.10.0/24 is directly connected, GigabitEthernet0/0/0.10
L       192.168.10.1/32 is directly connected, GigabitEthernet0/0/0.10
     192.168.20.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.20.0/24 is directly connected, GigabitEthernet0/0/0.20
L       192.168.20.1/32 is directly connected, GigabitEthernet0/0/0.20
O    192.168.30.0/24 [110/2] via 10.0.0.2, 00:09:32, GigabitEthernet0/0/1
     192.168.99.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.99.0/24 is directly connected, GigabitEthernet0/0/0.99
L       192.168.99.1/32 is directly connected, GigabitEthernet0/0/0.99
     203.0.113.0/24 is variably subnetted, 2 subnets, 2 masks
C       203.0.113.0/30 is directly connected, GigabitEthernet0/0/2
L       203.0.113.2/32 is directly connected, GigabitEthernet0/0/2
S*   0.0.0.0/0 [1/0] via 203.0.113.1

=== BR-R1: show ip route ===
Proves: branch learned the HQ VLANs (O) and the default route (O*E2) from HQ-R1


BR-R1#show ip route 
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is 10.0.0.1 to network 0.0.0.0

     10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C       10.0.0.0/30 is directly connected, GigabitEthernet0/0/1
L       10.0.0.2/32 is directly connected, GigabitEthernet0/0/1
O    192.168.10.0/24 [110/2] via 10.0.0.1, 00:10:08, GigabitEthernet0/0/1
O    192.168.20.0/24 [110/2] via 10.0.0.1, 00:10:08, GigabitEthernet0/0/1
     192.168.30.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.30.0/24 is directly connected, GigabitEthernet0/0/0
L       192.168.30.1/32 is directly connected, GigabitEthernet0/0/0
O    192.168.99.0/24 [110/2] via 10.0.0.1, 00:10:08, GigabitEthernet0/0/1
O*E2 0.0.0.0/0 [110/1] via 10.0.0.1, 00:10:08, GigabitEthernet0/0/1


=== AFTER OSPF: Staff-PC1: ping 192.168.30.11 (Branch-PC1) ===
Proves: HQ and branch can now reach each other (failed in phase 3)

C:\>ping 192.168.30.11

Pinging 192.168.30.11 with 32 bytes of data:

Request timed out.
Reply from 192.168.30.11: bytes=32 time<1ms TTL=126
Reply from 192.168.30.11: bytes=32 time=10ms TTL=126
Reply from 192.168.30.11: bytes=32 time=1ms TTL=126

Ping statistics for 192.168.30.11:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 10ms, Average = 3ms


=== Branch-PC1: ping 192.168.10.11 (Staff-PC1) ===
Proves: routing works in both directions

C:\>ping 192.168.10.11

Pinging 192.168.10.11 with 32 bytes of data:

Request timed out.
Reply from 192.168.10.11: bytes=32 time<1ms TTL=126
Reply from 192.168.10.11: bytes=32 time=1ms TTL=126
Reply from 192.168.10.11: bytes=32 time=1ms TTL=126

Ping statistics for 192.168.10.11:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms

C:\>
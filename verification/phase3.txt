=== HQ-R1: show ip interface brief ===
Proves: subinterfaces G0/0/0.10, .20, .99 are up/up with the correct gateway IPs

HQ-R1#show ip interface brief 
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES manual up                    up 
GigabitEthernet0/0/0.10192.168.10.1    YES manual up                    up 
GigabitEthernet0/0/0.20192.168.20.1    YES manual up                    up 
GigabitEthernet0/0/0.99192.168.99.1    YES manual up                    up 
GigabitEthernet0/0/0.999unassigned      YES unset  up                    up 
GigabitEthernet0/0/1   10.0.0.1        YES manual up                    up 
GigabitEthernet0/0/2   203.0.113.2     YES manual up                    up
HQ-R1#

=== HQ-R1: show ip route connected ===
Proves: connected routes exist for 192.168.10.0, 20.0 and 99.0

HQ-R1#show ip route connected
 C   10.0.0.0/30  is directly connected, GigabitEthernet0/0/1
 C   192.168.10.0/24  is directly connected, GigabitEthernet0/0/0.10
 C   192.168.20.0/24  is directly connected, GigabitEthernet0/0/0.20
 C   192.168.99.0/24  is directly connected, GigabitEthernet0/0/0.99
 C   203.0.113.0/30  is directly connected, GigabitEthernet0/0/2

=== Staff-PC1: ping 192.168.10.1 ===
Proves: Staff PC reaches its gateway

C:\>ping 192.168.10.1

Pinging 192.168.10.1 with 32 bytes of data:

Reply from 192.168.10.1: bytes=32 time<1ms TTL=255
Reply from 192.168.10.1: bytes=32 time=1ms TTL=255
Reply from 192.168.10.1: bytes=32 time=1ms TTL=255
Reply from 192.168.10.1: bytes=32 time=1ms TTL=255

Ping statistics for 192.168.10.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms

C:\>

=== Staff-PC1: ping 192.168.20.11 ===
Proves: inter-VLAN routing works (Staff -> Guest)

C:\>ping 192.168.20.11

Pinging 192.168.20.11 with 32 bytes of data:

Request timed out.
Reply from 192.168.20.11: bytes=32 time=1ms TTL=127
Reply from 192.168.20.11: bytes=32 time=2ms TTL=127
Reply from 192.168.20.11: bytes=32 time<1ms TTL=127

Ping statistics for 192.168.20.11:
    Packets: Sent = 4, Received = 3, Lost = 1 (25% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 2ms, Average = 1ms

C:\>

=== Admin-PC: ping 192.168.10.11 ===
Proves: Management VLAN reaches the user VLAN

C:\>ping 192.168.10.11

Pinging 192.168.10.11 with 32 bytes of data:

Reply from 192.168.10.11: bytes=32 time<1ms TTL=127
Reply from 192.168.10.11: bytes=32 time<1ms TTL=127
Reply from 192.168.10.11: bytes=32 time<1ms TTL=127
Reply from 192.168.10.11: bytes=32 time=1ms TTL=127

Ping statistics for 192.168.10.11:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms

C:\>

=== Branch-PC1: ping 192.168.30.1 ===
Proves: branch LAN is up

C:\>ping 192.168.30.1

Pinging 192.168.30.1 with 32 bytes of data:

Reply from 192.168.30.1: bytes=32 time<1ms TTL=255
Reply from 192.168.30.1: bytes=32 time<1ms TTL=255
Reply from 192.168.30.1: bytes=32 time<1ms TTL=255
Reply from 192.168.30.1: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.30.1:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms

=== Admin-PC: ssh -l admin 192.168.99.1 ===
Proves: SSH to HQ-R1 through the management VLAN gateway

C:\>ssh -l admin 192.168.99.1

Password: 

 Authorized access only. Activity is monitored. 

HQ-R1#
HQ-R1#

=== BEFORE OSPF: Staff-PC1: ping 192.168.30.11 (expected to fail) ===
Proves: HQ has no route to the branch LAN yet; fixed in phase 5

C:\>ping 192.168.30.11

Pinging 192.168.30.11 with 32 bytes of data:

Reply from 192.168.10.1: Destination host unreachable.
Request timed out.
Reply from 192.168.10.1: Destination host unreachable.
Reply from 192.168.10.1: Destination host unreachable.

Ping statistics for 192.168.30.11:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

C:\>

=== BEFORE OSPF: HQ-R1: show ip route ===
Proves: no route to 192.168.30.0/24


HQ-R1#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2, E - EGP
       i - IS-IS, L1 - IS-IS level-1, L2 - IS-IS level-2, ia - IS-IS inter area
       * - candidate default, U - per-user static route, o - ODR
       P - periodic downloaded static route

Gateway of last resort is not set

     10.0.0.0/8 is variably subnetted, 2 subnets, 2 masks
C       10.0.0.0/30 is directly connected, GigabitEthernet0/0/1
L       10.0.0.1/32 is directly connected, GigabitEthernet0/0/1
     192.168.10.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.10.0/24 is directly connected, GigabitEthernet0/0/0.10
L       192.168.10.1/32 is directly connected, GigabitEthernet0/0/0.10
     192.168.20.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.20.0/24 is directly connected, GigabitEthernet0/0/0.20
L       192.168.20.1/32 is directly connected, GigabitEthernet0/0/0.20
     192.168.99.0/24 is variably subnetted, 2 subnets, 2 masks
C       192.168.99.0/24 is directly connected, GigabitEthernet0/0/0.99
L       192.168.99.1/32 is directly connected, GigabitEthernet0/0/0.99
     203.0.113.0/24 is variably subnetted, 2 subnets, 2 masks
C       203.0.113.0/30 is directly connected, GigabitEthernet0/0/2
L       203.0.113.2/32 is directly connected, GigabitEthernet0/0/2

HQ-R1#
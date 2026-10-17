=== Guest-PC1: ping 8.8.8.8 ===
Proves: Guest keeps internet access
C:\>ping 8.8.8.8

Pinging 8.8.8.8 with 32 bytes of data:

Reply from 8.8.8.8: bytes=32 time<1ms TTL=254
Reply from 8.8.8.8: bytes=32 time=1ms TTL=254
Reply from 8.8.8.8: bytes=32 time=1ms TTL=254
Reply from 8.8.8.8: bytes=32 time=1ms TTL=254

Ping statistics for 8.8.8.8:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 1ms, Average = 0ms

=== Guest-PC1: ping Staff / Admin / Branch ===
Proves: Guest is isolated from internal networks (rule 1)

C:\>ping 192.168.10.11

Pinging 192.168.10.11 with 32 bytes of data:

Reply from 192.168.20.1: Destination host unreachable.
Reply from 192.168.20.1: Destination host unreachable.
Reply from 192.168.20.1: Destination host unreachable.
Reply from 192.168.20.1: Destination host unreachable.

Ping statistics for 192.168.10.11:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

C:\>
C:\>ping 192.168.30.11

Pinging 192.168.30.11 with 32 bytes of data:

Reply from 192.168.20.1: Destination host unreachable.
Reply from 192.168.20.1: Destination host unreachable.
Reply from 192.168.20.1: Destination host unreachable.
Reply from 192.168.20.1: Destination host unreachable.

Ping statistics for 192.168.30.11:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

C:\>ping 192.168.99.11

Pinging 192.168.99.11 with 32 bytes of data:

Reply from 192.168.20.1: Destination host unreachable.
Reply from 192.168.20.1: Destination host unreachable.
Reply from 192.168.20.1: Destination host unreachable.
Reply from 192.168.20.1: Destination host unreachable.

Ping statistics for 192.168.99.11:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
=== Guest-PC1: ipconfig /renew ===
Proves: DHCP still works through the ACL

C:\>ipconfig /renew

   IP Address......................: 192.168.20.11
   Subnet Mask.....................: 255.255.255.0
   Default Gateway.................: 192.168.20.1
   DNS Server......................: 8.8.8.8

C:\>


=== Staff-PC1: ssh -l admin 192.168.10.1 ===
Proves: SSH from outside VLAN 99 is refused (rule 2)

C:\>ssh -l admin 192.168.30.1

% Connection timed out; remote host not responding

=== HQ-R1: show access-lists ===
Proves: match counters show each rule was used

HQ-R1#show access-lists 
Standard IP access list NAT
Standard IP access list NAT-INSIDE
    10 permit 192.168.10.0 0.0.0.255 (16 match(es))
    20 permit 192.168.20.0 0.0.0.255 (16 match(es))
    30 permit 192.168.30.0 0.0.0.255 (16 match(es))
    40 permit 192.168.99.0 0.0.0.255
Extended IP access list GUEST-IN
    10 permit udp any any eq bootps (2 match(es))
    20 deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255 (8 match(es))
    30 deny ip 192.168.20.0 0.0.0.255 192.168.99.0 0.0.0.255 (4 match(es))
    40 deny ip 192.168.20.0 0.0.0.255 192.168.30.0 0.0.0.255 (28 match(es))
    50 deny ip 192.168.20.0 0.0.0.255 10.0.0.0 0.0.0.3
    60 permit ip 192.168.20.0 0.0.0.255 any (8 match(es))
Standard IP access list MGMT-SSH
    10 permit 192.168.99.0 0.0.0.255
    20 deny any

=== HQ-R1: show ip interface g0/0/0.20 ===
Proves: GUEST-IN is applied inbound on the Guest subinterface
HQ-R1#show ip interface g0/0/0.20
GigabitEthernet0/0/0.20 is up, line protocol is up (connected)
  Internet address is 192.168.20.1/24
  Broadcast address is 255.255.255.255
  Address determined by setup command
  MTU is 1500 bytes
  Helper address is not set
  Directed broadcast forwarding is disabled
  Outgoing access list is not set
  Inbound  access list is GUEST-IN
  Proxy ARP is enabled
  Security level is default
  Split horizon is enabled
  ICMP redirects are always sent
  ICMP unreachables are always sent
  ICMP mask replies are never sent
  IP fast switching is disabled
  IP fast switching on the same interface is disabled
  IP Flow switching is disabled
  IP Fast switching turbo vector
  IP multicast fast switching is disabled
  IP multicast distributed fast switching is disabled
  Router Discovery is disabled
  IP output packet accounting is disabled
  IP access violation accounting is disabled
  TCP/IP header compression is disabled
  RTP/IP header compression is disabled
  Probe proxy name replies are disabled
  Policy routing is disabled
  Network address translation is disabled
  BGP Policy Mapping is disabled
  Input features: MCI Check
  WCCP Redirect outbound is disabled
  WCCP Redirect inbound is disabled
  WCCP Redirect exclude is disabled

HQ-R1#
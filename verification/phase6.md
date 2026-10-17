
===  Staff-PC1: ping 8.8.8.8 ===
Proves: Staff VLAN reaches the internet

C:\>ping 8.8.8.8

Pinging 8.8.8.8 with 32 bytes of data:

Reply from 8.8.8.8: bytes=32 time<1ms TTL=254
Reply from 8.8.8.8: bytes=32 time=1ms TTL=254
Reply from 8.8.8.8: bytes=32 time=15ms TTL=254
Reply from 8.8.8.8: bytes=32 time=1ms TTL=254

Ping statistics for 8.8.8.8:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 15ms, Average = 4ms

=== Guest-PC1: ping 8.8.8.8 ===
Proves: Guest VLAN has internet access

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

=== Admin-PC: ping 8.8.8.8 ===
Proves: Management VLAN has internet access
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

=== Branch-PC1: ping 8.8.8.8 ===
Proves: branch traffic crosses OSPF to HQ and is translated by HQ-R1

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

=== HQ-R1: show ip nat translations ===
Proves: PAT, multiple inside local addresses share one inside global address (203.0.113.2) using different ports

HQ-R1#show ip nat translations 
Pro  Inside global     Inside local       Outside local      Outside global
icmp 203.0.113.2:10    192.168.30.11:10   8.8.8.8:10         8.8.8.8:10
icmp 203.0.113.2:11    192.168.30.11:11   8.8.8.8:11         8.8.8.8:11
icmp 203.0.113.2:12    192.168.30.11:12   8.8.8.8:12         8.8.8.8:12
icmp 203.0.113.2:21    192.168.10.12:21   8.8.8.8:21         8.8.8.8:21
icmp 203.0.113.2:22    192.168.10.12:22   8.8.8.8:22         8.8.8.8:22
icmp 203.0.113.2:23    192.168.10.12:23   8.8.8.8:23         8.8.8.8:23
icmp 203.0.113.2:24    192.168.10.12:24   8.8.8.8:24         8.8.8.8:24
icmp 203.0.113.2:9     192.168.30.11:9    8.8.8.8:9          8.8.8.8:9

HQ-R1#

=== HQ-R1: show ip nat statistics ===
Proves: G0/0/2 is outside; G0/0/1 and the three subinterfaces are inside; hits are counting


HQ-R1#show ip nat statistics 
Total translations: 4 (0 static, 4 dynamic, 4 extended)
Outside Interfaces: GigabitEthernet0/0/2
Inside Interfaces: GigabitEthernet0/0/1 , GigabitEthernet0/0/0.10 , GigabitEthernet0/0/0.20 , GigabitEthernet0/0/0.99
Hits: 20  Misses: 20
Expired translations: 16
Dynamic mappings:
HQ-R1#
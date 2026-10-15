=== HQ-SW1: show vlan brief ===
Proves: each access port is in the correct VLAN (Staff 10, Guest 20, Admin 99, unused 998)
HQ-SW1#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
10   STAFF                            active    Fa0/1, Fa0/2
20   GUEST                            active    Fa0/3
99   MGMT                             active    Fa0/4
998  PARKING                          active    Fa0/5, Fa0/6, Fa0/7, Fa0/8
                                                Fa0/9, Fa0/10, Fa0/11, Fa0/12
                                                Fa0/13, Fa0/14, Fa0/15, Fa0/16
                                                Fa0/17, Fa0/18, Fa0/19, Fa0/20
                                                Fa0/21, Fa0/22, Fa0/23, Fa0/24
                                                Gig0/2
999  NATIVE                           active    
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    

=== HQ-SW1: show interfaces trunk ===
Proves: G0/1 is trunking, native VLAN 999, allowed VLANs 10,20,99,999
HQ-SW1#show interface trunk
Port        Mode         Encapsulation  Status        Native vlan
Gig0/1      on           802.1q         trunking      999

Port        Vlans allowed on trunk
Gig0/1      10,20,99,999

Port        Vlans allowed and active in management domain
Gig0/1      10,20,99,999

Port        Vlans in spanning tree forwarding state and not pruned
Gig0/1      10,20,99,999

=== HQ-SW1: show ip interface brief ===
Proves: Vlan99 management interface is up/up, unused ports are shut down

HQ-SW1#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
FastEthernet0/1        unassigned      YES manual up                    up 
FastEthernet0/2        unassigned      YES manual up                    up 
FastEthernet0/3        unassigned      YES manual up                    up 
FastEthernet0/4        unassigned      YES manual up                    up 
FastEthernet0/5        unassigned      YES manual administratively down down 
FastEthernet0/6        unassigned      YES manual administratively down down 
FastEthernet0/7        unassigned      YES manual administratively down down 
FastEthernet0/8        unassigned      YES manual administratively down down 
FastEthernet0/9        unassigned      YES manual administratively down down 
FastEthernet0/10       unassigned      YES manual administratively down down 
FastEthernet0/11       unassigned      YES manual administratively down down 
FastEthernet0/12       unassigned      YES manual administratively down down 
FastEthernet0/13       unassigned      YES manual administratively down down 
FastEthernet0/14       unassigned      YES manual administratively down down 
FastEthernet0/15       unassigned      YES manual administratively down down 
FastEthernet0/16       unassigned      YES manual administratively down down 
FastEthernet0/17       unassigned      YES manual administratively down down 
FastEthernet0/18       unassigned      YES manual administratively down down 
FastEthernet0/19       unassigned      YES manual administratively down down 
FastEthernet0/20       unassigned      YES manual administratively down down 
FastEthernet0/21       unassigned      YES manual administratively down down 
FastEthernet0/22       unassigned      YES manual administratively down down 
FastEthernet0/23       unassigned      YES manual administratively down down 
FastEthernet0/24       unassigned      YES manual administratively down down 
GigabitEthernet0/1     unassigned      YES manual up                    up 
GigabitEthernet0/2     unassigned      YES manual administratively down down 
Vlan1                  unassigned      YES manual administratively down down 
Vlan99                 192.168.99.2    YES manual up                    up

=== BR-SW1: show vlan brief ===
Proves: Branch PC and uplink in VLAN 30, unused ports in VLAN 998
BR-SW1#show vlan brief

VLAN Name                             Status    Ports
---- -------------------------------- --------- -------------------------------
1    default                          active    
30   BRANCH-USERS                     active    Fa0/1, Gig0/1
998  PARKING                          active    Fa0/2, Fa0/3, Fa0/4, Fa0/5
                                                Fa0/6, Fa0/7, Fa0/8, Fa0/9
                                                Fa0/10, Fa0/11, Fa0/12, Fa0/13
                                                Fa0/14, Fa0/15, Fa0/16, Fa0/17
                                                Fa0/18, Fa0/19, Fa0/20, Fa0/21
                                                Fa0/22, Fa0/23, Fa0/24, Gig0/2
1002 fddi-default                     active    
1003 token-ring-default               active    
1004 fddinet-default                  active    
1005 trnet-default                    active    
BR-SW1#

=== Admin-PC: ping 192.168.99.2 ===
Proves: Admin-PC reaches the switch management interface
C:\>ping 192.168.99.2

Pinging 192.168.99.2 with 32 bytes of data:

Reply from 192.168.99.2: bytes=32 time<1ms TTL=255
Reply from 192.168.99.2: bytes=32 time<1ms TTL=255
Reply from 192.168.99.2: bytes=32 time<1ms TTL=255
Reply from 192.168.99.2: bytes=32 time<1ms TTL=255

Ping statistics for 192.168.99.2:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
    Minimum = 0ms, Maximum = 0ms, Average = 0ms


=== Admin-PC: ssh -l admin 192.168.99.2 ===
Proves: SSH to HQ-SW1 works (banner + login)
C:\>ssh -l admin 192.168.99.2

Password: 

 Authorized access only. Activity is monitored. 

HQ-SW1>exit

[Connection to 192.168.99.2 closed by foreign host]
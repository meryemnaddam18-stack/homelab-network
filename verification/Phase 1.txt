--------------------------------------------------------------------------- 
                                  HQ-R1
---------------------------------------------------------------------------
HQ-R1#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES NVRAM  administratively down down 
GigabitEthernet0/0/1   10.0.0.1        YES manual up                    up 
GigabitEthernet0/0/2   203.0.113.2     YES manual up                    up
HQ-R1#show ip ssh 
SSH Enabled - version 2.0
Authentication timeout: 120 secs; Authentication retries: 3
HQ-R1#
HQ-R1#ping 10.0.0.2

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 10.0.0.2, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms

HQ-R1#ping 203.0.113.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 203.0.113.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 0/0/0 ms

HQ-R1#ssh -l admin 10.0.0.2

Password: 

Authorized access only. Activity is monitored. 

BR-R1#
BR-R1#
--------------------------------------------------------------------------
                                  ISP 
-------------------------------------------------------------------------
ISP#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   203.0.113.1     YES manual up                    up 
GigabitEthernet0/0/1   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/2   unassigned      YES unset  administratively down down 
Loopback0              8.8.8.8         YES manual up                    up

------------------------------------------------------------------------------
                                BR-R1
--------------------------------------------------------------------------------
BR-R1#show ip interface brief
Interface              IP-Address      OK? Method Status                Protocol 
GigabitEthernet0/0/0   unassigned      YES unset  administratively down down 
GigabitEthernet0/0/1   10.0.0.2        YES manual up                    up 
GigabitEthernet0/0/2   unassigned      YES unset  administratively down down
BR-R1#show ip ssh
SSH Enabled - version 2.0
Authentication timeout: 120 secs; Authentication retries: 3
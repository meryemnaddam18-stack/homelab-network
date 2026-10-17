=== HQ-R1: show ip dhcp pool ===
Proves: STAFF and GUEST pools exist with the correct networks and gateways


HQ-R1#show ip dhcp pool

Pool STAFF :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0 
 Total addresses                : 254
 Leased addresses               : 2
 Excluded addresses             : 2
 Pending event                  : none

 1 subnet is currently in the pool
 Current index        IP address range                    Leased/Excluded/Total
 192.168.10.1         192.168.10.1     - 192.168.10.254    2    / 2     / 254

Pool GUEST :
 Utilization mark (high/low)    : 100 / 0
 Subnet size (first/next)       : 0 / 0 
 Total addresses                : 254
 Leased addresses               : 1
 Excluded addresses             : 2
 Pending event                  : none

 1 subnet is currently in the pool
 Current index        IP address range                    Leased/Excluded/Total
 192.168.20.1         192.168.20.1     - 192.168.20.254    1    / 2     / 254
HQ-R1#


=== HQ-R1: show ip dhcp binding ===
Proves: HQ PCs received leases from the router (addresses start at .11)


HQ-R1#show ip dhcp binding
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
192.168.10.11    0050.0FC5.0303           --                     Automatic
192.168.10.12    0000.0CB2.C0AE           --                     Automatic
192.168.20.11    0060.2F4E.179D           --                     Automatic
HQ-R1#


=== BR-R1: show ip dhcp binding ===
Proves: Branch-PC1 received a lease from the BRANCH pool


BR-R1#show ip dhcp binding
IP address       Client-ID/              Lease expiration        Type
                 Hardware address
192.168.30.11    0000.0C44.2438           --                     Automatic
BR-R1#


=== Staff-PC1: ipconfig /all ===
Proves: Staff PC got 192.168.10.x, mask /24, gateway 192.168.10.1 via DHCP


C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: homelab.local
   Physical Address................: 0000.0CB2.C0AE
   Link-local IPv6 Address.........: FE80::200:CFF:FEB2:C0AE
   IPv6 Address....................: ::
   IPv4 Address....................: 192.168.10.12
   Subnet Mask.....................: 255.255.255.0
   Default Gateway.................: ::
                                     192.168.10.1
   DHCP Servers....................: 192.168.10.1
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-71-96-01-4D-00-00-0C-B2-C0-AE
   DNS Servers.....................: ::
                                     8.8.8.8  


=== Guest-PC1: ipconfig /all ===
Proves: Guest PC got 192.168.20.x, gateway 192.168.20.1 via DHCP

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Physical Address................: 0060.2F4E.179D
   Link-local IPv6 Address.........: FE80::260:2FFF:FE4E:179D
   IPv6 Address....................: ::
   IPv4 Address....................: 192.168.20.11
   Subnet Mask.....................: 255.255.255.0
   Default Gateway.................: ::
                                     192.168.20.1
   DHCP Servers....................: 192.168.20.1
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-24-3C-4A-DD-00-60-2F-4E-17-9D
   DNS Servers.....................: ::
                                     8.8.8.8

Bluetooth Connection:

   Connection-specific DNS Suffix..: 
   Physical Address................: 0030.F27B.B4D2
   Link-local IPv6 Address.........: ::

=== Branch-PC1: ipconfig /all ===
Proves: Branch PC got 192.168.30.x, gateway 192.168.30.1 via DHCP

C:\>ipconfig /all

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: homelab.local
   Physical Address................: 0000.0C44.2438
   Link-local IPv6 Address.........: FE80::200:CFF:FE44:2438
   IPv6 Address....................: ::
   IPv4 Address....................: 192.168.30.11
   Subnet Mask.....................: 255.255.255.0
   Default Gateway.................: ::
                                     192.168.30.1
   DHCP Servers....................: 192.168.30.1
   DHCPv6 IAID.....................: 
   DHCPv6 Client DUID..............: 00-01-00-01-C3-4E-86-B5-00-00-0C-44-24-38
   DNS Servers.....................: ::
                                     8.8.8.8

=== Admin-PC: ipconfig ===
Proves: Admin-PC keeps its static address 192.168.99.10 (no DHCP on VLAN 99)


C:\>ipconfig

FastEthernet0 Connection:(default port)

   Connection-specific DNS Suffix..: 
   Link-local IPv6 Address.........: FE80::201:43FF:FE6E:E6C5
   IPv6 Address....................: ::
   IPv4 Address....................: 192.168.99.10
   Subnet Mask.....................: 255.255.255.0
   Default Gateway.................: ::
                                     192.168.99.1
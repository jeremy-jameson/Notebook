# IPv6 Issue

Thursday, October 02, 2014
4:32 AM

![(screenshot)](https://assets.technologytoolbox.com/screenshots/69/951F0F7F6D1537D30E81A7267F68ADFBA9A98869.png)

Also noticed very high latency when opening PowerShell prompt

```Text
PS C:\Users\jjameson> ping cyclops
Ping request could not find host cyclops. Please check the name and try again.
PS C:\Users\jjameson> nslookup
Default Server:  cdns02.comcast.net
Address:  2001:558:feed::2

> cyclops
Server:  cdns02.comcast.net
Address:  2001:558:feed::2

*** cdns02.comcast.net can't find cyclops: Non-existent domain
> cyclops.corp.technologytoolbox.com
Server:  cdns02.comcast.net
Address:  2001:558:feed::2

*** cdns02.comcast.net can't find cyclops.corp.technologytoolbox.com: Non-existent domain
> exit
PS C:\Users\jjameson> ipconfig /all

Windows IP Configuration

   Host Name . . . . . . . . . . . . : WOLVERINE
   Primary Dns Suffix  . . . . . . . : corp.technologytoolbox.com
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No
   DNS Suffix Search List. . . . . . : corp.technologytoolbox.com

Ethernet adapter LAN 1 - 192.168.10.x:

   Connection-specific DNS Suffix  . : corp.technologytoolbox.com
   Description . . . . . . . . . . . : Intel(R) 82579V Gigabit Network Connection
   Physical Address. . . . . . . . . : C8-60-00-08-D4-0E
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   IPv6 Address. . . . . . . . . . . : 2601:1:8200:6000:c919:bec6:d007:3ac8(Preferred)
   Temporary IPv6 Address. . . . . . : 2601:1:8200:6000:4000:fdd0:cfb:6dd2(Preferred)
   Link-local IPv6 Address . . . . . : fe80::c919:bec6:d007:3ac8%16(Preferred)
   IPv4 Address. . . . . . . . . . . : 192.168.10.2(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Lease Obtained. . . . . . . . . . : Thursday, October 02, 2014 4:35:48 AM
   Lease Expires . . . . . . . . . . : Friday, October 10, 2014 4:35:48 AM
   Default Gateway . . . . . . . . . : fe80::250:f1ff:fe80:0%16
                                       192.168.10.1
   DHCP Server . . . . . . . . . . . : 192.168.10.106
   DHCPv6 IAID . . . . . . . . . . . : 365453312
   DHCPv6 Client DUID. . . . . . . . : 00-01-00-01-1B-8D-4C-A6-C8-60-00-08-D3-77
   DNS Servers . . . . . . . . . . . : 2001:558:feed::2
                                       2001:558:feed::1
                                       192.168.10.103
                                       192.168.10.104
   NetBIOS over Tcpip. . . . . . . . : Enabled

Ethernet adapter LAN 2 - 192.168.10.x:

   Media State . . . . . . . . . . . : Media disconnected
   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Realtek PCIe GBE Family Controller
   Physical Address. . . . . . . . . : C8-60-00-08-D3-77
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
```

1. Set IPv6 address for XAVIER1 to private IPv6 address (2601:1:8200:6000::103)
2. Changed Comcast router setting to specify Primary DNS:
3. Added reverse lookup zone on XAVIER1:Type of zone: **Primary zone**Store the zone in Active Directory: **Yes (checked)**Replication scope: **To all DNS servers running on domain controllers in this domain: corp.technologytoolbox.com**Reverse lookup zone: **IPv6 Reverse Lookup Zone**IPv6 Address Prefix: **2601:1:8200:6000::/64**
4. Configured static IPv6 address on XAVIER2 and changed Comcast router setting to specify Secondary DNS:

![(screenshot)](https://assets.technologytoolbox.com/screenshots/6C/740DA8AEEAD5939502F1A3380B1EDEB0A235F46C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/66/BF8DCE9E771B922D958A2CDEE91CC36F97F7C666.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/8C/BA2AA3469EF11569DDDC045C7DB156219306C28C.png)

PowerShell prompt still very slow when starting. Using Process Monitor and Message Analyzer, I discovered this was due to WPAD queries.

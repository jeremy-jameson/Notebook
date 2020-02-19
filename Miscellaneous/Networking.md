# Networking

Thursday, January 09, 2014
10:11 AM

## DHCP/DNS issues

![(screenshot)](https://assets.technologytoolbox.com/screenshots/BB/43A52D405691A445629028928AA6881457E03BBB.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/54/9645A72BB414023CB9B876EEAD86B40743D02554.png)

```Text
C:\Windows\system32>ipconfig /all

Windows IP Configuration

   Host Name . . . . . . . . . . . . : wolverine
   Primary Dns Suffix  . . . . . . . : corp.technologytoolbox.com
   Node Type . . . . . . . . . . . . : Hybrid
   IP Routing Enabled. . . . . . . . : No
   WINS Proxy Enabled. . . . . . . . : No
   DNS Suffix Search List. . . . . . : corp.technologytoolbox.com
                                       domain.actdsltmp
...
Ethernet adapter Local Area Connection:

   Connection-specific DNS Suffix  . : domain.actdsltmp
   Description . . . . . . . . . . . : Intel(R) 82579V Gigabit Network Connection
   Physical Address. . . . . . . . . : C8-60-00-08-D4-0E
   DHCP Enabled. . . . . . . . . . . : Yes
   Autoconfiguration Enabled . . . . : Yes
   IPv4 Address. . . . . . . . . . . : 192.168.10.4(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Lease Obtained. . . . . . . . . . : Sunday, January 5, 2014 3:34:37 AM
   Lease Expires . . . . . . . . . . : Monday, January 6, 2014 3:34:37 AM
   Default Gateway . . . . . . . . . : 192.168.10.1
   DHCP Server . . . . . . . . . . . : 192.168.10.1
   DNS Servers . . . . . . . . . . . : 192.168.10.1
                                       192.168.10.104
   NetBIOS over Tcpip. . . . . . . . : Enabled
```

## Configure static IP address

```PowerShell
$ipAddress = "192.168.10.106"

New-NetIPAddress `
    -InterfaceAlias "LAN 1 - 192.168.10.x" `
    -IPAddress $ipAddress `
    -PrefixLength 24 `
    -DefaultGateway 192.168.10.1

Set-DNSClientServerAddress `
    -InterfaceAlias "LAN 1 - 192.168.10.x" `
    -ServerAddresses 192.168.10.103,192.168.10.104
```

## Add DHCP feature

```PowerShell
Install-WindowsFeature DHCP -IncludeManagementTools

Set-DhcpServerv4OptionValue `
    -DNSServer 192.168.10.103,192.168.10.104 `
    -DNSDomain corp.technologytoolbox.com `
    -Router 192.168.10.1

Add-DhcpServerSecurityGroup

Add-DhcpServerv4Scope `
    -Name "Default Scope"`
    -StartRange 192.168.10.2 `
    -EndRange 192.168.10.100`
    -SubnetMask 255.255.255.0

Add-DhcpServerInDC -DNSName corp.technologytoolbox.com
```

```Text
WARNING: ...Failed to initiate the authorization check on the DHCP server. Error: There are no more endpoints available from the endpoint mapper.  (1753).
```

**Workaround: Authorize the DHCP server from DHCP Manager on FOOBAR8.**

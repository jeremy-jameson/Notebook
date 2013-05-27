# iSCSI

Sunday, May 26, 2013
11:27 AM

**Introduction of iSCSI Target in Windows Server 2012**\
Pasted from <[http://blogs.technet.com/b/filecab/archive/2012/05/21/introduction-of-iscsi-target-in-windows-server-2012.aspx](http://blogs.technet.com/b/filecab/archive/2012/05/21/introduction-of-iscsi-target-in-windows-server-2012.aspx)>

**Configuring iSCSI Storage (Part 1)**\
Pasted from <[http://www.windowsnetworking.com/articles-tutorials/windows-server-2012/configuring-iscsi-storage-part1.html](http://www.windowsnetworking.com/articles-tutorials/windows-server-2012/configuring-iscsi-storage-part1.html)>

**DIY SAN: Windows Server 2012 Storage Spaces and iSCSI target**\
Pasted from <[http://www.techrepublic.com/blog/networking/diy-san-windows-server-2012-storage-spaces-and-iscsi-target/6293](http://www.techrepublic.com/blog/networking/diy-san-windows-server-2012-storage-spaces-and-iscsi-target/6293)>

**Step-by-Step: Speaking iSCSI with Windows Server 2012 and Hyper-V - Become a Virtualization Expert in 20 Days! ( Part 7 of 20 )**\
Pasted from <[http://blogs.technet.com/b/keithmayer/archive/2013/03/12/speaking-iscsi-with-windows-server-2012-and-hyper-v.aspx#.UaIAapyTmwE](http://blogs.technet.com/b/keithmayer/archive/2013/03/12/speaking-iscsi-with-windows-server-2012-and-hyper-v.aspx#.UaIAapyTmwE)>

**Virtualization: Network card settings for Hyper V Cluster**\
Pasted from <[http://blogs.technet.com/b/schadinio/archive/2010/11/06/virtualization-network-card-settings-for-hyper-v-cluster.aspx](http://blogs.technet.com/b/schadinio/archive/2010/11/06/virtualization-network-card-settings-for-hyper-v-cluster.aspx)>

**Getting "Authentication failure" error when adding an iSCSI target with CHAP in Windows 2008R2 Server**\
Pasted from <[http://social.technet.microsoft.com/Forums/windowsserver/en-US/733ec856-14ca-4a29-995c-4557409906c2/getting-authentication-failure-error-when-adding-an-iscsi-target-with-chap-in-windows-2008r2?forum=winserverfiles](http://social.technet.microsoft.com/Forums/windowsserver/en-US/733ec856-14ca-4a29-995c-4557409906c2/getting-authentication-failure-error-when-adding-an-iscsi-target-with-chap-in-windows-2008r2?forum=winserverfiles)>

![(screenshot)](https://assets.technologytoolbox.com/screenshots/20/F99F4ABB3E6E77D0E4FE57E1111087EC4D656320.png)

## Configure static IP address

```PowerShell
$ipAddress = "192.168.10.209"

New-NetIPAddress `
    -InterfaceAlias "LAN 1 - 192.168.10.x" `
    -IPAddress $ipAddress `
    -PrefixLength 24 `
    -DefaultGateway 192.168.10.1

Set-DNSClientServerAddress `
    -InterfaceAlias "LAN 1 - 192.168.10.x" `
    -ServerAddresses 192.168.10.210
```

## Add network adapter (Virtual iSCSI 1 - 10.1.10.x)

```PowerShell
$vmName = "EXT-DC01"
Stop-VM $vmName

Add-VMNetworkAdapter `
    -VMName $vmName `
    -SwitchName "Virtual iSCSI 1 - 10.1.10.x"

Start-VM $vmName
```

## Rename iSCSI network connection

```PowerShell
Get-NetAdapter -Physical

Get-NetAdapter -InterfaceDescription "Microsoft Hyper-V Network Adapter #2" |
    Rename-NetAdapter -NewName "iSCSI 1 - 10.1.10.x"

Get-NetAdapter -Physical
```

## Configure iSCSI network adapter

```PowerShell
$ipAddress = "10.1.10.209"

New-NetIPAddress -InterfaceAlias "iSCSI 1 - 10.1.10.x" -IPAddress $ipAddress `
    -PrefixLength 24

Disable-NetAdapterBinding -Name "iSCSI 1 - 10.1.10.x" `
    -DisplayName "Client for Microsoft Networks"

Disable-NetAdapterBinding -Name "iSCSI 1 - 10.1.10.x" `
    -DisplayName "File and Printer Sharing for Microsoft Networks"

Disable-NetAdapterBinding -Name "iSCSI 1 - 10.1.10.x" `
    -DisplayName "Link-Layer Topology Discovery Mapper I/O Driver"

Disable-NetAdapterBinding -Name "iSCSI 1 - 10.1.10.x" `
    -DisplayName "Link-Layer Topology Discovery Responder"

$adapter = Get-WmiObject -Class "Win32_NetworkAdapter" `
    -Filter "NetConnectionId = 'iSCSI 1 - 10.1.10.x'"

$adapterConfig = Get-WmiObject -Class "Win32_NetworkAdapterConfiguration" `
    -Filter "Index= '$($adapter.DeviceID)'"

# Do not register this connection in DNS
$adapterConfig.SetDynamicDNSRegistration($false)

# Disable NetBIOS over TCP/IP
$adapterConfig.SetTcpipNetbios(2)
```

## Enable jumbo frames

```PowerShell
Get-NetAdapterAdvancedProperty -DisplayName "Jumbo*"

Set-NetAdapterAdvancedProperty -Name "iSCSI 1 - 10.1.10.x" `
    -DisplayName "Jumbo Packet" -RegistryValue 9014

ping 10.1.10.106 -f -l 8900
```

## Enable iSCSI Initiator

Start -> iSCSI Initiator

![(screenshot)](https://assets.technologytoolbox.com/screenshots/EB/4A53DA5A4340A8ABE5F2AF9FFAADA619B36BA5EB.png)

Click Yes.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/9A/B54534041BE9EBB7BFFD0E55049BF636C6FC889A.png)

## Create iSCSI virtual disk (XAVIER1-Backup01.vhdx)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/1B/777148954AF6A60422F00404E1E136A546AB501B.png)

## Discover iSCSI Target portal

![(screenshot)](https://assets.technologytoolbox.com/screenshots/72/8313A23CC5FB9E97557F1E272594355FF3E6B572.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/2A/6213B3CD19D36E47382BF220A9022F68F9BB042A.png)

Try using PowerShell instead

```PowerShell
$chapUserName = "iqn.1991-05.com.microsoft:xavier1.corp.technologytoolbox.com"

New-IscsiTargetPortal -TargetPortalAddress 10.1.10.106 `
    –AuthenticationType OneWayCHAP `
    –ChapUserName $chapUserName -ChapSecret {password}

Get-iScsiTarget | Connect-iScsitarget –AuthenticationType OneWayCHAP `
    –ChapUserName $chapUserName -ChapSecret {password}
```

Connect-iScsitarget : Authentication Failure.\
At line:1 char:19\
+ Get-iScsiTarget | Connect-iScsitarget –AuthenticationType OneWayCHAP –ChapUserNa ...\
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~\
    + CategoryInfo          : NotSpecified: (MSFT_iSCSITarget:ROOT/Microsoft/...SFT_iSCSITarget) [Connect-IscsiTarget]\
   , CimException\
    + FullyQualifiedErrorId : HRESULT 0xefff0009,Connect-IscsiTarget

![(screenshot)](https://assets.technologytoolbox.com/screenshots/FC/7798425A70F41A427A520AFCDB8F049B70374AFC.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/5B/413419677D759BB760ED3DAF0AA09FB2DEC5EB5B.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/81/F685A7642CD3846C0B3355A402323E3DBA073981.png)

On the **Volumes and Devices** tab, click **Auto Configure**.

## Online the iSCSI LUN

```PowerShell
Get-Disk | Set-Disk -IsOffline $false
```

The PowerShell above doesn't work (the disk still appears offline in Disk Management).

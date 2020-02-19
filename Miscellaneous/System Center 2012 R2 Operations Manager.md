# System Center 2012 R2 Operations Manager

Wednesday, December 18, 2013
9:49 PM

**OpsMgr 2012 R2 - QuickStart Deployment Guide**\
Pasted from <[http://blogs.technet.com/b/kevinholman/archive/2013/10/18/opsmgr-2012-r2-quickstart-deployment-guide.aspx](http://blogs.technet.com/b/kevinholman/archive/2013/10/18/opsmgr-2012-r2-quickstart-deployment-guide.aspx)>

**About Gateway Servers in Operations Manager**\
Pasted from <[http://technet.microsoft.com/en-us/library/hh212823.aspx](http://technet.microsoft.com/en-us/library/hh212823.aspx)>

**Deploying a Gateway Server**\
Pasted from <[http://technet.microsoft.com/library/hh456447.aspx](http://technet.microsoft.com/library/hh456447.aspx)>

```Text
12345678901234567890123456789012345678901234567890123456789012345678901234567890
```

## # Create virtual machine

```PowerShell
$vmName = "JUBILEE"

New-VM `
    -Name $vmName `
    -Path C:\NotBackedUp\VMs `
    -MemoryStartupBytes 512MB `
    -SwitchName "Virtual LAN 2 - 192.168.10.x"

Set-VMProcessor -VMName $vmName -Count 2

Set-VMMemory `
    -VMName $vmName `
    -DynamicMemoryEnabled $true `
    -MaximumBytes 4GB

$sysPrepedImage =
    "\\ICEMAN\VM Library\ws2012std-r2\Virtual Hard Disks\ws2012std-r2.vhd"

$vhdPath = "C:\NotBackedUp\VMs\$vmName\Virtual Hard Disks\$vmName.vhdx"

Convert-VHD `
    -Path $sysPrepedImage `
    -DestinationPath $vhdPath

Set-VHD $vhdPath -PhysicalSectorSizeBytes 4096

Add-VMHardDiskDrive -VMName $vmName -Path $vhdPath

Start-VM $vmName
```

## # Rename the server and join domain

```PowerShell
Rename-Computer -NewName JUBILEE -Restart

Add-Computer -DomainName corp.technologytoolbox.com -Restart
```

## # Download PowerShell help files

```PowerShell
Update-Help
```

## # Change drive letter for DVD-ROM

### # To change the drive letter for the DVD-ROM using PowerShell

```PowerShell
$cdrom = Get-WmiObject -Class Win32_CDROMDrive
$driveLetter = $cdrom.Drive

$volumeId = mountvol $driveLetter /L
$volumeId = $volumeId.Trim()

mountvol $driveLetter /D

mountvol X: $volumeId
```

## # Rename network connection

```PowerShell
Get-NetAdapter -Physical

Get-NetAdapter -InterfaceDescription "Microsoft Hyper-V Network Adapter" |
    Rename-NetAdapter -NewName "LAN 1 - 192.168.10.x"
```

## # Enable jumbo frames

```PowerShell
Get-NetAdapterAdvancedProperty -DisplayName "Jumbo*"

Set-NetAdapterAdvancedProperty -Name "LAN 1 - 192.168.10.x" `
    -DisplayName "Jumbo Packet" -RegistryValue 9014

ping ICEMAN -f -l 8900
```

## Add SCOM Administrators domain group to local Administrators group

![(screenshot)](https://assets.technologytoolbox.com/screenshots/53/EDBDB63FF80D95D5C1548DE9160B73FAACA8A353.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/DE/8729C5E131507800E8752F4B4A0D7889DD0E50DE.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/BC/3E3A321AB34D82D3A81032A12DC1CCA2FFEF15BC.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/6E/E79D4BFAFBA0605F6EA004EE1E51A711B3CD8F6E.png)

## # Install SCOM prerequisites

### # Install .NET Framework 3.5

```PowerShell
Install-WindowsFeature `
    NET-Framework-Core `
    -Source '\\ICEMAN\Products\Microsoft\Windows Server 2012 R2\Sources\SxS'
```

### # Install SQL Server Reporting Services

**# Note: .NET Framework 3.5 is required for SQL Server Reporting Services.**

### Reference

**Set up SQL Server for TFS**\
Pasted from <[http://msdn.microsoft.com/en-us/library/jj620927.aspx](http://msdn.microsoft.com/en-us/library/jj620927.aspx)>

SQL Server 2012 with SP1

On the **Feature Selection** step, select **Reporting Services - Native**.

On the **Server Configuration** step, on the **Service Accounts** tab:

- In the **Account Name** column for **SQL Server Reporting Services**, type **NT AUTHORITY\\NETWORK SERVICE**.
- In the **Startup Type** column for **SQL Server Reporting Services**, ensure **Automatic** is selected.

### # Configure SQL Server Reporting Services

Start -> **Reporting Services Configuration Manager**

UAC prompt -> click **Yes**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/63/C2F498B3B5488A45F3B15037A1E0C6DEF82FCC63.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/EA/BD7FBFFE69955DDCF3F181049DBD386572EC4DEA.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/ED/FC9622FFF8CC43D556ACEA4A81F3A54396CFADED.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/BB/CA46C3588ED1434C8CECB094D4505399182501BB.png)

Click **Apply**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/7A/1E864C8A629B26B03941F97818896B22A1CFCE7A.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/21/5EAFB353770D70F55FCD0AD743D2E8256FBF2C21.png)

Click **Change Database**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/31/FC6DDDF048219D296329BF85D89CAEDE9D092431.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E5/E86C4A2C54F06FFDB341374F3006ADCA9C3030E5.png)

Click **Test Connection**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/D6/828060F714EA57F346651546479038919F347DD6.png)

Click **OK** and then click **Next**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/41/E54F3723D245C24AE4533191E2D737EC08DA2D41.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/AE/DBC188815383042DF7EE3C0512B3418EAFA063AE.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/86/7A59884BC08E81125111E9FDF1DE31C25FE1A086.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/DC/7E59AC049C8D8D30D394C12558584787FE8088DC.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/96/EF3E9578BA9A3B2181F0777E06AF301033A6F296.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E8/A3EAFD65F2C45893DF7C311C5C46A7B89BB1BCE8.png)

[http://social.msdn.microsoft.com/Forums/sqlserver/en-US/c3b77818-fe8f-49f4-a094-2f53cd94ab13/installing-secondary-srs-instance-issue](http://social.msdn.microsoft.com/Forums/sqlserver/en-US/c3b77818-fe8f-49f4-a094-2f53cd94ab13/installing-secondary-srs-instance-issue)

Change service account to **Network Service**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F7/DB128B6341118AE62DBB1DA5FFF268D1A0E2F4F7.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/7E/0F4EFA5EEAF0A7519D84E99DAC95B0A4E727627E.png)

Reserving url [http://+:80](http://+:80)\
The url was successfully reserved.

Removing url [http://+:80](http://+:80)\
The url was successfully removed.

Starting report server "MSSQLSERVER" on JUBILEE.\
The task completed successfully.

Setting Windows Service Identity to Built-In Account\
The Windows service is now configured to use a built-in windows account.

Stopping report server "MSSQLSERVER" on JUBILEE.\
The task completed successfully.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F3/9C21D5E78F4DF5E6D602E3DBC2B0EDE9E2A33AF3.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/59/B54B51F234A6E12811D933BA89046CCB013E0359.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/41/E54F3723D245C24AE4533191E2D737EC08DA2D41.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/2E/6107ABB69F96713DB5B3111820EE89FFD252142E.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/79/8D85B42B9BD305A10654303FFE9D9D1E0752B779.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/53/DE268843A3F1EC2F0A6C8B33001B85BDB3AD2253.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/D6/0C290C6E1093352B2AECEEF605D4574071C9A2D6.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/18/69FC0BAF2980D89EBA46EEEB11575D37ED920418.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/11/C32911D99B9CD058702043EF81CAFEB18B26FD11.png)

Click **Apply**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/38/A63DC71C7421B7CB95186E3BFC57C9762F3A4438.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/54/612FC8CF8184A963E4E2EA163EB60E6F3775C654.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/5B/8E8BFEBC8CA501E4FF087C23BFC3E6DC9061A35B.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F0/D95BF228D0C0F1D80D5B43244B72A14A7E1688F0.png)

Click **Backup**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/16/3A5DFE80CC45E0FBC642E3897DDE99D3400FA916.png)

[\\\\iceman\\Users\$\\jjameson-admin\\Documents\\Reporting Services - JUBILEE.snk](\\iceman\Users\$\jjameson-admin\Documents\Reporting Services - JUBILEE.snk)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/D8/B62490B5FECF0C7E4D9FDF0A0511A45F606E89D8.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/88/1194F5DD84CB53529265A74E9096A855DD179B88.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/8E/795FA656973D3B84FB1438236261788785AF078E.png)

Click **Exit**.

### # Install IIS

```PowerShell
Install-WindowsFeature `
    NET-WCF-HTTP-Activation45, `
    Web-Static-Content, `
    Web-Default-Doc, `
    Web-Dir-Browsing, `
    Web-Http-Errors, `
    Web-Http-Logging, `
    Web-Request-Monitor, `
    Web-Filtering, `
    Web-Stat-Compression, `
    Web-Mgmt-Console, `
    Web-Metabase, `
    Web-Asp-Net, `
    Web-Windows-Auth `
    -Source '\\ICEMAN\Products\Microsoft\Windows Server 2012 R2\Sources\SxS' `
    -Restart
```

## Create IIS Website for System Center

Using DNS Manager, create a new alias (CNAME record):

![(screenshot)](https://assets.technologytoolbox.com/screenshots/68/8FBD746FF190C41613B03EE6826A1ADEB6E61168.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/D9/7D1658A4C2D1D6CC73D9C37E26E0BC51158218D9.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/7B/6A8E8851FB501840F2C15C73066DE3D3D89B0A7B.png)

```Console
mkdir C:\inetpub\wwwroot\SystemCenter
```

![(screenshot)](https://assets.technologytoolbox.com/screenshots/B6/9120D04F8AF1562877204935DB6DAA64B2AB1FB6.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/57/B897FBB030C747EE57509B566F39BD25CC542757.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/B2/94872097D3B3C0F76EA6D4451902D58764C958B2.png)

Click **No**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/CD/BC0B0D2896C2FE7738289B408E3BBB9B51A2E3CD.png)

Right-click **Sites** and then click **Add Website...**

![(screenshot)](https://assets.technologytoolbox.com/screenshots/FF/49B7A8AA613933C965FFFB2E0EC063A986D239FF.png)

### Install Microsoft System CLR Types for SQL Server 2012

## "\\\\iceman\\Products\\Microsoft\\System Center 2012 R2\\Microsoft System CLR Types for SQL Server 2012\\SQLSysClrTypes.msi"

### Install Microsoft Report Viewer Runtime

"[\\\\iceman\\Products\\Microsoft\\System Center 2012 R2\\Microsoft Report Viewer Runtime\\ReportViewer.msi](\\iceman\Products\Microsoft\System Center 2012 R2\Microsoft Report Viewer Runtime\ReportViewer.msi)"

## Install Operations Manager

![(screenshot)](https://assets.technologytoolbox.com/screenshots/06/37E3980F82E21B260ABBA2EA5CC5232317A21C06.png)

Select **Download the latest updates to the setup program** and then click **Install**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/3B/0D64CEC89947B224EFFB7A4C1E1534EE19669A3B.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/70/29E74E615134A7149BA55A753F7231DA754B8170.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/FB/280631F22B118ABE9EA449D74EBD600795A54BFB.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F4/F7482DA8797247106FB4B7562A03B35E1775A9F4.png)

```PowerShell
$vmName = "JUBILEE"

Stop-VM $vmName

Set-VMMemory `
    -VMName $vmName `
    -DynamicMemoryEnabled $true `
    -MinimumBytes 2GB `
    -MaximumBytes 4GB `
    -StartupBytes 2GB

Start-VM $vmName
```

Same warning...

![(screenshot)](https://assets.technologytoolbox.com/screenshots/EA/6B3BBD97A8DE48D64AE5FCD6C53DA172F08179EA.png)

Click **Next**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/7A/84CBF49FE524E539D7A16C968076D7E84D99FE7A.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/57/09444DEA7BAA842EEC7E79802D4AE70086CB6257.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/56/C0589C026CEA4BD0773A70E960F5EF3994C1ED56.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/AD/A57132621078BBE6D7777D10B8C550F407B9D8AD.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/8A/BF536A1216473E26DC633B0B89FE70B459DA5E8A.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/EA/7D0BDFE63112ED67F3B5D0733F009947C52E10EA.png)

The installed version of SQL Server could not be verified or is not supported. Verify that the computer and the installed version of SQL Server meet the minimum requirements for installation, and that the firewall settings are correct. See the Supported Configurations document for further information.

Enabled firewall logging on HAVOK:

#Fields: date time action protocol src-ip dst-ip src-port dst-port size tcpflags tcpsyn tcpack tcpwin icmptype icmpcode info path\
...\
2014-01-07 13:04:10 DROP TCP 192.168.10.18 192.168.10.13 59576 135 52 S 4037609328 0 8192 - - - RECEIVE\
...\
2014-01-07 13:04:31 DROP TCP 192.168.10.18 192.168.10.13 59577 445 52 S 1121136589 0 8192 - - - RECEIVE\
...\
2014-01-07 13:04:38 DROP UDP 192.168.10.18 192.168.10.13 137 137 78 - - - - - - - RECEIVE

**SCOM 2012 - Installing Operations Manager Database on server behind a firewall**\
Pasted from <[http://social.technet.microsoft.com/Forums/systemcenter/en-US/6c3dc8ff-4f66-4c73-9c9e-4ca948cde3ff/scom-2012-installing-operations-manager-database-on-server-behind-a-firewall?forum=operationsmanagerdeployment](http://social.technet.microsoft.com/Forums/systemcenter/en-US/6c3dc8ff-4f66-4c73-9c9e-4ca948cde3ff/scom-2012-installing-operations-manager-database-on-server-behind-a-firewall?forum=operationsmanagerdeployment)>

Add "temporary" firewall rules for SCOM 2012 installation:

Name: **SCOM 2012 Installation - TCP**\
Protocol:** UDP**\
Local Port:** 135, 445, 49152-65535**\
Profile: **Domain**\
Direction: **Inbound**\
Action: **Allow**

Name: **SCOM 2012 Installation - UDP**\
Protocol:** UDP**\
Local Port:** 137**\
Profile: **Domain**\
Direction: **Inbound**\
Action: **Allow**

![(screenshot)](https://assets.technologytoolbox.com/screenshots/1E/AF80EE7EA595294686C1D35B6424199EDF1CE11E.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/7B/7A3CC1DB068B1BE12DAAB602014FC35A92745A7B.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/94/5FD3C5C7C3594B7BCF79148BA6975B232743D994.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/EA/71B1EDC1F91D709A6032DB6988C5AFE5456EB0EA.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/14/B8BCDD8946EB62F2D4B73B91728A6253F8C20414.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/DC/9F3D1CFA49E5CE33114AC56312C4C50D6C7AA8DC.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/8C/5D06F0E8094EA01126252D515F76FC5F5B28E18C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/FF/7E47881C2E96CD9774BC85984E7E436C02E360FF.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/22/DD519A5F991933B895B28B8B3EEE7F5DBAEB3C22.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/DA/894078AB6ED377D40A9E4559CE8324DAE53C21DA.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/5C/C5F7E9F4EAEF06B695CCDBEE690244601E7CD35C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/08/75B28D59F45DA77D9EB209A99FD259CFB3F61108.png)

## Disable temporary firewall rules on database server

```PowerShell
Disable-NetFirewallRule -DisplayName "SCOM 2012 Installation - TCP"
Disable-NetFirewallRule -DisplayName "SCOM 2012 Installation - UDP"
```

## Create SMTP Channel

![(screenshot)](https://assets.technologytoolbox.com/screenshots/BA/38B9E25E6660CD5F5F60EC28E23FCD694D75EABA.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/A3/019E525F37F25924ED5BDEFAED9337C4B07190A3.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/09/541E7DFC17496D687D0936A4316AF49FFA2C5009.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/1B/7F86DFDA7287D581B56138123702FACF62BC211B.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/81/1D8E8796D2FF93578EE3EFF05DA7F8A24D64C081.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E2/DFB3538463AF8CE1B80383AE6329A8CC76B996E2.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/22/ECCB260F0CE32376E509148ED80DE493140D2C22.png)

Click **Finish** to accept the defaults.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/AF/F61F15BEEB94F6FCDEA245B17CC7FABB48B89FAF.png)

## Create a new subscriber

![(screenshot)](https://assets.technologytoolbox.com/screenshots/C8/36A4E83F5B9808EBE18CBBEDD092D50FCE5323C8.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/2E/44F940D5CD5C7C74986FD878E4C45DCF286FA72E.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/76/486C19FEA382F5E044B1CD4C2D4657F3A0EBB076.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/9C/52750BFBF80CFF6AB8DB3D8DE294DC010162879C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/CA/FC52C7AC1621B9772DCA96D758AEFE7068B015CA.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/39/19801DDBBF87992063843EC24130D0E960605139.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/52/21B0319D3735E725C6192304D0E781BB4164B252.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/60/E05C3808A4F8B078D55C2C19161F507CB47E4C60.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/A3/BC52D5E2FD74293AF9EDEF307DC5975D5E2395A3.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/75/AFCB05358312A70AAEE893D7E1B92D1DA15AD275.png)

## Create subscription

![(screenshot)](https://assets.technologytoolbox.com/screenshots/0B/658751BA60B977CD3DC68BADC6477CAE7093430B.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E3/70750A400596BFDAF86EEDB2AD339C8482CB63E3.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/6E/060F2F9F49287E844FCE4FD981FD1E0163B74F6E.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/5C/72E51D5737E28B6A4253AC55420788223A103C5C.png)

Click **Next**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/A2/1DA51C0052725495428B9C7BAA41786EE36906A2.png)

Click **Add...**

![(screenshot)](https://assets.technologytoolbox.com/screenshots/0E/B73537AD69CF0B9AA7B406B07289B454756B4C0E.png)

Click **Search**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/72/1F95EFEBABF10D1D8F962FFE57077CAAFA410972.png)

Select the subscriber, click **Add **to move it to the **Selected subscribers** list, and then click **OK**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/C7/8EF3A8B683AD3EBEDE55D7DF56AFDE832F3D5FC7.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/C6/0C667CFC5C473A69BC57DEC4209C4F9265C196C6.png)

Click **Add...**

![(screenshot)](https://assets.technologytoolbox.com/screenshots/2E/B0521D941540E599AAC3D4679E3109965A0D412E.png)

Click **Search**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/7A/93793B9AA623507D8134C1C95904E9E33978097A.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/40/FE1B71529B27D0BDB2AA0D227D1489A984D35C40.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/53/A5C0BA7905200EA4E817683551EFB79306441C53.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/FA/C852A20D1934A1E634B3F738E233C626D89826FA.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/2F/6707E1F4E350507E849BA75B7F599B520313CE2F.png)

## Import management packs

![(screenshot)](https://assets.technologytoolbox.com/screenshots/0F/9179881C54AE5ADD73F3C5883399A0F4AB86DD0F.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/CC/230EADBEB038D883B242C4C7D63BF019FB463FCC.png)

Click **Add** and then click **Add from catalog...**

![(screenshot)](https://assets.technologytoolbox.com/screenshots/A4/8D9865AC9F0EE48E64C9920B67894CB530A802A4.png)

Click **Search**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/25/87EC6B1C4710F2A67C9BBE557904E78F30A8CE25.png)

In the **Management packs in the catalog** list, expand **Microsoft Corporation**.

- Microsoft Corporation
  - ~~Office Servers~~
    - ~~SharePoint 2010 Products~~
      - ~~Microsoft.SharePoint.Foundation.2010~~
      - ~~Microsoft.SharePoint.Server.2010~~
  - SQL Server
    - SQL Server 2012
      - SQL Server 2012 (Discovery)
      - SQL Server 2012 (Monitoring)
      - SQL Server Core Library
  - Windows Server
    - Active Directory Server 2008
      - Active Directory Server 2008 and above (Discovery)
      - Active Directory Server 2008 and above (Monitoring)
      - Active Directory Server Common Library
    - Core OS
      - Windows Server 2003 Operating System
      - Windows Server 2008 Operating System (Discovery)
      - Windows Server 2008 Operating System (Monitoring)
      - Windows Server 2012 R2 Operating System (Discovery)
      - Windows Server 2012 R2 Operating System (Monitoring)
      - Windows Server Operating System Reports
    - DHCP Server
      - Microsoft Windows Server DHCP 2012 R2
      - Microsoft Windows Server DHCP Library
    - Domain Naming Service 2012/2012R2
      - Microsoft Windows Server DNS Monitoring
    - File Services 2012 R2
      - File Services Management Pack for Windows Server 2012 R2
      - File Services Management Pack Library
      - Microsoft Windows Server iSCSI Target 2012 R2
      - Microsoft Windows Server SMB 2012 R2
    - Hyper-V 2012 R2\
      Microsoft Windows Hyper-V 2012 R2 Discovery\
      Microsoft Windows Hyper-V 2012 R2 Monitoring\
      Microsoft Windows Hyper-V 2012 Library
    - IIS 2008
      - Windows Server 2008 Internet Information Services 7
      - Windows Server Internet Information Services Library
    - IIS 2012
      - Windows Server 2012 Internet Information Services 8
      - Windows Server Internet Information Services Library
    - Windows Update Services 3.0
      - Windows Server Update Services 3.0
      - Windows Server Update Services Core Library

![(screenshot)](https://assets.technologytoolbox.com/screenshots/80/FB0EAC5FEEE3FCFAD1BA6069A548282A5BCA8A80.png)

For each item that has a dependency that is not selected, click the **Resolve** link in the **Status** column.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/24/4F5557A0C72FE1C68B1A5FE442CD505CD55B8724.png)

Repeat the previous steps the remaining warnings.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E5/164040C845789192961A9CC4449A5FEE1F378EE5.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/4B/38B20AA8EA4687C442EB78271EC75851E7EF9F4B.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/B5/9E9BCA6F0219FF7883F8CAD1AE0C05FB4796FCB5.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/90/5F6FA540FA70D9AFDACDBD4776905EB848AA2690.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/34/4825B66027B732F667683B1E977A97A118A42134.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F2/9D1929615F59AA193F7EE2721F97F25F92A302F2.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/22/3A51E67ED6C362EFC843A89A29C874B65A91EB22.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/AB/5E7D8C09EDF2B5FC6C221CDA06104452CA005CAB.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/EB/58FB48553F9BFF8EB95B7B119CDA07D667E1E7EB.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E6/70D7B5750939AB5997FBA07EA2015E22A80928E6.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/CC/230EADBEB038D883B242C4C7D63BF019FB463FCC.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F2/52C4FD5D62E9C42735CE0EA07E0A9177B3AFE9F2.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/C3/4DF720BACFD9079A943A207842044715A4113EC3.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/10/A9200A55DC3A9C5FB451B7A6ECE1D0DFC164F910.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/03/2F1FC208D4B63E76E23123F871A9234A4E74FB03.png)

## Configure security settings for manual agent installations

![(screenshot)](https://assets.technologytoolbox.com/screenshots/A4/E46318ED283C00A3BE5D3CF77CAA6FCB130AE6A4.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/9C/EF4C7D4779C2030A7A880D33F06358763386279C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/20/8474378C09FE370A1213FE8E37352E7DB032BD20.png)

## Configure domain controllers to manage

![(screenshot)](https://assets.technologytoolbox.com/screenshots/FC/973464D2EEC28C96B64FFE7A9047DCE622D6ACFC.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/B6/4CA14417C955167034364A60293EB86AF2714FB6.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F6/8E6EA9FCC6E53928FC7A681AB97BB6D33A6831F6.png)

Select **Automatic computer discovery** and then click **Next**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/6B/A8A51FA2911CAB8203F7A95AA4E2D287E701256B.png)

Click **Discover**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/9C/A8559BC5A8119463C1B872D908D470626FE3849C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/D2/4D300DBE9CC08D721A21672650645BEDAAD7DDD2.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/9A/FF5BBBC09A93ACEC75E0D4A7476038EAFFC2869A.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/AF/5A9E3C9F7520A452DD141E27609FDF2A9DD18EAF.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/38/898914E94C5516499B32A937028BB8237F87A338.png)

Click **Finish**.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/0C/0164EBE2C30C3096B821D6D9820D039B60E7290C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/A4/5760456CFABAEB6421A7FC372F78ACBF1EFD7CA4.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/02/A71E4F54079FD0B2DB85A4FC0CB78F761EB46C02.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/F5/73A4F65DD992BCF8CA070E4938A0FDFED809B2F5.png)

In the **Agent Properties** window, on the **Security** tab, select **Allow this agent to act as a proxy and discover managed objects on other computers** and then click **OK**.

## Configure other servers to manage

![(screenshot)](https://assets.technologytoolbox.com/screenshots/67/D4BFF498629B584D761200225B2B7429063F9867.png)

Click **Browse...**

![(screenshot)](https://assets.technologytoolbox.com/screenshots/45/F90A4C3D2ECB973475346F3389C8E552106E0B45.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/2E/29CB97F712347BE3927ACE08FF3F3E4D6AC8D42E.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/CB/19E423F825A68323A49F1CC6A57B589E458189CB.png)

## Create rules to generate SCOM alerts for event log errors

**Operations Manager Alerts for Event Log Errors**\
Pasted from <[http://www.technologytoolbox.com/blog/jjameson/archive/2011/03/18/operations-manager-alerts-for-event-log-errors.aspx](http://www.technologytoolbox.com/blog/jjameson/archive/2011/03/18/operations-manager-alerts-for-event-log-errors.aspx)>

## Install SCOM agent

### Reference

**Install Agent Using the Command Line**\
Pasted from <[http://technet.microsoft.com/en-us/library/hh230736.aspx](http://technet.microsoft.com/en-us/library/hh230736.aspx)>

```PowerShell
$imagePath = '\\iceman\Products\Microsoft\System Center 2012 R2' `
    + '\en_system_center_2012_r2_operations_manager_x86_and_x64_dvd_2920299.iso'

$imageDriveLetter = (Mount-DiskImage -ImagePath $imagePath -PassThru |
    Get-Volume).DriveLetter

$msiPath = $imageDriveLetter + ':\agent\AMD64\MOMAgent.msi'
```

**Note:** Attempting to perform a "silent install" did not work (the Windows Installer processes appeared to hang):

```PowerShell
msiexec.exe /i $msiPath /qn+ `
    AcceptEndUserLicenseAgreement=1 `
    USE_SETTINGS_FROM_AD=0 `
    MANAGEMENT_GROUP=HQ `
    MANAGEMENT_SERVER_DNS=JUBILEE `
    ACTIONS_USE_COMPUTER_ACCOUNT=1
```

So use this instead:

```PowerShell
msiexec.exe /i $msiPath `
    MANAGEMENT_GROUP=HQ `
    MANAGEMENT_SERVER_DNS=JUBILEE `
    ACTIONS_USE_COMPUTER_ACCOUNT=1
```

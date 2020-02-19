# VMM setup error

Thursday, January 12, 2017
11:02 AM

## Create failover cluster names in Active Directory

---

**FOOBAR8 - Run as TECHTOOLBOX\\jjameson-admin**

```PowerShell
cls
```

### # Create failover cluster name in Active Directory

```PowerShell
$failoverClusterName = "TT-VMM01-FC"
$description = "Failover cluster virtual network name"
$orgUnit = "OU=System Center Servers,OU=Servers,OU=Resources,OU=IT," `
    + "DC=corp,DC=technologytoolbox,DC=com"

New-ADComputer `
    -Name $failoverClusterName  `
    -Description $description `
    -Path $orgUnit `
    -Enabled:$false

Start-Sleep -Seconds 15
```

### # Add "Full Control" permission to the VMM administrators group (since a member of that group will create the cluster)

```PowerShell
$failoverClusterCreator = New-Object System.Security.Principal.NTAccount(
    "TECHTOOLBOX\VMM Admins")

$computer = Get-ADComputer -Identity $failoverClusterName
$computerObject = Get-ADObject -Identity $computer.DistinguishedName -Properties *
$securityDescriptor = $computerObject.nTSecurityDescriptor

$accessRule = New-Object System.DirectoryServices.ActiveDirectoryAccessRule(
    $failoverClusterCreator,
    "GenericAll",
    "Allow")

$securityDescriptor.AddAccessRule($accessRule)

Set-ADObject $computerObject -Replace @{ nTSecurityDescriptor = $securityDescriptor }

Start-Sleep -Seconds 15
```

### # Create failover cluster name for VMM

```PowerShell
$failoverClusterName = "TT-VMM01"
$description = "Failover cluster name for Virtual Machine Manager"
$orgUnit = "OU=System Center Servers,OU=Servers,OU=Resources,OU=IT," `
    + "DC=corp,DC=technologytoolbox,DC=com"

New-ADComputer `
    -Name $failoverClusterName  `
    -Description $description `
    -Path $orgUnit `
    -Enabled:$false

Start-Sleep -Seconds 15
```

### # Add "Full Control" permission to the VMM administrators group (since a member of that group will create the cluster)

```PowerShell
$failoverClusterCreator = New-Object System.Security.Principal.NTAccount(
    "TECHTOOLBOX\VMM Admins")

$computer = Get-ADComputer -Identity $failoverClusterName
$computerObject = Get-ADObject -Identity $computer.DistinguishedName -Properties *
$securityDescriptor = $computerObject.nTSecurityDescriptor

$accessRule = New-Object System.DirectoryServices.ActiveDirectoryAccessRule(
    $failoverClusterCreator,
    "GenericAll",
    "Allow")

$securityDescriptor.AddAccessRule($accessRule)

Set-ADObject $computerObject -Replace @{ nTSecurityDescriptor = $securityDescriptor }
```

---

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E2/B146BB04A55B61D96BE121AC1793F086D47B00E2.png)

Creation of the VMM resource group TT-VMM01 failed.\
Ensure that the group name is valid, and cluster resource or group with the same name does not exist, and the group name is not used in the network.

---

**C:\\ProgramData\\VMMLogs\\SetupWizard.log**

```Text
...
11:00:06:VMMPostinstallProcessor threw an exception: Threw Exception.Type: Microsoft.VirtualManager.Utils.CarmineException, Exception.Message: Creation of the VMM resource group TT-VMM01 failed.
Ensure that the group name is valid, and cluster resource or group with the same name does not exist, and the group name is not used in the network.
11:00:06:StackTrace:   at Microsoft.VirtualManager.Setup.ClusterServiceHelper.CreateService(String name, String[] ipAddress, String[] checkPointKey, PercentageProgressReport ReportProgress)
   at Microsoft.VirtualManager.Setup.ClusterServiceHelper.UpdateHAVMMResource()
   at Microsoft.VirtualManager.Setup.InstallItemCustomDelegates.<>c__DisplayClass13.<PangaeaServerPostinstallProcessor>b__e(IVmmDbConnection _dbConnection)
   at Microsoft.VirtualManager.DB.SqlContext.Connect(Action`1 action)
   at Microsoft.VirtualManager.Setup.InstallItemCustomDelegates.PangaeaServerPostinstallProcessor()
...
```

---

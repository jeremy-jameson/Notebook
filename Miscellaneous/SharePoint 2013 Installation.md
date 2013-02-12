# SharePoint 2013 Installation

Tuesday, February 12, 2013
5:27 AM

On the database server (BEAST), use SQL Server Management Studio to set the **Max degree of parallelism** to **1**.\
[http://technet.microsoft.com/en-us/library/ee805948.aspx](http://technet.microsoft.com/en-us/library/ee805948.aspx)

Error installing prerequisites:

![(screenshot)](https://assets.technologytoolbox.com/screenshots/6D/202844998C8769428243C74313AF666DDF78066D.png)

**The Products Preparation Tool in SharePoint Server 2013 may not progress past "Configuring Application Server Role, Web Server (IIS) Role"**\
Pasted from <[http://support.microsoft.com/kb/2765260](http://support.microsoft.com/kb/2765260)>

Downloaded hotfix for KB 2771431 (a.k.a. "Method 1"), but it reports that it is already installed.

Ran PowerShell commands (a.k.a. "Method 2"). In second command, added "-Source X:\\sources\\sxs" to avoid error ("The source files could not be downloaded.") by specifying location on Windows Server 2012 DVD.

Prerequisites installer subsequently failed installing AppFabric (presumably because download was taking too long).

Start over (rebuild CYCLOPS)

Create a snapshot (**Baseline Windows Server 2012**)

Run SharePoint prerequisites installer

![(screenshot)](https://assets.technologytoolbox.com/screenshots/1C/AA516D612A204F26C2DB6BD760A611367A0FF41C.png)

![(screenshot)](https://assets.technologytoolbox.com/screenshots/E5/92B21F4340418DE4DBBE08EA6503B651E9A046E5.png)

**The Products Preparation Tool in SharePoint Server 2013 may not progress past "Configuring Application Server Role, Web Server (IIS) Role"**\
Pasted from <[http://support.microsoft.com/kb/2765260](http://support.microsoft.com/kb/2765260)>

Downloaded hotfix for KB 2771431 (a.k.a. "Method 1"), but it reports that it is already installed.

Ran PowerShell commands (a.k.a. "Method 2"). In second command, added "-Source X:\\sources\\sxs" to avoid error ("The source files could not be downloaded.") by specifying location on Windows Server 2012 DVD.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/65/75F1C522F9F3D003ED105E7576310B9A31550A65.png)

After finishing prerequisites, installed hotfix:\
[http://support.microsoft.com/kb/2765317](http://support.microsoft.com/kb/2765317)

Power down, delete old snapshot, and create a new snapshot (**Baseline SharePoint Server 2013 installation**)

## Install SharePoint Server 2013

## Configure outgoing e-mail settings

## Configure diagnostic logging

## Configure usage and health data collection

## Configure State Service

## Configure managed accounts

- TECHTOOLBOX\\svc-sharepoint
- TECHTOOLBOX\\svc-search
- TECHTOOLBOX\\svc-index
- TECHTOOLBOX\\svc-spserviceapp
- TECHTOOLBOX\\svc-sp-usercode
- TECHTOOLBOX\\svc-sp-ups
- TECHTOOLBOX\\svc-ttweb
- TECHTOOLBOX\\svc-web-my-team
- TECHTOOLBOX\\svc-web-tfs

## Backup Farm

- ```PowerShell
      Backup-SPFarm -Directory '\\iceman\Backups\SharePoint Farm' -BackupMethod Full
  ```

[\\\\iceman\\Backups\\SharePoint Farm\\spbr0000](\\iceman\Backups\SharePoint Farm\spbr0000)

## Configure Search Service Application

Restore intranet Web application

1. ```PowerShell
       New-SPWebApplication -ApplicationPool "SharePoint - ttweb80" -Name "SharePoint - ttweb80" -ApplicationPoolAccount "TECHTOOLBOX\svc-ttweb" -AuthenticationMethod "NTLM" -HostHeader "ttweb" -Port 80 -Url "http://ttweb/"
   ```
2. ```PowerShell
       Test-SPContentDatabase -Name WSS_Content_ttweb -WebApplication http://ttweb
   ```
3. ```PowerShell
       Mount-SPContentDatabase -Name WSS_Content_ttweb -WebApplication http://ttweb
   ```

## Upgrade the Secure Store service application

Pasted from <[http://technet.microsoft.com/en-us/library/jj839719.aspx](http://technet.microsoft.com/en-us/library/jj839719.aspx)>

1. ```PowerShell
       $applicationPool = Get-SPServiceApplicationPool "SharePoint Service Applications"
   ```
2. ```PowerShell
       $sss = New-SPSecureStoreServiceApplication -Name 'Secure Store Service' -ApplicationPool $applicationPool -DatabaseName 'Secure_Store_Service_DB' -AuditingEnabled
   ```

Error occurred in upgrade timer job. Detailed error from log file:

ERROR	Sequence [Microsoft.Office.SecureStoreService.Server.Upgrade.SecureStoreDatabaseSequence] cannot upgrade an object [SecureStoreServiceDatabase Name=Secure_Store_Service_DB] whose build version [12.0.0.6421] is too old. Upgrade requires [14.0.4730.1010] or higher.	7facfe9b-61e1-105c-69ee-56e5959e0d7f

## Configure Secure Store Service

## Upgrade the Managed Metadata service application

Pasted from <[http://technet.microsoft.com/en-us/library/jj839719.aspx](http://technet.microsoft.com/en-us/library/jj839719.aspx)>

1. ```PowerShell
       New-SPMetadataServiceApplication -Name 'Managed Metadata Service' -ApplicationPool $applicationPool -DatabaseName 'ManagedMetadataService'
   ```
2. ```PowerShell
       New-SPMetadataServiceApplicationProxy -Name 'Managed Metadata Service' -ServiceApplication $mms-DefaultProxyGroup
   ```

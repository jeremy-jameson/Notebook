# Building VMs using MDT

Tuesday, April 14, 2015\
10:04 AM

## # Add setup files for Windows 8.1 and Windows Server 2012 R2

### # Import operating system - "Windows 8.1 Enterprise x64 with Update"

#### # Mount the installation image

```PowerShell
$imagePath = "\\ICEMAN\Products\Microsoft\Windows 8.1" `
    + "\en_windows_8.1_enterprise_with_update_x64_dvd_4065178.iso"

$imageDriveLetter = (Mount-DiskImage -ImagePath $imagePath -PassThru |
    Get-Volume).DriveLetter

Write-Host ("Source directory: " + $imageDriveLetter + ":\")
```

**Note:** Source directory should be **D:\\**

#### Import the operating system

1. Open **Deployment Workbench**, expand the **Deployment Shares** node, and then expand **MDT Build Lab ([\\\\ICEMAN\\MDT-Build\$](\\ICEMAN\MDT-Build$))**.
2. Right-click the **Operating Systems** node, and create a new folder named **Windows 8.1**.
3. Expand the **Operating Systems** node, right-click the **Windows 8.1** folder, and select **Import Operating System**.
4. Use the following settings for the **Import Operating System Wizard**:
   - OS Type
     - **Full set of source files**
   - Source
     - Source directory: **D:\\**
   - Destination
     - Destination directory name: **W81Ent-x64-Update**
5. Wait for the installation files to be copied.
6. After adding the operating system, in the **Operating Systems / Windows 8.1** folder, double-click the added operating system name in the **Operating System** node and change the name to **Windows 8.1 Enterprise x64 with Update**.

#### # Dismount the installation image

```PowerShell
Dismount-DiskImage -ImagePath $imagePath
```

### # Import operating system - "Windows Server 2012 R2 with Update"

#### # Mount the installation image

```PowerShell
$imagePath = "\\ICEMAN\Products\Microsoft\Windows Server 2012 R2" `
    + "\en_windows_server_2012_r2_with_update_x64_dvd_4065220.iso"

$imageDriveLetter = (Mount-DiskImage -ImagePath $imagePath -PassThru |
    Get-Volume).DriveLetter

Write-Host ("Source directory: " + $imageDriveLetter + ":\")
```

**Note:** Source directory should be **D:\\**

#### Import the operating system

1. Open **Deployment Workbench**, expand the **Deployment Shares** node, and then expand **MDT Build Lab (ICEMAN\\MDT-Build\$)**.
2. Right-click the **Operating Systems** node, and create a new folder named **Windows Server 2012 R2**.
3. Expand the **Operating Systems** node, right-click the **Windows Server 2012 R2** folder, and select **Import Operating System**.
4. Use the following settings for the **Import Operating System Wizard**:
   - OS Type
     - **Full set of source files**
   - Source
     - Source directory: **D:\\**
   - Destination
     - Destination directory name: **WS2012-R2-Update**
5. Wait for the installation files to be copied.
6. After adding the operating system, in the **Operating Systems / Windows Server 2012 R2** folder, double-click the added operating system name in the **Operating System** node and change the names to the following:
   - **Windows Server 2012 R2 Datacenter with Update**
   - **Windows Server 2012 R2 Datacenter (Server Core Installation) with Update**
   - **Windows Server 2012 R2 Standard with Update**
   - **Windows Server 2012 R2 Standard (Server Core Installation) with Update**

#### # Dismount the installation image

```PowerShell
Dismount-DiskImage -ImagePath $imagePath
```

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### Check-in files

---

## Create task sequences for building baseline images

### Create task sequence - "Windows 8.1 Enterprise x64 - Baseline"

1. Open **Deployment Workbench**, expand **Deployment Shares / MDT Build Lab ([\\\\ICEMAN\\MDT-Build\$](\\ICEMAN\MDT-Build$))**, right-click **Task Sequences**, and create a new folder named **Windows 8.1**.
2. Expand the **Task Sequences** node, right-click the **Windows 8.1** folder and select **New Task Sequence**.
3. Use the following settings for the **New Task Sequence Wizard**:
   - General Settings
     - Task sequence ID: **W81Ent-x64**
     - Task sequence name: **Windows 8.1 Enterprise x64 - Baseline**
     - Task sequence comments: **Baseline image (reference build)**
   - Select Template
     - Template: **Standard Client Task Sequence**
   - Select OS
     - **Windows 8.1 Enterprise x64 with Update**
   - Specify Product Key
     - **Do not specify a product key at this time.**
   - OS Settings
     - Full Name: **Windows User**
     - Organization: **Technology Toolbox**
     - Internet Explorer Home Page: **about:blank**
   - Admin Password
     - **Do not specify an Administrator password at this time.**

### Create task sequence - "Windows Server 2012 R2 Standard - Baseline"

1. Open **Deployment Workbench**, expand **Deployment Shares / MDT Build Lab ([\\\\ICEMAN\\MDT-Build\$](\\ICEMAN\MDT-Build$))**, right-click **Task Sequences**, and create a new folder named **Windows Server 2012 R2**.
2. Expand the **Task Sequences** node, right-click the **Windows Server 2012 R2** folder and select **New Task Sequence**.
3. Use the following settings for the **New Task Sequence Wizard**:
   - General Settings
     - Task sequence ID: **WS2012-R2**
     - Task sequence name: **Windows Server 2012 R2 Standard - Baseline**
     - Task sequence comments: **Baseline image (reference build)**
   - Select Template
     - Template: **Standard Server Task Sequence**
   - Select OS
     - **Windows Server 2012 R2 Standard with Update**
   - Specify Product Key
     - **Specify the product key for this operating system.**
     - Product Key: {product key}
   - OS Settings
     - Full Name: **Windows User**
     - Organization: **Technology Toolbox**
     - Internet Explorer Home Page: **about:blank**
   - Admin Password
     - **Use the specified local Administrator password.**
     - Administrator Password: **{password}**

> **Important**
>
> The MSDN version of Windows Server 2012 R2 will prompt to enter a product key (but provide an option to skip this step). It does not honor the SkipProductKey=YES entry in the MDT CustomSettings.ini file.

> **Important**
>
> Windows Server 2012 R2 with Update requires a password to be specified for the Administrator account (unlike Windows 8.1). If an Administrator password is not specified in the task sequence, the Lite Touch Installation will prompt for a password (which must be subsequently be entered manually when completing the actions specified in the task sequence).

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### Add new files to TFS

```PowerShell
& "C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\TF.exe" add Main /r
```

#### Check-in files

---

## Configure MDT build lab deployment share

### Configure MDT deployment settings

1. Open **Deployment Workbench**, expand **Deployment Shares**, right-click** MDT Build Lab ([\\\\ICEMAN\\MDT-Build\$](\\ICEMAN\MDT-Build$))**, and click** Properties.**
2. On the **Rules** tab:
   1. Specify the following rules:
   2. Click **Edit Bootstrap.ini** and specify the following information:
3. On the **Windows PE** tab:
   1. In the **Platform** drop-down list, click **x86**.
   2. In the **Lite Touch Boot Image Settings** section, configure the following settings:
      1. Image description: **MDT Build Lab (x86)**
      2. ISO file name: **MDT-Build-x86.iso**
   3. In the **Platform** drop-down list, click **x64**.
   4. In the **Lite Touch Boot Image Settings** section, configure the following settings:
      1. Image description: **MDT Build Lab (x64)**
      2. ISO file name: **MDT-Build-x64.iso**
4. Click **OK**.

```INI
        [Settings]
        Priority=Default

        [Default]
        _SMSTSORGNAME=Technology Toolbox
        UserDataLocation=NONE
        DoCapture=YES
        BackupFile=%TaskSequenceID%_# Replace(Replace(Replace(FormatDateTime(Now,0),"/","-"), " ", "-"), ":", "-") #.wim
        OSInstall=Y
        TimeZoneName=Mountain Standard Time
        JoinWorkgroup=WORKGROUP
        HideShell=YES
        FinishAction=SHUTDOWN
        DoNotCreateExtraPartition=YES
        WSUSServer=http://colossus.corp.technologytoolbox.com:8530
        ApplyGPOPack=NO
        SLSHARE=\\ICEMAN\MDT-Build$\Logs

        SkipAdminPassword=YES
        SkipProductKey=YES
        SkipComputerName=YES
        SkipDomainMembership=YES
        SkipUserData=YES
        SkipLocaleSelection=YES
        SkipTaskSequence=NO
        SkipTimeZone=YES
        SkipApplications=YES
        SkipBitLocker=YES
        SkipSummary=YES
        SkipRoles=YES
        SkipCapture=NO
        SkipFinalSummary=YES
```

```INI
        [Settings]
        Priority=Default

        [Default]
        DeployRoot=\\ICEMAN\MDT-Build$
        UserDomain=TECHTOOLBOX
        UserID=s-mdt-build
        SkipBDDWelcome=YES
```

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### Check-in files

---

### Update the deployment share (create Windows PE boot images)

After the deployment share has been configured, it needs to be updated. This is the process when the Windows PE boot images are created.

1. Open **Deployment Workbench**, right-click the **MDT Build Lab ([\\\\ICEMAN\\MDT-Build\$](\\ICEMAN\MDT-Build$))** and click **Update Deployment Share**.
2. Use the default options for the **Update Deployment Share Wizard**.

| **Note**                                      |
| --------------------------------------------- |
| The update process will take 5 to 10 minutes. |
|                                               |

## Build baseline images

### # Copy MDT boot images to Products file share

```PowerShell
robocopy '\\ICEMAN\MDT-Build$\Boot' '\\ICEMAN\Products\Microsoft' *.iso
```

---

**STORM**

### # Create temporary VM to build image (Windows 8.1 Enterprise x64 - Baseline)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
</table>

---

**STORM**

### # Create temporary VM to build image (Windows Server 2012 R2 Standard - Baseline)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 14:12</p>
</td>
<td valign='top'>
<p>2015-03-27 14:31</p>
</td>
<td valign='top'>
<p>00:23:32</p>
</td>
<td valign='top'>
<p>3,621,239</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
</table>

## Customize baseline images

### Configure task sequence - "Windows 8.1 Enterprise x64 - Baseline"

Edit the task sequence to include the actions required to update the reference image with the latest updates from WSUS, copy Toolbox content from ICEMAN, install .NET Framework 3.5, update PowerShell help files, and easily suspend the deployment process after installing applications.

1. In the **Task Sequences / Windows 8.1** folder, right-click **Windows 8.1 Enterprise x64 - Baseline **and click **Properties**.
2. On the **Task Sequence** tab, configure the following settings:
   1. **State Restore**
      1. Enable the **Windows Update (Pre-Application Installation)** action.
      2. Enable the **Windows Update (Post-Application Installation)** action.
      3. After the **Tattoo** action, add a new **Group** action with the following setting:
         1. Name: **Custom Tasks (Pre-Windows Update)**
      4. After the **Windows Update (Post-Application Installation)** action, rename **Custom Tasks** to **Custom Tasks (Post-Windows Update)**.
      5. Select **Custom Tasks (Pre-Windows Update)** and add a new **Run Command Line **action with the following settings:
         1. Name: **Copy Toolbox content from ICEMAN**
         2. Command line: **robocopy [\\\\ICEMAN\\Public\\Toolbox](\\ICEMAN\Public\Toolbox) C:\\NotBackedUp\\Public\\Toolbox /E**
         3. Success codes: **0 3010 1 2 3 4 5 6 7 8 16**
      6. Select **Custom Tasks (Pre-Windows Update)** and add a new **Install Roles and Features** action with the following settings:
         1. Name: **Install - Microsoft NET Framework 3.5**
         2. Select the operating system for which roles are to be installed: **Windows 8.1**
         3. Select the roles and features that should be installed: **.NET Framework 3.5 (includes .NET 2.0 and 3.0)**
      7. Select **Custom Tasks (Pre-Windows Update)** and add a new **Run Command Line **action with the following settings:
         1. Name: **Update PowerShell help files**
         2. Command line: **PowerShell.exe -Command "& { Update-Help }"**
      8. Select **Install Applications **and add a new **Run Command Line **action with the following settings:
         1. Name: **Suspend**
         2. Command line: **cscript.exe "%SCRIPTROOT%\\LTISuspend.wsf"**
         3. Disable this step:** Yes (checked)**
3. Click **OK**.

> **Note**
>
> The reason for adding the applications after the Tattoo action but before running Windows Update is simply to save time during the deployment. This way we can add all applications that will upgrade some of the built-in components and avoid unnecessary updating.

Repeat the steps above for the **Windows Server 2012 R2 Standard** task sequence.

## Build baseline images

---

**STORM**

### # Create temporary VM to build image (Windows 8.1 Enterprise x64 - Baseline)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 14:12</p>
</td>
<td valign='top'>
<p>2015-03-27 14:31</p>
</td>
<td valign='top'>
<p>00:23:32</p>
</td>
<td valign='top'>
<p>3,621,239</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 15:12</p>
</td>
<td valign='top'>
<p>2015-03-27 19:02</p>
</td>
<td valign='top'>
<p>03:53:46</p>
</td>
<td valign='top'>
<p>5,969,687</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
</table>

---

**STORM**

### # Create temporary VM to build image (Windows Server 2012 R2 Standard - Baseline)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 14:12</p>
</td>
<td valign='top'>
<p>2015-03-27 14:31</p>
</td>
<td valign='top'>
<p>00:23:32</p>
</td>
<td valign='top'>
<p>3,621,239</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 15:12</p>
</td>
<td valign='top'>
<p>2015-03-27 19:02</p>
</td>
<td valign='top'>
<p>03:53:46</p>
</td>
<td valign='top'>
<p>5,969,687</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 19:26</p>
</td>
<td valign='top'>
<p>2015-03-27 22:19:09</p>
</td>
<td valign='top'>
<p>02:52:28</p>
</td>
<td valign='top'>
<p>5,552,836</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
</table>

## Download latest setup files for Windows 8.1 and Windows Server 2012 R2 from MSDN

Previous Windows 8.1 build: 6.3.9600.17031\
Latest Windows 8.1 build on MSDN: 6.3.9600.17415

Delete the operating systems in the **Windows 8.1** and **Windows Server 2012 R2** folders.

### # Add the latest Windows 8.1 installation files

#### # Mount the installation image

```PowerShell
$imagePath = "\\ICEMAN\Products\Microsoft\Windows 8.1" `
    + "\en_windows_8.1_enterprise_with_update_x64_dvd_6054382.iso"

$imageDriveLetter = (Mount-DiskImage -ImagePath $imagePath -PassThru |
    Get-Volume).DriveLetter

Write-Host ("Source directory: " + $imageDriveLetter + ":\")
```

**# Note:** Source directory should be **D:\\**

#### # Add Windows 8.1 Enterprise x64 (full source)

```PowerShell
Import-Module 'C:\Program Files\Microsoft Deployment Toolkit\Bin\MicrosoftDeploymentToolkit.psd1'

New-PSDrive -Name "DS001" -PSProvider MDTProvider -Root \\ICEMAN\MDT-Build$

$os = Import-MDTOperatingSystem `
    -Path "DS001:\Operating Systems\Windows 8.1" `
    -SourcePath D:\ `
    -DestinationFolder W81Ent-x64-Update

$os.RenameItem("Windows 8.1 Enterprise x64 with Update")
```

#### # Dismount the installation image

```PowerShell
Dismount-DiskImage -ImagePath $imagePath
```

```PowerShell
cls
```

### # Add the latest Windows Server 2012 R2 installation files

#### # Mount the installation image

```PowerShell
$imagePath = "\\ICEMAN\Products\Microsoft\Windows Server 2012 R2" `
    + "\en_windows_server_2012_r2_with_update_x64_dvd_6052708.iso"

$imageDriveLetter = (Mount-DiskImage -ImagePath $imagePath -PassThru |
    Get-Volume).DriveLetter

Write-Host ("Source directory: " + $imageDriveLetter + ":\")
```

**# Note:** Source directory should be **D:\\**

#### # Add Windows Server 2012 R2 (full source)

```PowerShell
$os = Import-MDTOperatingSystem `
    -Path "DS001:\Operating Systems\Windows Server 2012 R2" `
    -SourcePath D:\ `
    -DestinationFolder WS2012-R2-Update

$os[0].RenameItem("Windows Server 2012 R2 Standard (Server Core Installation) with Update")

$os[1].RenameItem("Windows Server 2012 R2 Standard with Update")

$os[2].RenameItem("Windows Server 2012 R2 Datacenter (Server Core Installation) with Update")

$os[3].RenameItem("Windows Server 2012 R2 Datacenter with Update")
```

#### # Dismount the installation image

```PowerShell
Dismount-DiskImage -ImagePath $imagePath
```

### # Update OS GUIDs in task sequences

```PowerShell
Notepad \\ICEMAN\MDT-Build$\Control\W81ENT-X64\ts.xml
```

Change the **/sequence/globalVarList/variable[@name="OSGUID"]** and **/sequence/group[@name="Install"]/step[@type="BDD_InstallOS"]/defaultVarList/variable[@name="OSGUID"] **elements to:

```Console
    <variable name="OSGUID" property="OSGUID">{be46f1aa-2066-4beb-b183-5c4845ada6f1}</variable>

Notepad \\ICEMAN\MDT-Build$\Control\WS2012-R2\ts.xml
```

Change the **/sequence/globalVarList/variable[@name="OSGUID"]** and **/sequence/group[@name="Install"]/step[@type="BDD_InstallOS"]/defaultVarList/variable[@name="OSGUID"] **elements to:

```XML
    <variable name="OSGUID" property="OSGUID">{c82b94fc-d3e1-4d86-acec-1bcd0cd59939}</variable>
```

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### Check-in files

---

## Build baseline images

---

**STORM**

### # Create temporary VM to build image (Windows 8.1 Enterprise x64 - Baseline)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 14:12</p>
</td>
<td valign='top'>
<p>2015-03-27 14:31</p>
</td>
<td valign='top'>
<p>00:23:32</p>
</td>
<td valign='top'>
<p>3,621,239</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 15:12</p>
</td>
<td valign='top'>
<p>2015-03-27 19:02</p>
</td>
<td valign='top'>
<p>03:53:46</p>
</td>
<td valign='top'>
<p>5,969,687</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 19:26</p>
</td>
<td valign='top'>
<p>2015-03-27 22:19:09</p>
</td>
<td valign='top'>
<p>02:52:28</p>
</td>
<td valign='top'>
<p>5,552,836</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 06:20</p>
</td>
<td valign='top'>
<p>2015-03-28 08:00</p>
</td>
<td valign='top'>
<p>01:44:18</p>
</td>
<td valign='top'>
<p>4,354,499</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
</table>

---

**STORM**

### # Create temporary VM to build image (Windows Server 2012 R2 Standard - Baseline)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 14:12</p>
</td>
<td valign='top'>
<p>2015-03-27 14:31</p>
</td>
<td valign='top'>
<p>00:23:32</p>
</td>
<td valign='top'>
<p>3,621,239</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 15:12</p>
</td>
<td valign='top'>
<p>2015-03-27 19:02</p>
</td>
<td valign='top'>
<p>03:53:46</p>
</td>
<td valign='top'>
<p>5,969,687</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 19:26</p>
</td>
<td valign='top'>
<p>2015-03-27 22:19:09</p>
</td>
<td valign='top'>
<p>02:52:28</p>
</td>
<td valign='top'>
<p>5,552,836</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 06:20</p>
</td>
<td valign='top'>
<p>2015-03-28 08:00</p>
</td>
<td valign='top'>
<p>01:44:18</p>
</td>
<td valign='top'>
<p>4,354,499</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 08:20</p>
</td>
<td valign='top'>
<p>2015-03-28 09:26</p>
</td>
<td valign='top'>
<p>01:09:10</p>
</td>
<td valign='top'>
<p>5,219,797</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
</table>

## Build baseline image (Windows 8.1 + Office 2013)

---

**STORM**

### # Create temporary VM to build image (Windows 8.1 Enterprise x64 - Baseline)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 14:12</p>
</td>
<td valign='top'>
<p>2015-03-27 14:31</p>
</td>
<td valign='top'>
<p>00:23:32</p>
</td>
<td valign='top'>
<p>3,621,239</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 15:12</p>
</td>
<td valign='top'>
<p>2015-03-27 19:02</p>
</td>
<td valign='top'>
<p>03:53:46</p>
</td>
<td valign='top'>
<p>5,969,687</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 19:26</p>
</td>
<td valign='top'>
<p>2015-03-27 22:19:09</p>
</td>
<td valign='top'>
<p>02:52:28</p>
</td>
<td valign='top'>
<p>5,552,836</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 06:20</p>
</td>
<td valign='top'>
<p>2015-03-28 08:00</p>
</td>
<td valign='top'>
<p>01:44:18</p>
</td>
<td valign='top'>
<p>4,354,499</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 08:20</p>
</td>
<td valign='top'>
<p>2015-03-28 09:26</p>
</td>
<td valign='top'>
<p>01:09:10</p>
</td>
<td valign='top'>
<p>5,219,797</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 10:01</p>
</td>
<td valign='top'>
<p>2015-03-28 12:28</p>
</td>
<td valign='top'>
<p>02:26:43</p>
</td>
<td valign='top'>
<p>7,571,734</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
</table>

## Add custom action to cleanup image before SysPrep

Reference:

**Nice to Know - Get rid of all junk before Sysprep and Capture when creating a reference image in MDT**\
From <[https://anothermike2.wordpress.com/2014/06/05/nice-to-know-get-rid-of-all-junk-before-sysprep-and-capture-when-creating-a-reference-image-in-mdt/](https://anothermike2.wordpress.com/2014/06/05/nice-to-know-get-rid-of-all-junk-before-sysprep-and-capture-when-creating-a-reference-image-in-mdt/)>

1. Download custom script: [http://1drv.ms/ThvLFE](http://1drv.ms/ThvLFE)
2. Unblock zip file and extract to folder ("C:\\NotBackedUp\\Temp\\Action - Cleanup before Sysprep")

#### Add custom action to execute the cleanup script

1. Open **Deployment Workbench**, expand the **Deployment Shares** node, and then expand **MDT Build Lab ([\\\\ICEMAN\\MDT-Build\$](\\ICEMAN\MDT-Build$))**.
2. Right-click the **Applications **node, and create a new folder named **Actions**.
3. Expand the **Applications **node, right-click the **Actions **folder, and click **New Application**.
4. Use the following settings for the **New Application Wizard**:
   - Application Type
     - **Application with source files**
   - Details
     - Application Name: **Action - Cleanup before Sysprep**
   - Source
     - Source directory: **C:\\NotBackedUp\\Temp\\Action - Cleanup before Sysprep**
   - Destination
     - Directory name: **Action - Cleanup before Sysprep**
   - Command Details
     - Command line: **cscript.exe Action-CleanupBeforeSysprep.wsf**
     - Working directory: **.\\Applications\\Action - Cleanup before Sysprep**

#### Add action to task sequence to cleanup image before SysPrep

1. In the **Task Sequences / Windows 8.1 **folder, right-click **Windows 8.1 Enterprise x64 - Baseline **and click **Properties**.
2. On the **Task Sequence** tab, configure the following settings:
   1. **State Restore**
      1. After the **Apply Local GPO Package** action, add a new **Group** action with the following setting:
         1. Name: **Cleanup before Sysprep**
      2. In the **Cleanup before Sysprep **group, add a new **Group** action with the following setting:
         1. Name: **Compress the image**
      3. Select **Compress the image **and add a new **Restart computer **action.
      4. Select **Compress the image** and add a new **Install Application** action with the following settings:
         1. Name: **Action - Cleanup before Sysprep**
         2. **Install a single application**
         3. Application to install: **Action - Cleanup before Sysprep**
      5. Select **Compress the image **and add a new **Restart computer **action.
3. Click **OK**.

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### Check-in files

---

## Build baseline image (Windows 8.1 + Office 2013 + cleanup before Sysprep)

---

**STORM**

### # Create temporary VM to build image (Windows 8.1 Enterprise x64 - Baseline)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 14:12</p>
</td>
<td valign='top'>
<p>2015-03-27 14:31</p>
</td>
<td valign='top'>
<p>00:23:32</p>
</td>
<td valign='top'>
<p>3,621,239</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 15:12</p>
</td>
<td valign='top'>
<p>2015-03-27 19:02</p>
</td>
<td valign='top'>
<p>03:53:46</p>
</td>
<td valign='top'>
<p>5,969,687</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 19:26</p>
</td>
<td valign='top'>
<p>2015-03-27 22:19:09</p>
</td>
<td valign='top'>
<p>02:52:28</p>
</td>
<td valign='top'>
<p>5,552,836</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 06:20</p>
</td>
<td valign='top'>
<p>2015-03-28 08:00</p>
</td>
<td valign='top'>
<p>01:44:18</p>
</td>
<td valign='top'>
<p>4,354,499</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 08:20</p>
</td>
<td valign='top'>
<p>2015-03-28 09:26</p>
</td>
<td valign='top'>
<p>01:09:10</p>
</td>
<td valign='top'>
<p>5,219,797</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 10:01</p>
</td>
<td valign='top'>
<p>2015-03-28 12:28</p>
</td>
<td valign='top'>
<p>02:26:43</p>
</td>
<td valign='top'>
<p>7,571,734</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 21:01</p>
</td>
<td valign='top'>
<p>2015-03-29 00:11</p>
</td>
<td valign='top'>
<p>03:21:31</p>
</td>
<td valign='top'>
<p>7,273,045</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
<li>Cleanup before Sysprep</li>
</ul>
</td>
</tr>
</table>

```PowerShell
cls
```

## # Add applications - SQL Server 2014 Developer Edition and Visual Studio 2013

### # Create application: SQL Server 2014 Developer Edition - x64 (Complete)

#### # Mount the installation image

```PowerShell
$imagePath = "\\ICEMAN\Products\Microsoft\SQL Server 2014" `
    + "\en_sql_server_2014_developer_edition_x64_dvd_3940406.iso"

$imageDriveLetter = (Mount-DiskImage -ImagePath $imagePath -PassThru |
    Get-Volume).DriveLetter

$appSourcePath = $imageDriveLetter + ":\"
```

#### # Add application - SQL Server 2014 Developer Edition - x64 (Complete)

```PowerShell
Import-Module 'C:\Program Files\Microsoft Deployment Toolkit\Bin\MicrosoftDeploymentToolkit.psd1'

New-PSDrive -Name "DS001" -PSProvider MDTProvider -Root \\ICEMAN\MDT-Build$
```

\$appName = "SQL Server 2014 Developer Edition - x64 (Complete)"

```PowerShell
$appShortName = "SQL2014-Dev-x64"
$appSetupFolder = $appShortName
$commandLine = "Setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=PrepareImage" `
    + " /FEATURES=SQL,AS,RS,DQC,IS,MDS,Tools /INSTANCEID=MSSQLSERVER"

Import-MDTApplication `
    -Path "DS001:\Applications\Microsoft" `
    -Name $appName `
    -ShortName $appShortName `
    -ApplicationSourcePath $appSourcePath `
    -DestinationFolder $appSetupFolder `
    -CommandLine $commandLine `
    -WorkingDirectory ".\Applications\$appSetupFolder"
```

**Note:** An error occurs if you try to install SQL Server 2014 after Visual Studio 2013 has already been installed.

![(screenshot)](https://assets.technologytoolbox.com/screenshots/CA/E7142E76BC97530A98ACD966E07C0FC0DA0251CA.png)

#### # Dismount the installation image

```PowerShell
Dismount-DiskImage -ImagePath $imagePath
```

```PowerShell
cls
```

### # Create application: SQL Server 2014 Developer Edition - x64 (Database Engine and Management Tools)

\$appName = "SQL Server 2014 Developer Edition - x64" `

```PowerShell
    + " (Database Engine and Management Tools)"

$appShortName = "SQL2014-Dev-x64-DE-MT"
$appSetupFolder = "SQL2014-Dev-x64"
$commandLine = "Setup.exe /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=PrepareImage" `
    + " /FEATURES=SQLEngine,ADV_SSMS /INSTANCEID=MSSQLSERVER"

Import-MDTApplication `
    -Path "DS001:\Applications\Microsoft" `
    -Name $appName `
    -ShortName $appShortName `
    -NoSource `
    -CommandLine $commandLine `
    -WorkingDirectory ".\Applications\$appSetupFolder"
```

```PowerShell
cls
```

### # Create application: Visual Studio 2013 with Update 4 (Default)

#### # Mount the installation image

```PowerShell
$imagePath = "\\ICEMAN\Products\Microsoft\Visual Studio 2013" `
    + "\en_visual_studio_ultimate_2013_with_update_4_x86_dvd_5935075.iso"

$imageDriveLetter = (Mount-DiskImage -ImagePath $imagePath -PassThru |
    Get-Volume).DriveLetter

$appSourcePath = $imageDriveLetter + ":\"
```

#### # Add application - Visual Studio 2013 with Update 4 (Default)

\$appName = "Visual Studio 2013 with Update 4 (Default)"

```PowerShell
$appShortName = "VS2013-Update4"
$appSetupFolder = $appShortName
$commandLine = "vs_ultimate.exe /Quiet /NoRestart" `
    + " /AdminFile Z:\Applications\appSetupFolder\AdminDeployment.xml"
```

> **Important**
>
> You must specify the full path for the **AdminFile** parameter or else vs_ultimate.exe terminates with an error.

```PowerShell
Import-MDTApplication `
    -Path "DS001:\Applications\Microsoft" `
    -Name $appName `
    -ShortName $appShortName `
    -ApplicationSourcePath $appSourcePath `
    -DestinationFolder $appSetupFolder `
    -CommandLine $commandLine `
    -WorkingDirectory ".\Applications\$appSetupFolder"
```

#### # Dismount the installation image

```PowerShell
Dismount-DiskImage -ImagePath $imagePath
```

```PowerShell
cls
```

### # Create application: Visual Studio 2013 with Update 4 (SP2013 Development)

#### # Create custom "AdminFile" for unattended Visual Studio 2013 installation for SP2013 development

\$path = "[\\\\ICEMAN\\MDT-Build\$\\Applications\\VS2013-Update4](\\ICEMAN\MDT-Build$\Applications\VS2013-Update4)"

```PowerShell
Copy-Item "$path\AdminDeployment.xml" "$path\AdminDeployment-SP2013-Dev.xml"
```

#### # Edit custom Visual Studio 2013 "AdminFile" for SP2013 development

```PowerShell
Notepad "$path\AdminDeployment-SP2013-Dev.xml"
```

Make the following changes:

- In the `<BundleCustomizations>` element, change the NoWeb attribute to "yes"
- Change the Selected attributes for the following `<SelectableItemCustomization>` elements to "no":
  - Id="Blend"
  - Id="LightSwitch"
  - Id="VC_MFC_Libraries"
  - Id="SilverLight_Developer_Kit"

#### # Add application - Visual Studio 2013 with Update 4 (SP2013 Development)

\$appName = "Visual Studio 2013 with Update 4 (SP2013 Development)"

```PowerShell
$appShortName = "VS2013-Update4-SP2013-Dev"
$appSetupFolder = "VS2013-Update4"
$commandLine = "vs_ultimate.exe /Quiet /NoRestart" `
    + " /AdminFile Z:\Applications\$appSetupFolder\AdminDeployment-SP2013-Dev.xml"
```

> **Important**
>
> You must specify the full path for the **AdminFile** parameter or else vs_ultimate.exe terminates with an error.

```PowerShell
Import-MDTApplication `
    -Path "DS001:\Applications\Microsoft" `
    -Name $appName `
    -ShortName $appShortName `
    -NoSource `
    -CommandLine $commandLine `
    -WorkingDirectory ".\Applications\$appSetupFolder"
```

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools

robocopy '\\ICEMAN\MDT-Build$\Applications\VS2013-Update4' '.\Main\MDT-Build$\Applications\VS2013-Update4' AdminDeployment-SP2013-Dev.xml
```

#### # Add files to TFS

```PowerShell
& "C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\TF.exe" add Main /r
```

#### Check-in files

---

```PowerShell
cls
```

### # Create application: Microsoft Office Developer Tools for Visual Studio 2013 - November 2014 Update

#### Download update using Web Platform Installer

- Folder: [\\\\ICEMAN\\Products\\Microsoft\\Visual Studio 2013\\OfficeToolsForVS2013Update1](\\ICEMAN\Products\Microsoft\Visual Studio 2013\OfficeToolsForVS2013Update1)

#### # Add application - Microsoft Office Developer Tools for Visual Studio 2013 - November 2014 Update

\$appName = "Microsoft Office Developer Tools for Visual Studio 2013" `

```PowerShell
    + " - November 2014 Update"

$appShortName = "OfficeToolsForVS2013Update1"
$appSourcePath = "\\ICEMAN\Products\Microsoft\Visual Studio 2013" `
    + "\OfficeToolsForVS2013Update1"

$appSetupFolder = $appShortName
$commandLine = "cba_bundle.exe /Quiet /NoRestart"

Import-MDTApplication `
    -Path "DS001:\Applications\Microsoft" `
    -Name $appName `
    -ShortName $appShortName `
    -ApplicationSourcePath $appSourcePath `
    -DestinationFolder $appSetupFolder `
    -CommandLine $commandLine `
    -WorkingDirectory ".\Applications\$appSetupFolder"
```

```PowerShell
cls
```

### # Create application: Microsoft SQL Server Update for database tooling

#### Download ISO for SQL Server Database Tools (SSDT_12.0.50318.0_EN.iso)

- Download link:\
  **SQL Server database tooling in Visual Studio 2013**\
  From <[https://msdn.microsoft.com/en-us/dn864412](https://msdn.microsoft.com/en-us/dn864412)>
- Destination folder: [\\\\ICEMAN\\Products\\Microsoft\\Visual Studio 2013\\](\\ICEMAN\Products\Microsoft\Visual Studio 2013\)

#### # Mount the installation image

```PowerShell
$imagePath = "\\ICEMAN\Products\Microsoft\Visual Studio 2013\SSDT_12.0.50318.0_EN.iso"

$imageDriveLetter = (Mount-DiskImage -ImagePath $imagePath -PassThru |
    Get-Volume).DriveLetter

$appSourcePath = $imageDriveLetter + ":\"
```

#### # Add application - Microsoft SQL Server Update for database tooling (VS2013)

\$appName = "Microsoft SQL Server Update for database tooling (VS2013)"

```PowerShell
$appShortName = "SSDT-VS2013"
$appSetupFolder = $appShortName
$commandLine = "SSDTSETUP.EXE /q /norestart"

Import-MDTApplication `
    -Path "DS001:\Applications\Microsoft" `
    -Name $appName `
    -ShortName $appShortName `
    -ApplicationSourcePath $appSourcePath `
    -DestinationFolder $appSetupFolder `
    -CommandLine $commandLine `
    -WorkingDirectory ".\Applications\$appSetupFolder"
```

#### # Dismount the installation image

```PowerShell
Dismount-DiskImage -ImagePath $imagePath
```

```PowerShell
cls
```

### # Create application bundle: Visual Studio 2013 for SP2013 Development

#### # Add application bundle - Visual Studio 2013 for SP2013 Development

\$appName = "Visual Studio 2013 for SP2013 Development"

```PowerShell
$appShortName = "VS2013-SP2013"

Import-MDTApplication `
    -Path "DS001:\Applications\Microsoft" `
    -Name $appName `
    -ShortName $appShortName `
    -Bundle
```

#### # Add application bundle - Visual Studio 2013 for SP2013 Development

Add applications to bundle (be sure to specify the following order):

1. **Visual Studio 2013 with Update 4 (SP2013 Development)**
2. **Microsoft Office Developer Tools for Visual Studio 2013 - November 2014 Update**
3. **Microsoft SQL Server Update for database tooling (VS2013)**

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### Check-in files

---

```PowerShell
cls
```

### # Create application: SharePoint Server 2013 with Service Pack 1

#### # Mount the installation image

```PowerShell
$imagePath = "\\ICEMAN\Products\Microsoft\SharePoint 2013\" `
    + "en_sharepoint_server_2013_with_sp1_x64_dvd_3823428.iso"

$imageDriveLetter = (Mount-DiskImage -ImagePath $imagePath -PassThru |
    Get-Volume).DriveLetter

$appSourcePath = $imageDriveLetter + ":\"
```

#### # Add application - SharePoint Server 2013 with Service Pack 1

\$appName = "SharePoint Server 2013 with Service Pack 1"

```PowerShell
$appShortName = "SP2013-SP1"
$appSetupFolder = $appShortName
$commandLine = "setup.exe /config Config.xml"

Import-MDTApplication `
    -Path "DS001:\Applications\Microsoft" `
    -Name $appName `
    -ShortName $appShortName `
    -ApplicationSourcePath $appSourcePath `
    -DestinationFolder $appSetupFolder `
    -CommandLine $commandLine `
    -WorkingDirectory ".\Applications\$appSetupFolder"
```

#### # Dismount the installation image

```PowerShell
Dismount-DiskImage -ImagePath $imagePath
```

#### # Create configuration file for unattended SharePoint 2013 setup

```PowerShell
$xml = New-Object XML
$xml.LoadXml(
'<Configuration>
  <Package Id="sts">
    <Setting Id="LAUNCHEDFROMSETUPSTS" Value="Yes"/>
  </Package>
  <Package Id="spswfe">
    <Setting Id="SETUPCALLED" Value="1"/>
  </Package>
  <Logging Type="verbose" Path="%temp%" Template="SharePoint Server Setup(*).log"/>
  <!-- Enterprise trial key = NQTMW-K63MQ-39G6H-B2CH9-FRDWJ -->
  <PIDKEY Value="NQTMW-K63MQ-39G6H-B2CH9-FRDWJ"/>
  <Setting Id="SERVERROLE" Value="APPLICATION"/>
  <Setting Id="SETUPTYPE" Value="CLEAN_INSTALL"/>
  <Setting Id="SETUP_REBOOT" Value="Never"/>
  <Display Level="None" AcceptEula="Yes"/>
</Configuration>')

$xml.Save("\\ICEMAN\MDT-Build$\Applications\$appSetupFolder\Config.xml")
```

#### # Create script to install SharePoint 2013 prerequisites

```PowerShell
Notepad.exe "\\ICEMAN\MDT-Build$\Applications\$appSetupFolder\prerequisiteinstaller.cmd"
```

Copy/paste the following into the file:

```Console
setlocal

set SOURCE_PATH=\\ICEMAN\Products\Microsoft\SharePoint 2013\PrerequisiteInstallerFiles_SP1

REM An error occurs if %TEMP% is used to store the prerequisite files when installing
REM via the Microsoft Deployment Toolkit (presumably due to cleanup of temp files during
REM reboot)...
REM
REM set LOCAL_PATH=%TEMP%
REM
REM ...instead copy the files to a custom folder at the root of the C: drive
set LOCAL_PATH=C:\PrerequisiteInstallerFiles_SP1

robocopy "%SOURCE_PATH%" "%LOCAL_PATH%"

PrerequisiteInstaller.exe %* ^
    /SQLNCli:"%LOCAL_PATH%\sqlncli.msi" ^
    /PowerShell:"%LOCAL_PATH%\Windows6.1-KB2506143-x64.msu" ^
    /NETFX:"%LOCAL_PATH%\dotNetFx45_Full_setup.exe" ^
    /IDFX:"%LOCAL_PATH%\Windows6.1-KB974405-x64.msu" ^
    /Sync:"%LOCAL_PATH%\Synchronization.msi" ^
    /AppFabric:"%LOCAL_PATH%\WindowsServerAppFabricSetup_x64.exe" ^
    /IDFX11:"%LOCAL_PATH%\MicrosoftIdentityExtensions-64.msi" ^
    /MSIPCClient:"%LOCAL_PATH%\setup_msipc_x64.msi" ^
    /WCFDataServices:"%LOCAL_PATH%\WcfDataServices.exe" ^
    /KB2671763:"%LOCAL_PATH%\AppFabric1.1-RTM-KB2671763-x64-ENU.exe" ^
    /WCFDataServices56:"%LOCAL_PATH%\WcfDataServices-5.6.exe"
```

> **Important**
>
> The prerequisite files are copied locally to avoid a prompt when running WcfDataServices.exe (despite unblocking that file in the network file share).

```PowerShell
cls
```

### # Create application: SharePoint Server 2013 with Service Pack 1 - Prerequisites

\$appName = "SharePoint Server 2013 with Service Pack 1 - Prerequisites"

```PowerShell
$appShortName = "SP2013-SP1-Prereq"
$appSetupFolder = "SP2013-SP1"
$commandLine = "prerequisiteinstaller.cmd /unattended"

Import-MDTApplication `
    -Path "DS001:\Applications\Microsoft" `
    -Name $appName `
    -ShortName $appShortName `
    -NoSource `
    -CommandLine $commandLine `
    -WorkingDirectory ".\Applications\$appSetupFolder"
```

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools

robocopy '\\ICEMAN\MDT-Build$\Applications\SP2013-SP1' '.\Main\MDT-Build$\Applications\SP2013-SP1' Config.xml prerequisiteinstaller.cmd
```

#### # Add files to TFS

```PowerShell
& "C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\TF.exe" add Main /r
```

#### Check-in files

---

## Create task sequence for SharePoint 2013 development image

### Create task sequence - "SharePoint Server 2013 - Development"

1. Open **Deployment Workbench**, expand **Deployment Shares / MDT Build Lab ([\\\\ICEMAN\\MDT-Build\$](\\ICEMAN\MDT-Build$))**, right-click **Task Sequences**, and create a new folder named **Windows Server 2012 R2**.
2. Expand the **Task Sequences** node, right-click the **Windows Server 2012 R2** folder and select **New Task Sequence**.
3. Use the following settings for the **New Task Sequence Wizard**:
   - General Settings
     - Task sequence ID: **SP2013-DEV**
     - Task sequence name: **SharePoint Server 2013 - Development**
     - Task sequence comments: **Windows Server 2012 R2, SQL Server 2014, Visual Studio 2013 with Update 4, Office 2013, and SharePoint Server 2013**
   - Select Template
     - Template: **Standard Server Task Sequence**
   - Select OS
     - **Windows Server 2012 R2 Standard with Update**
   - Specify Product Key
     - **Specify the product key for this operating system.**
     - Product Key: {product key}
   - OS Settings
     - Full Name: **Windows User**
     - Organization: **Technology Toolbox**
     - Internet Explorer Home Page: **about:blank**
   - Admin Password
     - **Use the specified local Administrator password.**
     - Administrator Password: **{password}**

> **Important**
>
> The MSDN version of Windows Server 2012 R2 will prompt to enter a product key (but provide an option to skip this step). It does not honor the SkipProductKey=YES entry in the MDT CustomSettings.ini file.

> **Important**
>
> Windows Server 2012 R2 with Update requires a password to be specified for the Administrator account (unlike Windows 8.1). If an Administrator password is not specified in the task sequence, the Lite Touch Installation will prompt for a password (which must be subsequently be entered manually when completing the actions specified in the task sequence).

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### # Add new files to TFS

```PowerShell
& "C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\TF.exe" add Main /r
```

#### Check-in files

---

```PowerShell
cls
```

### # Copy task sequence customizations (from WS2012-R2 to SP2013-DEV)

```PowerShell
copy '\\ICEMAN\MDT-Build$\Control\WS2012-R2\ts.xml' '\\ICEMAN\MDT-Build$\Control\SP2013-DEV'
```

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### Check-in files

---

### # Identify Windows features for SharePoint 2013

```PowerShell
$requiredFeatures = "NET-WCF-HTTP-Activation45,NET-WCF-TCP-Activation45,NET-WCF-Pipe-Activation45,Net-Framework-Features,Web-Server,Web-WebServer,Web-Common-Http,Web-Static-Content,Web-Default-Doc,Web-Dir-Browsing,Web-Http-Errors,Web-App-Dev,Web-Asp-Net,Web-Net-Ext,Web-ISAPI-Ext,Web-ISAPI-Filter,Web-Health,Web-Http-Logging,Web-Log-Libraries,Web-Request-Monitor,Web-Http-Tracing,Web-Security,Web-Basic-Auth,Web-Windows-Auth,Web-Filtering,Web-Digest-Auth,Web-Performance,Web-Stat-Compression,Web-Dyn-Compression,Web-Mgmt-Tools,Web-Mgmt-Console,Web-Mgmt-Compat,Web-Metabase,Application-Server,AS-Web-Support,AS-TCP-Port-Sharing,AS-WAS-Support,AS-HTTP-Activation,AS-TCP-Activation,AS-Named-Pipes,AS-Net-Framework,WAS,WAS-Process-Model,WAS-NET-Environment,WAS-Config-APIs,Web-Lgcy-Scripting,Windows-Identity-Foundation,Server-Media-Foundation,Xps-Viewer".Split(",")

$allOosRoles = "AD-Certificate,AD-Domain-Services,ADFS-Federation,ADLDS,ADRMS,Application-Server,DHCP,DNS,Fax,FileAndStorage-Services,Hyper-V,NPAS,Print-Services,RemoteAccess,Remote-Desktop-Services,VolumeActivation,Web-Server,WDS,ServerEssentialsRole,UpdateServices".Split(",")

$allOsRoleServices = "ADCS-Cert-Authority,ADCS-Enroll-Web-Pol,ADCS-Enroll-Web-Svc,ADCS-Web-Enrollment,ADCS-Device-Enrollment,ADCS-Online-Cert,ADRMS-Server,ADRMS-Identity,AS-NET-Framework,AS-Ent-Services,AS-Dist-Transaction,AS-WS-Atomic,AS-Incoming-Trans,AS-Outgoing-Trans,AS-TCP-Port-Sharing,AS-Web-Support,AS-WAS-Support,AS-HTTP-Activation,AS-MSMQ-Activation,AS-Named-Pipes,AS-TCP-Activation,File-Services,FS-FileServer,FS-BranchCache,FS-Data-Deduplication,FS-DFS-Namespace,FS-DFS-Replication,FS-Resource-Manager,FS-VSS-Agent,FS-iSCSITarget-Server,iSCSITarget-VSS-VDS,FS-NFS-Service,Storage-Services,FS-SyncShareService,Storage-Services,NPAS-Health,NPAS-Host-Cred,NPAS-Policy-Server,Print-Server,Print-Scan-Server,Print-Internet,Print-LPD-Service,DirectAccess-VPN,Routing,Web-Application-Proxy,RDS-Connection-Broker,RDS-Gateway,RDS-Licensing,RDS-RD-Server,RDS-Virtualization,RDS-Web-Access,Web-Mgmt-Tools,Web-Mgmt-Console,Web-Mgmt-Compat,Web-Metabase,Web-Lgcy-Mgmt-Console,Web-Lgcy-Scripting,Web-WMI,Web-Scripting-Tools,Web-Mgmt-Service,Web-WebServer,Web-Common-Http,Web-Default-Doc,Web-Dir-Browsing,Web-Http-Errors,Web-Static-Content,Web-Http-Redirect,Web-DAV-Publishing,Web-Health,Web-Http-Logging,Web-Custom-Logging,Web-Log-Libraries,Web-ODBC-Logging,Web-Request-Monitor,Web-Http-Tracing,Web-Performance,Web-Stat-Compression,Web-Dyn-Compression,Web-Security,Web-Filtering,Web-Basic-Auth,Web-CertProvider,Web-Client-Auth,Web-Digest-Auth,Web-Cert-Auth,Web-IP-Security,Web-Url-Auth,Web-Windows-Auth,Web-App-Dev,Web-Net-Ext,Web-Net-Ext45,Web-AppInit,Web-ASP,Web-Asp-Net,Web-Asp-Net45,Web-CGI,Web-ISAPI-Ext,Web-ISAPI-Filter,Web-Includes,Web-WebSockets,Web-Ftp-Server,Web-Ftp-Service,Web-Ftp-Ext,Web-WHC,WDS-Deployment,WDS-Transport,UpdateServices-WidDB,UpdateServices-Services,UpdateServices-DB".Split(",")

$allOsFeatures = "NET-Framework-Features,NET-Framework-Core,NET-HTTP-Activation,NET-Non-HTTP-Activ,NET-Framework-45-Features,NET-Framework-45-Core,NET-Framework-45-ASPNET,NET-WCF-Services45,NET-WCF-HTTP-Activation45,NET-WCF-MSMQ-Activation45,NET-WCF-Pipe-Activation45,NET-WCF-TCP-Activation45,NET-WCF-TCP-PortSharing45,BITS,BITS-IIS-Ext,BITS-Compact-Server,BitLocker,BitLocker-NetworkUnlock,BranchCache,NFS-Client,Data-Center-Bridging,Direct-Play,EnhancedStorage,Failover-Clustering,GPMC,InkAndHandwritingServices,Internet-Print-Client,IPAM,ISNS,LPR-Port-Monitor,ManagementOdata,Server-Media-Foundation,MSMQ,MSMQ-Services,MSMQ-Server,MSMQ-Directory,MSMQ-HTTP-Support,MSMQ-Triggers,MSMQ-Multicasting,MSMQ-Routing,MSMQ-DCOM,Multipath-IO,NLB,PNRP,qWave,CMAK,Remote-Assistance,RDC,RSAT,RSAT-Feature-Tools,RSAT-SMTP,RSAT-Feature-Tools-BitLocker,RSAT-Feature-Tools-BitLocker-RemoteAdminTool,RSAT-Feature-Tools-BitLocker-BdeAducExt,RSAT-Bits-Server,RSAT-Clustering,RSAT-Clustering-Mgmt,RSAT-Clustering-PowerShell,RSAT-Clustering-AutomationServer,RSAT-Clustering-CmdInterface,IPAM-Client-Feature,RSAT-NLB,RSAT-SNMP,RSAT-WINS,RSAT-Role-Tools,RSAT-AD-Tools,RSAT-AD-PowerShell,RSAT-ADDS,RSAT-AD-AdminCenter,RSAT-ADDS-Tools,RSAT-NIS,RSAT-ADLDS,RSAT-Hyper-V-Tools,Hyper-V-Tools,Hyper-V-PowerShell,RSAT-RDS-Tools,RSAT-RDS-Gateway,RSAT-RDS-Licensing-Diagnosis-UI,RDS-Licensing-UI,UpdateServices-RSAT,UpdateServices-API,UpdateServices-UI,RSAT-ADCS,RSAT-ADCS-Mgmt,RSAT-Online-Responder,RSAT-ADRMS,RSAT-DHCP,RSAT-DNS-Server,RSAT-Fax,RSAT-File-Services,RSAT-DFS-Mgmt-Con,RSAT-FSRM-Mgmt,RSAT-NFS-Admin,RSAT-CoreFile-Mgmt,RSAT-NPAS,RSAT-Print-Services,RSAT-RemoteAccess,RSAT-RemoteAccess-Mgmt,RSAT-RemoteAccess-PowerShell,RSAT-VA-Tools,WDS-AdminPack,RPC-over-HTTP-Proxy,Simple-TCPIP,FS-SMB1,FS-SMBBW,SMTP-Server,SNMP-Service,SNMP-WMI-Provider,Telnet-Client,Telnet-Server,TFTP-Client,User-Interfaces-Infra,Server-Gui-Mgmt-Infra,Desktop-Experience,Server-Gui-Shell,Biometric-Framework,WFF,Windows-Identity-Foundation,Windows-Internal-Database,PowerShellRoot,DSC-Service,PowerShell,PowerShell-V2,PowerShell-ISE,WindowsPowerShellWebAccess,WAS,WAS-Process-Model,WAS-NET-Environment,WAS-Config-APIs,Search-Service,Windows-Server-Backup,Migration,WindowsStorageManagementService,Windows-TIFF-IFilter,WinRM-IIS-Ext,WINS,Wireless-Networking,WoW64-Support,XPS-Viewer".Split(",")

$selectedOsRoles = ($allOosRoles | ? { $requiredFeatures.Contains($_) }) -join ","

$selectedOsRoleServices = ($allOsRoleServices | ? { $requiredFeatures.Contains($_) }) -join ","

$selectedOsFeatures = ($allOsFeatures | ? { $requiredFeatures.Contains($_) }) -join ","

"<variable name=`"OSRoles`" property=`"OSRoles`">" + $selectedOsRoles + "</variable>"

"<variable name=`"OSRoleServices`" property=`"OSRoleServices`">" + $selectedOsRoleServices + "</variable>"

"<variable name=`"OSFeatures`" property=`"OSFeatures`">" + $selectedOsFeatures + "</variable>"
```

### Configure task sequence - "SharePoint Server 2013 - Development"

Edit the task sequence to include the actions required to install SQL Server 2014, Visual Studio 2013, Office 2013, and SharePoint Server 2013. Temporarily disable the Windows Update actions (for testing purposes).

1. In the **Task Sequences / Windows Server 2012 R2** folder, right-click **SharePoint Server 2013 - Development** and click **Properties**.
2. On the **Task Sequence** tab, configure the following settings:
   1. **State Restore**
      1. In the **Custom Tasks (Pre-Windows Update)** group, select **Install - Microsoft NET Framework 3.5** and modify the action with the following settings:
         1. Name: **Install Windows features for SharePoint 2013**
         2. Select the operating system for which roles are to be installed: **Windows Server 2012 R2**
         3. Select the roles and features that should be installed:
            - Roles
              - **Application Server**
                - **TCP Port Sharing**
                - **Web Server (IIS) Support**
                - **Windows Process Activation Service Support**
                  - **HTTP Activation**
                  - **Named Pipes Activation**
                  - **TCP Activation**
              - **Web Server (IIS)**
                - **Management Tools**
                  - **IIS Management Console**
                  - **IIS 6 Management Compatibility**
                    - **IIS 6 Metabase Compatibility**
                    - **IIS 6 Scripting Tools**
                - **Web Server**
                  - **Common HTTP Features**
                    - **Default Document**
                    - **Directory Browsing**
                    - **HTTP Errors**
                    - **Static Content**
                  - **Health and Diagnostics**
                    - **HTTP Logging**
                    - **Logging Tools**
                    - **Request monitor**
                    - **Tracing**
                  - **Performance**
                    - **Static Content Compression**
                    - **Dynamic Content Compression**
                  - **Security**
                    - **Request Filtering**
                    - **Basic Authentication**
                    - **Digest Authentication**
                    - **Windows Authentication**
                  - **Application Development**
                    - **.NET Extensibility 3.5**
                    - **ASP.NET 3.5**
                    - **ISAPI Extensions**
                    - **ISAPI Filters**
            - Features
              - .NET Framework 4.5 Features
                - WCF Services
                  - **HTTP Activation**
                  - **Named Pipe Activation**
                  - **TCP Activation**
              - **Media Foundation**
              - **Windows Identity Foundation 3.5**
              - **Windows Process Activation Service**
                - **Process Model**
                - **.NET Environment 3.5**
                - **Configuration APIs**
      2. After the **Install Windows features for SharePoint 2013 **action, add a new **Restart computer** action.
      3. Select **Custom Tasks (Pre-Windows Update)** and add a new **Install Application** action with the following settings:
         1. Name: **Install SQL Server 2014 Developer Edition - x64 (Database Engine and Management Tools)**
         2. **Install a single application**
         3. Application to install: **SQL Server 2014 Developer Edition - x64 (Database Engine and Management Tools)**
      4. Select **Custom Tasks (Pre-Windows Update)** and add a new **Install Application** action with the following settings:
         1. Name: **Install Visual Studio 2013 for SP2013 Development**
         2. **Install a single application**
         3. Application to install: **Visual Studio 2013 for SP2013 Development**
      5. Select **Custom Tasks (Pre-Windows Update)** and add a new **Install Application** action with the following settings:
         1. Name: **Install SharePoint Server 2013 with Service Pack 1 - Prerequisites**
         2. **Install a single application**
         3. Application to install: **SharePoint Server 2013 with Service Pack 1 - Prerequisites**
      6. After the **Install SharePoint Server 2013 with Service Pack 1 - Prerequisites **action, add a new **Restart computer** action.
      7. Select **Custom Tasks (Pre-Windows Update)** and add a new **Install Application** action with the following settings:
         1. Name: **Install SharePoint Server 2013 with Service Pack 1**
         2. **Install a single application**
         3. Application to install: **SharePoint Server 2013 with Service Pack 1**
      8. Disable the **Windows Update (Pre-Application Installation)** action.
      9. Disable the **Windows Update (Post-Application Installation)** action.
3. Click **OK**.

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### Check-in files

---

## Build baseline image (SharePoint Server 2013 - Development)

---

**STORM**

### # Create temporary VM to build image (SharePoint Server 2013 - Development)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 14:12</p>
</td>
<td valign='top'>
<p>2015-03-27 14:31</p>
</td>
<td valign='top'>
<p>00:23:32</p>
</td>
<td valign='top'>
<p>3,621,239</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 15:12</p>
</td>
<td valign='top'>
<p>2015-03-27 19:02</p>
</td>
<td valign='top'>
<p>03:53:46</p>
</td>
<td valign='top'>
<p>5,969,687</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 19:26</p>
</td>
<td valign='top'>
<p>2015-03-27 22:19:09</p>
</td>
<td valign='top'>
<p>02:52:28</p>
</td>
<td valign='top'>
<p>5,552,836</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 06:20</p>
</td>
<td valign='top'>
<p>2015-03-28 08:00</p>
</td>
<td valign='top'>
<p>01:44:18</p>
</td>
<td valign='top'>
<p>4,354,499</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 08:20</p>
</td>
<td valign='top'>
<p>2015-03-28 09:26</p>
</td>
<td valign='top'>
<p>01:09:10</p>
</td>
<td valign='top'>
<p>5,219,797</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 10:01</p>
</td>
<td valign='top'>
<p>2015-03-28 12:28</p>
</td>
<td valign='top'>
<p>02:26:43</p>
</td>
<td valign='top'>
<p>7,571,734</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 21:01</p>
</td>
<td valign='top'>
<p>2015-03-29 00:11</p>
</td>
<td valign='top'>
<p>03:21:31</p>
</td>
<td valign='top'>
<p>7,273,045</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
<li>Cleanup before Sysprep</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>SharePoint Server 2013 - Development</p>
</td>
<td valign='top'>
<p>2015-03-30 11:03</p>
</td>
<td valign='top'>
<p>2015-03-30 13:07</p>
</td>
<td valign='top'>
<p>02:04:10</p>
</td>
<td valign='top'>
<p>13,955,744</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>SQL Server 2014</li>
<li>Visual Studio 2013</li>
<li>Office 2013</li>
<li>SharePoint Server 2013</li>
</ul>
</td>
</tr>
</table>

### Update task sequence - "SharePoint Server 2013 - Development"

Edit the task sequence to enable the Windows Update actions.

1. In the **Task Sequences / Windows Server 2012 R2** folder, right-click **SharePoint Server 2013 - Development** and click **Properties**.
2. On the **Task Sequence** tab, configure the following settings:
   1. **State Restore**
      1. Enable the **Windows Update (Pre-Application Installation)** action.
      2. Enable the **Windows Update (Post-Application Installation)** action.
3. Click **OK**.

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### Check-in files

---

## Build baseline image (SharePoint Server 2013 - Development)

---

**STORM**

### # Create temporary VM to build image (SharePoint Server 2013 - Development)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 14:12</p>
</td>
<td valign='top'>
<p>2015-03-27 14:31</p>
</td>
<td valign='top'>
<p>00:23:32</p>
</td>
<td valign='top'>
<p>3,621,239</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 15:12</p>
</td>
<td valign='top'>
<p>2015-03-27 19:02</p>
</td>
<td valign='top'>
<p>03:53:46</p>
</td>
<td valign='top'>
<p>5,969,687</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 19:26</p>
</td>
<td valign='top'>
<p>2015-03-27 22:19:09</p>
</td>
<td valign='top'>
<p>02:52:28</p>
</td>
<td valign='top'>
<p>5,552,836</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 06:20</p>
</td>
<td valign='top'>
<p>2015-03-28 08:00</p>
</td>
<td valign='top'>
<p>01:44:18</p>
</td>
<td valign='top'>
<p>4,354,499</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 08:20</p>
</td>
<td valign='top'>
<p>2015-03-28 09:26</p>
</td>
<td valign='top'>
<p>01:09:10</p>
</td>
<td valign='top'>
<p>5,219,797</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 10:01</p>
</td>
<td valign='top'>
<p>2015-03-28 12:28</p>
</td>
<td valign='top'>
<p>02:26:43</p>
</td>
<td valign='top'>
<p>7,571,734</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 21:01</p>
</td>
<td valign='top'>
<p>2015-03-29 00:11</p>
</td>
<td valign='top'>
<p>03:21:31</p>
</td>
<td valign='top'>
<p>7,273,045</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
<li>Cleanup before Sysprep</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>SharePoint Server 2013 - Development</p>
</td>
<td valign='top'>
<p>2015-03-30 11:03</p>
</td>
<td valign='top'>
<p>2015-03-30 13:07</p>
</td>
<td valign='top'>
<p>02:04:10</p>
</td>
<td valign='top'>
<p>13,955,744</p>
</td>
<td valign='top'>
<ul>
<li>Windows Server 2012 R2 build 6.3.9600.17415</li>
<li>SQL Server 2014</li>
<li>Visual Studio 2013</li>
<li>Office 2013</li>
<li>SharePoint Server 2013</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>SharePoint Server 2013 - Development</p>
</td>
<td valign='top'>
<p>2015-03-30 13:12</p>
</td>
<td valign='top'>
<p>2015-03-30 16:22</p>
</td>
<td valign='top'>
<p>03:10:31</p>
</td>
<td valign='top'>
<p>16,203,536</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>SQL Server 2014</li>
<li>Visual Studio 2013</li>
<li>Office 2013</li>
<li>SharePoint Server 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
</table>

### Add actions to task sequence to cleanup image before SysPrep

1. In the **Task Sequences / Windows Server 2012 R2 **folder, right-click **SharePoint Server 2013 - Development** and click **Properties**.
2. On the **Task Sequence** tab, configure the following settings:
   1. **State Restore**
      1. In the **Custom Tasks (Pre-Windows Update)** group, after the **Install SharePoint Server 2013 with Service Pack 1 - Prerequisites** action, add a new **Run Command Line** action with the following settings:
         1. Name: **Remove SharePoint prerequisite files**
         2. Command line: **PowerShell.exe -Command "& { Remove-Item C:\\PrerequisiteInstallerFiles_SP1 -Recurse -Force }"Note: **I originally attempted to use the following command line...rmdir /S /Q C:\\PrerequisiteInstallerFiles_SP1...but encountered numerous issues (despite adding "1" to the list of success codes -- since rmdir was found to return this value when deleting the folder). Consequently I switched to deleting the folder via PowerShell instead.
      2. After the **Apply Local GPO Package** action, add a new **Group** action with the following setting:
         1. Name: **Cleanup before Sysprep**
      3. In the **Cleanup before Sysprep **group, add a new **Group** action with the following setting:
         1. Name: **Compress the image**
      4. Select **Compress the image **and add a new **Restart computer **action.
      5. Select **Compress the image** and add a new **Install Application** action with the following settings:
         1. Name: **Action - Cleanup before Sysprep**
         2. **Install a single application**
         3. Application to install: **Action - Cleanup before Sysprep**
      6. Select **Compress the image **and add a new **Restart computer **action.
3. Click **OK**.

---

**WOLVERINE**

### # Update files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

C:\NotBackedUp\Public\Toolbox\DiffMerge\DiffMerge.exe \\ICEMAN\MDT-Build$ '.\Main\MDT-Build$'
```

#### # Sync files

```PowerShell
robocopy \\ICEMAN\MDT-Build$ Main\MDT-Build$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### Check-in files

---

## Build baseline image (SharePoint Server 2013 - Development)

---

**STORM**

### # Create temporary VM to build image (SharePoint Server 2013 - Development)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 14:12</p>
</td>
<td valign='top'>
<p>2015-03-27 14:31</p>
</td>
<td valign='top'>
<p>00:23:32</p>
</td>
<td valign='top'>
<p>3,621,239</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 15:12</p>
</td>
<td valign='top'>
<p>2015-03-27 19:02</p>
</td>
<td valign='top'>
<p>03:53:46</p>
</td>
<td valign='top'>
<p>5,969,687</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 19:26</p>
</td>
<td valign='top'>
<p>2015-03-27 22:19:09</p>
</td>
<td valign='top'>
<p>02:52:28</p>
</td>
<td valign='top'>
<p>5,552,836</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 06:20</p>
</td>
<td valign='top'>
<p>2015-03-28 08:00</p>
</td>
<td valign='top'>
<p>01:44:18</p>
</td>
<td valign='top'>
<p>4,354,499</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 08:20</p>
</td>
<td valign='top'>
<p>2015-03-28 09:26</p>
</td>
<td valign='top'>
<p>01:09:10</p>
</td>
<td valign='top'>
<p>5,219,797</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 10:01</p>
</td>
<td valign='top'>
<p>2015-03-28 12:28</p>
</td>
<td valign='top'>
<p>02:26:43</p>
</td>
<td valign='top'>
<p>7,571,734</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 21:01</p>
</td>
<td valign='top'>
<p>2015-03-29 00:11</p>
</td>
<td valign='top'>
<p>03:21:31</p>
</td>
<td valign='top'>
<p>7,273,045</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
<li>Cleanup before Sysprep</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>SharePoint Server 2013 - Development</p>
</td>
<td valign='top'>
<p>2015-03-30 11:03</p>
</td>
<td valign='top'>
<p>2015-03-30 13:07</p>
</td>
<td valign='top'>
<p>02:04:10</p>
</td>
<td valign='top'>
<p>13,955,744</p>
</td>
<td valign='top'>
<ul>
<li>Windows Server 2012 R2 build 6.3.9600.17415</li>
<li>SQL Server 2014</li>
<li>Visual Studio 2013</li>
<li>Office 2013</li>
<li>SharePoint Server 2013</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>SharePoint Server 2013 - Development</p>
</td>
<td valign='top'>
<p>2015-03-30 13:12</p>
</td>
<td valign='top'>
<p>2015-03-30 16:22</p>
</td>
<td valign='top'>
<p>03:10:31</p>
</td>
<td valign='top'>
<p>16,203,536</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>SQL Server 2014</li>
<li>Visual Studio 2013</li>
<li>Office 2013</li>
<li>SharePoint Server 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>SharePoint Server 2013 - Development</p>
</td>
<td valign='top'>
<p>2015-04-06 13:56</p>
</td>
<td valign='top'>
<p>2015-04-06 17:23</p>
</td>
<td valign='top'>
<p>03:26:46</p>
</td>
<td valign='top'>
<p>15,361,376</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>SQL Server 2014</li>
<li>Visual Studio 2013</li>
<li>Office 2013</li>
<li>SharePoint Server 2013</li>
<li>Latest patches installed</li>
<li>Cleanup image before Sysprep</li>
</ul>
</td>
</tr>
</table>

## Add custom images

### Add custom image: Windows 8.1 Enterprise x64 - Baseline

1. Open **Deployment Workbench**, expand the **Deployment Shares** node, expand **MDT Production ([\\\\ICEMAN\\MDT-Deploy\$](\\ICEMAN\MDT-Deploy$))**, right-click the **Operating Systems** node, and create a folder named **Windows 8.1**.
2. Right-click the **Windows 8.1** folder and select **Import Operating System**.
3. On the **OS Type** page, select **Custom image file** and click **Next**.
4. On the **Image** page:
   1. Click **Browse...**
      1. Browse to **[\\\\ICEMAN\\MDT-Build\$\\Captures](\\ICEMAN\MDT-Build$\Captures).**
      2. Select the Windows 8.1 Enterprise x64 image, and click **Open**.
   2. Click** Next**.
5. On the **Setup** page:
   1. Select **Copy Windows 7, Windows Server 2008 R2, or later setup files from the specified path.**
   2. In the **Setup source directory** text box, type **[\\\\ICEMAN\\MDT-Build\$\\Operating Systems\\W81Ent-x64-Update](\\ICEMAN\MDT-Build$\Operating Systems\W81Ent-x64-Update)**.
   3. Click **Next**.
6. On the **Destination** page, in the **Destination directory name** text box, type **W81Ent-x64-Update**, and click **Next**.
7. On the **Summary** page, click **Next**.
8. Wait for the operating system to be imported and then click **Finish**.
9. Double-click the new operating system name in the **Operating Systems / Windows 8.1** folder and change the name to **Windows 8.1 Enterprise x64 - Baseline**.

### Add custom image: Windows Server 2012 R2 Standard - Baseline

1. Open **Deployment Workbench**, expand the **Deployment Shares** node, expand **MDT Production ([\\\\ICEMAN\\MDT-Deploy\$](\\ICEMAN\MDT-Deploy$))**, right-click the **Operating Systems** node, and create a folder named **Windows Server 2012 R2**.
2. Right-click the **Windows Server 2012 R2** folder and select **Import Operating System**.
3. On the **OS Type** page, select **Custom image file** and click **Next**.
4. On the **Image** page:
   1. Click **Browse...**
      1. Browse to **[\\\\ICEMAN\\MDT-Build\$\\Captures](\\ICEMAN\MDT-Build$\Captures).**
      2. Select the Windows Server 2012 R2 image, and click **Open**.
   2. Click** Next**.
5. On the **Setup** page:
   1. Select **Copy Windows 7, Windows Server 2008 R2, or later setup files from the specified path.**
   2. In the **Setup source directory** text box, type **[\\\\ICEMAN\\MDT-Build\$\\Operating Systems\\WS2012-R2-Update](\\ICEMAN\MDT-Build$\Operating Systems\WS2012-R2-Update)**.
   3. Click **Next**.
6. On the **Destination** page, in the **Destination directory name** text box, type **WS2012-R2-Update**, and click **Next**.
7. On the **Summary** page, click **Next**.
8. Wait for the operating system to be imported and then click **Finish**.
9. Double-click the new operating system name in the **Operating Systems / Windows Server 2012 R2** folder and change the name to **Windows Server 2012 R2 Standard - Baseline**.

### Add custom image: SharePoint 2013 - Development

1. Open **Deployment Workbench**, expand the **Deployment Shares** node, expand **MDT Production ([\\\\ICEMAN\\MDT-Deploy\$](\\ICEMAN\MDT-Deploy$))**, expand the **Operating Systems** node, right-click the **Windows Server 2012 R2** folder and select **Import Operating System**.
2. On the **OS Type** page, select **Custom image file** and click **Next**.
3. On the **Image** page:
   1. Click **Browse...**
      1. Browse to **[\\\\ICEMAN\\MDT-Build\$\\Captures](\\ICEMAN\MDT-Build$\Captures).**
      2. Select the SharePoint 2013 development image, and click **Open**.
   2. Click** Next**.
4. On the **Setup** page, ensure **Setup files are not needed** is selected, and click **Next**.
5. On the **Destination** page, in the **Destination directory name** text box, type **SP2013-DEV**, and click **Next**.
6. On the **Summary** page, click **Next**.
7. Wait for the operating system to be imported and then click **Finish**.
8. Double-click the new operating system name in the **Operating Systems / Windows Server 2012 R2** folder and change the name to **SharePoint 2013 - Development**.

---

**WOLVERINE**

### # Baseline files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

robocopy \\ICEMAN\MDT-Deploy$ Main\MDT-Deploy$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### # Add files to TFS

```PowerShell
& "C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\TF.exe" add Main /r
```

##### Check-in files

---

## Create task sequences for deployments

### Create task sequence - "Windows 8.1 Enterprise x64"

1. Open **Deployment Workbench**, expand **Deployment Shares / MDT Production ([\\\\ICEMAN\\MDT-Deploy\$](\\ICEMAN\MDT-Deploy$))**, right-click **Task Sequences**, and create a new folder named **Windows 8.1**.
2. Expand the **Task Sequences** node, right-click the **Windows 8.1** folder and select **New Task Sequence**.
3. Use the following settings for the **New Task Sequence Wizard**:
   - General Settings
     - Task sequence ID: **W81Ent-x64**
     - Task sequence name: **Windows 8.1 Enterprise x64**
     - Task sequence comments: **Production image**
   - Select Template
     - Template: **Standard Client Task Sequence**
   - Select OS
     - **Windows 8.1 Enterprise x64 - Baseline**
   - Specify Product Key
     - **Do not specify a product key at this time.**
   - OS Settings
     - Full Name: **Windows User**
     - Organization: **Technology Toolbox**
     - Internet Explorer Home Page: **about:blank**
   - Admin Password
     - **Do not specify an Administrator password at this time.**

### Create task sequence - "Windows Server 2012 R2 Standard"

1. Open **Deployment Workbench**, expand **Deployment Shares / MDT Build Lab ([\\\\ICEMAN\\MDT-Deploy\$](\\ICEMAN\MDT-Deploy$))**, right-click **Task Sequences**, and create a new folder named **Windows Server 2012 R2**.
2. Expand the **Task Sequences** node, right-click the **Windows Server 2012 R2** folder and select **New Task Sequence**.
3. Use the following settings for the **New Task Sequence Wizard**:
   - General Settings
     - Task sequence ID: **WS2012-R2**
     - Task sequence name: **Windows Server 2012 R2 Standard**
     - Task sequence comments: **Production image**
   - Select Template
     - Template: **Standard Server Task Sequence**
   - Select OS
     - **Windows Server 2012 R2 Standard - Baseline**
   - Specify Product Key
     - **Specify the product key for this operating system.**
     - Product Key: {product key}
   - OS Settings
     - Full Name: **Windows User**
     - Organization: **Technology Toolbox**
     - Internet Explorer Home Page: **about:blank**
   - Admin Password
     - **Use the specified local Administrator password.**
     - Administrator Password: **{password}**

> **Important**
>
> The MSDN version of Windows Server 2012 R2 will prompt to enter a product key (but provide an option to skip this step). It does not honor the SkipProductKey=YES entry in the MDT CustomSettings.ini file.

> **Important**
>
> Windows Server 2012 R2 with Update requires a password to be specified for the Administrator account (unlike Windows 8.1). If an Administrator password is not specified in the task sequence, the Lite Touch Installation will prompt for a password (which must be subsequently be entered manually when completing the actions specified in the task sequence).

### Create task sequence - "SharePoint Server 2013 - Development"

1. Open **Deployment Workbench**, expand **Deployment Shares / MDT Build Lab ([\\\\ICEMAN\\MDT-Deploy\$](\\ICEMAN\MDT-Deploy$))**, expand **Task Sequences**, right-click the **Windows Server 2012 R2** folder and select **New Task Sequence**.
2. Use the following settings for the **New Task Sequence Wizard**:
   - General Settings
     - Task sequence ID: **SP2013-DEV**
     - Task sequence name: **SharePoint Server 2013 - Development**
     - Task sequence comments: **Windows Server 2012 R2, SQL Server 2014, Visual Studio 2013 with Update 4, Office 2013, and SharePoint Server 2013**
   - Select Template
     - Template: **Standard Server Task Sequence**
   - Select OS
     - **SharePoint 2013 - Development**
   - Specify Product Key
     - **Specify the product key for this operating system.**
     - Product Key: {product key}
   - OS Settings
     - Full Name: **Windows User**
     - Organization: **Technology Toolbox**
     - Internet Explorer Home Page: **about:blank**
   - Admin Password
     - **Use the specified local Administrator password.**
     - Administrator Password: **{password}**

> **Important**
>
> The MSDN version of Windows Server 2012 R2 will prompt to enter a product key (but provide an option to skip this step). It does not honor the SkipProductKey=YES entry in the MDT CustomSettings.ini file.

> **Important**
>
> Windows Server 2012 R2 with Update requires a password to be specified for the Administrator account (unlike Windows 8.1). If an Administrator password is not specified in the task sequence, the Lite Touch Installation will prompt for a password (which must be subsequently be entered manually when completing the actions specified in the task sequence).

---

**WOLVERINE**

### # Baseline files in TFS

```PowerShell
cd C:\NotBackedUp\TechnologyToolbox\Infrastructure

robocopy \\ICEMAN\MDT-Deploy$ Main\MDT-Deploy$ /E /XD Applications Backup Boot Captures Logs "Operating Systems" Servicing Tools
```

#### # Add files to TFS

```PowerShell
& "C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\TF.exe" add Main /r
```

##### Check-in files

---

```PowerShell
cls
```

## # Build VM (Windows 8.1 Enterprise x64)

### # Copy MDT boot images to \\\\ICEMAN\\Products\\Microsoft

```PowerShell
robocopy '\\ICEMAN\MDT-Build$\Boot' '\\ICEMAN\Products\Microsoft' *.iso
robocopy '\\ICEMAN\MDT-Deploy$\Boot' '\\ICEMAN\Products\Microsoft' *.iso
```

---

**STORM**

### # Create temporary VM to build VM (Windows 8.1 Enterprise x64)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Production-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>VHD Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64</p>
</td>
<td valign='top'>
<p>2015-04-08 08:46</p>
</td>
<td valign='top'>
<p>2015-04-08 09:16</p>
</td>
<td valign='top'>
<p>00:30:20</p>
</td>
<td valign='top'>
<p>17,764,352</p>
</td>
<td valign='top'>
<ul>
<li>Image: W81ENT-X64_3-28-2015-9-02-25-PM.wim</li>
<li>Additional applications:
<ul>
<li>Adobe Reader 8.3.1</li>
<li>SharePoint Designer 2013 with Service Pack 1 - x86</li>
<li>Google Chrome</li>
<li>Mozilla Firefox 36.0</li>
<li>Mozilla Thunderbird 31.3.0</li>
</ul>
</li>
<li>No additional patches installed</li>
</ul>
</td>
</tr>
</table>

#### Add action to task sequence to create native images for .NET assemblies

1. Open **Deployment Workbench**, expand **Deployment Shares / MDT Production ([\\\\ICEMAN\\MDT-Deploy\$](\\ICEMAN\MDT-Deploy$)) / Task Sequences / Windows 8.1 **folder, right-click **Windows 8.1 Enterprise x64 **and click **Properties**.
2. On the **Task Sequence** tab, configure the following settings:
   1. **State Restore**
      1. In the **Custom Tasks** group, add a new **Run Command Line** action with the following settings:
         1. Name: **Create native images for .NET assemblies**
         2. Command line: **PowerShell.exe -Command "Get-ChildItem \$env:SystemRoot\\Microsoft.NET\\Ngen.exe -Recurse | % { & \$\_ executeQueuedItems }"**
3. Click **OK**.

---

**STORM**

### # Create temporary VM to build VM (Windows 8.1 Enterprise x64)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Production-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>VHD Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64</p>
</td>
<td valign='top'>
<p>2015-04-08 08:46</p>
</td>
<td valign='top'>
<p>2015-04-08 09:16</p>
</td>
<td valign='top'>
<p>00:30:20</p>
</td>
<td valign='top'>
<p>17,764,352</p>
</td>
<td valign='top'>
<ul>
<li>Image: W81ENT-X64_3-28-2015-9-02-25-PM.wim</li>
<li>Additional applications:
<ul>
<li>Adobe Reader 8.3.1</li>
<li>SharePoint Designer 2013 with Service Pack 1 - x86</li>
<li>Google Chrome</li>
<li>Mozilla Firefox 36.0</li>
<li>Mozilla Thunderbird 31.3.0</li>
</ul>
</li>
<li>No additional patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64</p>
</td>
<td valign='top'>
<p>2015-04-08 09:29</p>
</td>
<td valign='top'>
<p>2015-04-08 10:04</p>
</td>
<td valign='top'>
<p>00:34:45</p>
</td>
<td valign='top'>
<p>18,288,640</p>
</td>
<td valign='top'>
<ul>
<li>Image: W81ENT-X64_3-28-2015-9-02-25-PM.wim</li>
<li>Additional applications:
<ul>
<li>Adobe Reader 8.3.1</li>
<li>SharePoint Designer 2013 with Service Pack 1 - x86</li>
<li>Google Chrome</li>
<li>Mozilla Firefox 36.0</li>
<li>Mozilla Thunderbird 31.3.0</li>
</ul>
</li>
<li>No additional patches installed</li>
<li>Create native images for .NET assemblies</li>
</ul>
</td>
</tr>
</table>

#### Enable action to run Windows Update after installing applications

1. Open **Deployment Workbench**, expand **Deployment Shares / MDT Production ([\\\\ICEMAN\\MDT-Deploy\$](\\ICEMAN\MDT-Deploy$)) / Task Sequences / Windows 8.1 **folder, right-click **Windows 8.1 Enterprise x64 **and click **Properties**.
2. On the **Task Sequence** tab, configure the following settings:
   1. **State Restore**
      1. Enable the **Windows Update (Post-Application Installation)** action.
3. Click **OK**.

## Install System Center 2012 R2 Configuration Manager Toolkit (for log file viewer)

**System Center 2012 R2 Configuration Manager Toolkit**\
From <[https://www.microsoft.com/en-us/download/details.aspx?id=36213](https://www.microsoft.com/en-us/download/details.aspx?id=36213)>

## Build baseline Windows 7 image

---

**STORM**

### # Create temporary VM to build baseline image (Windows 7 Ultimate x86 - Baseline)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 14:12</p>
</td>
<td valign='top'>
<p>2015-03-27 14:31</p>
</td>
<td valign='top'>
<p>00:23:32</p>
</td>
<td valign='top'>
<p>3,621,239</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 15:12</p>
</td>
<td valign='top'>
<p>2015-03-27 19:02</p>
</td>
<td valign='top'>
<p>03:53:46</p>
</td>
<td valign='top'>
<p>5,969,687</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 19:26</p>
</td>
<td valign='top'>
<p>2015-03-27 22:19:09</p>
</td>
<td valign='top'>
<p>02:52:28</p>
</td>
<td valign='top'>
<p>5,552,836</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 06:20</p>
</td>
<td valign='top'>
<p>2015-03-28 08:00</p>
</td>
<td valign='top'>
<p>01:44:18</p>
</td>
<td valign='top'>
<p>4,354,499</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 08:20</p>
</td>
<td valign='top'>
<p>2015-03-28 09:26</p>
</td>
<td valign='top'>
<p>01:09:10</p>
</td>
<td valign='top'>
<p>5,219,797</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 10:01</p>
</td>
<td valign='top'>
<p>2015-03-28 12:28</p>
</td>
<td valign='top'>
<p>02:26:43</p>
</td>
<td valign='top'>
<p>7,571,734</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 21:01</p>
</td>
<td valign='top'>
<p>2015-03-29 00:11</p>
</td>
<td valign='top'>
<p>03:21:31</p>
</td>
<td valign='top'>
<p>7,273,045</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
<li>Cleanup before Sysprep</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>SharePoint Server 2013 - Development</p>
</td>
<td valign='top'>
<p>2015-03-30 11:03</p>
</td>
<td valign='top'>
<p>2015-03-30 13:07</p>
</td>
<td valign='top'>
<p>02:04:10</p>
</td>
<td valign='top'>
<p>13,955,744</p>
</td>
<td valign='top'>
<ul>
<li>Windows Server 2012 R2 build 6.3.9600.17415</li>
<li>SQL Server 2014</li>
<li>Visual Studio 2013</li>
<li>Office 2013</li>
<li>SharePoint Server 2013</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>SharePoint Server 2013 - Development</p>
</td>
<td valign='top'>
<p>2015-03-30 13:12</p>
</td>
<td valign='top'>
<p>2015-03-30 16:22</p>
</td>
<td valign='top'>
<p>03:10:31</p>
</td>
<td valign='top'>
<p>16,203,536</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>SQL Server 2014</li>
<li>Visual Studio 2013</li>
<li>Office 2013</li>
<li>SharePoint Server 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>SharePoint Server 2013 - Development</p>
</td>
<td valign='top'>
<p>2015-04-06 13:56</p>
</td>
<td valign='top'>
<p>2015-04-06 17:23</p>
</td>
<td valign='top'>
<p>03:26:46</p>
</td>
<td valign='top'>
<p>15,361,376</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>SQL Server 2014</li>
<li>Visual Studio 2013</li>
<li>Office 2013</li>
<li>SharePoint Server 2013</li>
<li>Latest patches installed</li>
<li>Cleanup image before Sysprep</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 7 Ultimate x86 - Baseline</p>
</td>
<td valign='top'>
<p>2015-04-09 08:33</p>
</td>
<td valign='top'>
<p>2015-04-09 08:54</p>
</td>
<td valign='top'>
<p>00:20:14</p>
</td>
<td valign='top'>
<p>2,069,388</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.1.7601.17514</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
</table>

## Customize task sequences for building Windows 7 baseline images

### Configure task sequence - "Windows 7 Ultimate x86 - Baseline"

Edit the task sequences to update the reference images with the latest updates from WSUS, copy Toolbox content from ICEMAN, install .NET Framework 3.5, install Office 2013, and cleanup the image before Sysprep.

1. In the **Task Sequences / Windows 7** folder, right-click **Windows 7 Ultimate x86 - Baseline **and click **Properties**.
2. On the **Task Sequence** tab, configure the following settings:
   1. **State Restore**
      1. Enable the **Windows Update (Pre-Application Installation)** action.
      2. Enable the **Windows Update (Post-Application Installation)** action.
      3. After the **Tattoo** action, add a new **Group** action with the following setting:
         1. Name: **Custom Tasks (Pre-Windows Update)**
      4. After the **Windows Update (Post-Application Installation)** action, rename **Custom Tasks** to **Custom Tasks (Post-Windows Update)**.
      5. Select **Custom Tasks (Pre-Windows Update)** and add a new **Run Command Line **action with the following settings:
         1. Name: **Copy Toolbox content from ICEMAN**
         2. Command line: **robocopy [\\\\ICEMAN\\Public\\Toolbox](\\ICEMAN\Public\Toolbox) C:\\NotBackedUp\\Public\\Toolbox /E**
         3. Success codes: **0 3010 1 2 3 4 5 6 7 8 16**
      6. After the **Copy Toolbox content from ICEMAN** action, add a new **Install Roles and Features** action with the following settings:
         1. Name: **Install Microsoft NET Framework 3.5.1**
         2. Select the operating system for which roles are to be installed: **Windows 7**
         3. Select the roles and features that should be installed: **Microsoft .NET Framework 3.5.1**
      7. After the **Install Microsoft NET Framework 3.5.1** action, add a new **Install Application** action with the following settings:
         1. Name: **Install Microsoft Office 2013 Professional Plus - x86**
         2. **Install a single application**
         3. Application to install: **Microsoft Office 2013 Professional Plus - x86**
      8. After the **Install Microsoft Office 2013 Professional Plus - x86 **action, add a new **Restart computer** action.
      9. Select **Install Applications **and add a new **Run Command Line **action with the following settings:
         1. Name: **Suspend**
         2. Command line: **cscript.exe "%SCRIPTROOT%\\LTISuspend.wsf"**
         3. Disable this step:** Yes (checked)**
      10. After the **Apply Local GPO Package** action, add a new **Group** action with the following setting:
          1. Name: **Cleanup before Sysprep**
      11. In the **Cleanup before Sysprep **group, add a new **Group** action with the following setting:
          1. Name: **Compress the image**
      12. Select **Compress the image **and add a new **Restart computer **action.
      13. Select **Compress the image** and add a new **Install Application** action with the following settings:
          1. Name: **Action - Cleanup before Sysprep**
          2. **Install a single application**
          3. Application to install: **Action - Cleanup before Sysprep**
      14. After the **Action - Cleanup before Sysprep** action, add a new **Restart computer **action.
3. Click **OK**.

> **Note**
>
> The reason for adding the applications after the Tattoo action but before running Windows Update is simply to save time during the deployment. This way we can add all applications that will upgrade some of the built-in components and avoid unnecessary updating.

### Configure task sequence - "Windows 7 Ultimate x64 - Baseline"

Repeat the steps in the previous section for the **Windows 7 Ultimate x64 - Baseline **task sequence.

## Build baseline Windows 7 image

---

**STORM**

### # Create temporary VM to build baseline image (Windows 7 Ultimate x86 - Baseline)

```PowerShell
& 'C:\NotBackedUp\Public\Toolbox\PowerShell\Create Temporary VM.ps1' `
    -IsoPath \\ICEMAN\Products\Microsoft\MDT-Build-x86.iso -Force
```

---

<table>
<thead>
<th>
<p><strong>Task Sequence</strong></p>
</th>
<th>
<p><strong>Start</strong></p>
</th>
<th>
<p><strong>End</strong></p>
</th>
<th>
<p><strong>Duration[HH:MM:SS]</strong></p>
</th>
<th>
<p><strong>Image Size [KB]</strong></p>
</th>
<th>
<p><strong>Comments</strong></p>
</th>
</thead>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 12:22</p>
</td>
<td valign='top'>
<p>2015-03-27 12:45</p>
</td>
<td valign='top'>
<p>00:23:19</p>
</td>
<td valign='top'>
<p>3,250,322</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 14:12</p>
</td>
<td valign='top'>
<p>2015-03-27 14:31</p>
</td>
<td valign='top'>
<p>00:23:32</p>
</td>
<td valign='top'>
<p>3,621,239</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 15:12</p>
</td>
<td valign='top'>
<p>2015-03-27 19:02</p>
</td>
<td valign='top'>
<p>03:53:46</p>
</td>
<td valign='top'>
<p>5,969,687</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-27 19:26</p>
</td>
<td valign='top'>
<p>2015-03-27 22:19:09</p>
</td>
<td valign='top'>
<p>02:52:28</p>
</td>
<td valign='top'>
<p>5,552,836</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17031</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 06:20</p>
</td>
<td valign='top'>
<p>2015-03-28 08:00</p>
</td>
<td valign='top'>
<p>01:44:18</p>
</td>
<td valign='top'>
<p>4,354,499</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows Server 2012 R2 Standard - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 08:20</p>
</td>
<td valign='top'>
<p>2015-03-28 09:26</p>
</td>
<td valign='top'>
<p>01:09:10</p>
</td>
<td valign='top'>
<p>5,219,797</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 10:01</p>
</td>
<td valign='top'>
<p>2015-03-28 12:28</p>
</td>
<td valign='top'>
<p>02:26:43</p>
</td>
<td valign='top'>
<p>7,571,734</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 8.1 Enterprise x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-03-28 21:01</p>
</td>
<td valign='top'>
<p>2015-03-29 00:11</p>
</td>
<td valign='top'>
<p>03:21:31</p>
</td>
<td valign='top'>
<p>7,273,045</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
<li>Cleanup before Sysprep</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>SharePoint Server 2013 - Development</p>
</td>
<td valign='top'>
<p>2015-03-30 11:03</p>
</td>
<td valign='top'>
<p>2015-03-30 13:07</p>
</td>
<td valign='top'>
<p>02:04:10</p>
</td>
<td valign='top'>
<p>13,955,744</p>
</td>
<td valign='top'>
<ul>
<li>Windows Server 2012 R2 build 6.3.9600.17415</li>
<li>SQL Server 2014</li>
<li>Visual Studio 2013</li>
<li>Office 2013</li>
<li>SharePoint Server 2013</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>SharePoint Server 2013 - Development</p>
</td>
<td valign='top'>
<p>2015-03-30 13:12</p>
</td>
<td valign='top'>
<p>2015-03-30 16:22</p>
</td>
<td valign='top'>
<p>03:10:31</p>
</td>
<td valign='top'>
<p>16,203,536</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>SQL Server 2014</li>
<li>Visual Studio 2013</li>
<li>Office 2013</li>
<li>SharePoint Server 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>SharePoint Server 2013 - Development</p>
</td>
<td valign='top'>
<p>2015-04-06 13:56</p>
</td>
<td valign='top'>
<p>2015-04-06 17:23</p>
</td>
<td valign='top'>
<p>03:26:46</p>
</td>
<td valign='top'>
<p>15,361,376</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.3.9600.17415</li>
<li>SQL Server 2014</li>
<li>Visual Studio 2013</li>
<li>Office 2013</li>
<li>SharePoint Server 2013</li>
<li>Latest patches installed</li>
<li>Cleanup image before Sysprep</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 7 Ultimate x86 - Baseline</p>
</td>
<td valign='top'>
<p>2015-04-09 08:33</p>
</td>
<td valign='top'>
<p>2015-04-09 08:54</p>
</td>
<td valign='top'>
<p>00:20:14</p>
</td>
<td valign='top'>
<p>2,069,388</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.1.7601.17514</li>
<li>No patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 7 Ultimate x86 - Baseline</p>
</td>
<td valign='top'>
<p>2015-04-09 16:54</p>
</td>
<td valign='top'>
<p>2015-04-09 19:07</p>
</td>
<td valign='top'>
<p>02:13:20</p>
</td>
<td valign='top'>
<p>5,641,546</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.1.7601.17514</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 7 Ultimate x86 - Baseline</p>
</td>
<td valign='top'>
<p>2015-04-10 06:23</p>
</td>
<td valign='top'>
<p>2015-04-10 10:17</p>
</td>
<td valign='top'>
<p>03:53:28</p>
</td>
<td valign='top'>
<p>5,878,544</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.1.7601.17514</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 7 Ultimate x86 - Baseline</p>
</td>
<td valign='top'>
<p>2015-04-10 15:51</p>
</td>
<td valign='top'>
<p>2015-04-10 19:33</p>
</td>
<td valign='top'>
<p>03:42:12</p>
</td>
<td valign='top'>
<p>5,882,811</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.1.7601.17514</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
<tr>
<td valign='top'>
<p>Windows 7 Ultimate x64 - Baseline</p>
</td>
<td valign='top'>
<p>2015-04-10 20:04</p>
</td>
<td valign='top'>
<p>2015-04-11 03:01</p>
</td>
<td valign='top'>
<p>06:57:08</p>
</td>
<td valign='top'>
<p>7,699,484</p>
</td>
<td valign='top'>
<ul>
<li>Build 6.1.7601.17514</li>
<li>Office 2013</li>
<li>Latest patches installed</li>
</ul>
</td>
</tr>
</table>

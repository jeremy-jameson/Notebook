# Windows Server 2012

Thursday, November 15, 2012
9:08 PM

**Windows Server 2012 "Early Experts" Challenge**\
Pasted from <[http://blogs.technet.com/b/keithmayer/p/earlyexpertws12.aspx#.UaI6_5yTmwE](http://blogs.technet.com/b/keithmayer/p/earlyexpertws12.aspx#.UaI6_5yTmwE)>

**Deploy Roaming User Profiles**\
Pasted from <[http://technet.microsoft.com/en-us/library/jj649079.aspx](http://technet.microsoft.com/en-us/library/jj649079.aspx)>

**Right-size IT Budgets with "Storage Spaces" in Windows Server 2012 - 31 Days of Favorite Features in #WinServ 2012 ( Part 6 of 31 )**\
Pasted from <[http://blogs.technet.com/b/keithmayer/archive/2012/10/06/optimize-it-budgets-with-storage-spaces-in-windows-server-2012-31-days-of-favorite-features-part-6-of-31.aspx#.UaIB1ZyTmwE](http://blogs.technet.com/b/keithmayer/archive/2012/10/06/optimize-it-budgets-with-storage-spaces-in-windows-server-2012-31-days-of-favorite-features-part-6-of-31.aspx#.UaIB1ZyTmwE)>

S**tep-by-Step: Building a Windows Server 2012 Storage Server in the Cloud with Windows Azure**\
Pasted from <[http://blogs.technet.com/b/keithmayer/archive/2013/01/24/step-by-step-building-a-windows-server-2012-storage-server-in-the-cloud-with-windows-azure.aspx#.UaICgpyTmwE](http://blogs.technet.com/b/keithmayer/archive/2013/01/24/step-by-step-building-a-windows-server-2012-storage-server-in-the-cloud-with-windows-azure.aspx#.UaICgpyTmwE)>

## Turn GUI on

### References

**How To Convert Windows Server 2012 Standard GUI Interface to Windows Server 2012 Core interface and Vice Versa..**\
Pasted from <[http://forums.mydigitallife.info/threads/40304-How-To-Convert-Server-2012-Core-Installation-into-Full-GUI-Server-and-viceversa](http://forums.mydigitallife.info/threads/40304-How-To-Convert-Server-2012-Core-Installation-into-Full-GUI-Server-and-viceversa)>

**Add a GUI to Server Core 2012 and Overcoming Error: 0x800f0906**\
Pasted from <[http://blog.ittoby.com/2013/01/add-gui-to-server-core-2012-and.html](http://blog.ittoby.com/2013/01/add-gui-to-server-core-2012-and.html)>

```PowerShell
$imagePath = "\\iceman\Products\Microsoft\Windows Server 2012 R2\" `
    + "en_windows_server_2012_r2_x64_dvd_2707946.iso"

$imageDriveLetter = (Mount-DiskImage -ImagePath $imagePath -PassThru |
    Get-Volume).DriveLetter

Get-WindowsImage -ImagePath ($imageDriveLetter + ":\sources\install.wim")

Install-WindowsFeature `
    Server-Gui-Mgmt-Infra,Server-Gui-Shell `
    -Source ("wim:" + $imageDriveLetter + ":\sources\install.wim:2"), `
         "\\ICEMAN\Products\Microsoft\Windows Server 2012 R2\Sources\SxS" `
    -Restart
```

Ended up having to create a group policy to disable WSUS (**Disable intranet Windows Update location**)

…\
gpupdate /force

# Removing a network adapter in Server Core

Thursday, January 09, 2014\
10:09 AM

## Remove network adapter

After removing the Intel network adapter from the second PCI slot, the remaining network adapter had "#2" in the interface description (i.e. "Intel(R) Gigabit CT Desktop Adapter #2").

**"...uninstall the ghosted network adapter from the registry..."**\
Pasted from <[http://support.microsoft.com/kb/269155](http://support.microsoft.com/kb/269155)>

**The DevCon command-line utility functions as an alternative to Device Manager**\
Pasted from <[http://support.microsoft.com/kb/311272](http://support.microsoft.com/kb/311272)>

```PowerShell
C:\NotBacked\Public\Toolbox\DevCon\DevCon.exe listclass net

C:\NotBacked\Public\Toolbox\DevCon\DevCon.exe findall =net

C:\NotBackedUp\Public\Toolbox\DevCon\DevCon.exe -r remove "@PCI\VEN_8086&DEV_10D3&SUBSYS_A01F8086&REV_00\4&1E036870&0&0048"

C:\NotBackedUp\Public\Toolbox\DevCon\DevCon.exe -r remove "@PCI\VEN_8086&DEV_10D3&SUBSYS_A01F8086&REV_00\4&19B43EC8&0&0038"

C:\NotBackedUp\Public\Toolbox\DevCon\DevCon.exe -r remove "@SWD\IP_TUNNEL_VBUS\ISATAP_0"

C:\NotBackedUp\Public\Toolbox\DevCon\DevCon.exe -r remove "@SWD\IP_TUNNEL_VBUS\ISATAP_1"

C:\NotBackedUp\Public\Toolbox\DevCon\DevCon.exe -r remove "@SWD\IP_TUNNEL_VBUS\ISATAP_2"

C:\NotBackedUp\Public\Toolbox\DevCon\DevCon.exe -r remove "@SWD\IP_TUNNEL_VBUS\ISATAP_3"

C:\NotBacked\Public\Toolbox\DevCon\DevCon.exe rescan

Get-NetAdapter -Physical

Get-NetAdapter -InterfaceDescription "Intel(R) Gigabit CT Desktop Adapter" |
    Rename-NetAdapter -NewName "iSCSI 1 - 10.1.10.x"
```

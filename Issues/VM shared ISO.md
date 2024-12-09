# VM shared ISO

Monday, February 17, 2014\
8:16 AM

**How to Enable Shared ISO Images for Hyper-V Virtual Machines in VMM**\
Pasted from <[http://technet.microsoft.com/en-us/library/ee340124.aspx](http://technet.microsoft.com/en-us/library/ee340124.aspx)>

**Access denied error when configuring shared ISO feature on a VM using System Center Virtual Machine Manager 2008**\
Pasted from <[http://support.microsoft.com/kb/2285882](http://support.microsoft.com/kb/2285882)>

**Troubleshooting ISO file sharing in System Center Virtual Machine Manager 2012 SP1**\
Pasted from <[http://morgansimonsen.wordpress.com/2013/08/30/troubleshooting-iso-file-sharing-in-system-center-virtual-machine-manager-2012-sp1/](http://morgansimonsen.wordpress.com/2013/08/30/troubleshooting-iso-file-sharing-in-system-center-virtual-machine-manager-2012-sp1/)>

## REM RDP to console

```Console
mstsc.exe /console /v:BEAST
```

## REM Start network capture

```Console
mkdir C:\NotBackedUp\Temp\Captures

netsh trace start capture=yes tracefile=C:\NotBackedUp\Temp\Captures\%COMPUTERNAME%.etl maxsize=300 overwrite=yes
```

## REM Start command prompt as SYSTEM

```Console
C:\NotBackedUp\Public\Toolbox\Sysinternals\PsExec.exe -i -s -d cmd
```

## REM Clear name resolution cache and Kerberos tickets

### REM Clear DNS name cache

```Console
IPConfig /FlushDNS
```

### REM Clear NetBIOS name cache

```Console
NBTStat -R
```

### REM Clear Kerberos tickets

```Console
KList purge
```

## REM Run command that requires authentication to the target server

```Console
dir \\ICEMAN\Products
```

## REM Stop network capture

```Console
netsh trace stop
```

## REM Copy network capture for analysis

```Console
robocopy C:\NotBackedUp\Temp\Captures \\WOLVERINE\C$\NotBackedUp\Temp\Captures
```

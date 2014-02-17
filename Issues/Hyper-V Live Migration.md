# Hyper-V Live Migration

Monday, February 17, 2014
8:16 AM

## REM RDP to console

```Console
mstsc.exe /console /v:STORM
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

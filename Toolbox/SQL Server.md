# SQL Server

Tuesday, May 19, 2015
11:06 AM

## # Restart SQL Server

```PowerShell
Stop-Service SQLSERVERAGENT

Restart-Service MSSQLSERVER

Start-Service SQLSERVERAGENT
```

## # Add DNS host records for SQL Server cluster

```PowerShell
Add-DNSServerResourceRecordA `
    -ZoneName extranet.technologytoolbox.com `
    -Name EXT-SQL01 `
    -IPv4Address 192.168.10.204

Add-DNSServerResourceRecordA `
    -ZoneName extranet.technologytoolbox.com `
    -Name EXT-SQL01-A `
    -IPv4Address 192.168.10.205
```

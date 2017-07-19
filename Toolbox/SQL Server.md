# SQL Server

Tuesday, May 19, 2015
11:06 AM

---


**SQL Server Management Studio**

## -- Create copy-only database backup

```SQL
DECLARE @databaseName VARCHAR(50) = 'WSS_Content'

DECLARE @backupDirectory VARCHAR(255)

EXEC master.dbo.xp_instance_regread
    N'HKEY_LOCAL_MACHINE'
    , N'Software\Microsoft\MSSQLServer\MSSQLServer'
    , N'BackupDirectory'
    , @backupDirectory OUTPUT

DECLARE @backupFilePath VARCHAR(255) =
    @backupDirectory + '\Full\' + @databaseName + '.bak'

DECLARE @backupName VARCHAR(100) = @databaseName + '-Full Database Backup'

BACKUP DATABASE @databaseName
    TO DISK = @backupFilePath
    WITH COMPRESSION
        , COPY_ONLY
        , INIT
        , NAME = @backupName
        , STATS = 10

GO
```

---


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

**SQL Server Maintenance Solution**\
From <[https://ola.hallengren.com/](https://ola.hallengren.com/)>



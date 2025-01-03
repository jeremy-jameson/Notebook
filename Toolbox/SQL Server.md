# SQL Server

Tuesday, May 19, 2015\
11:06 AM

---

**SQL Server Management Studio**

## Backup/restore

### -- Create copy-only database backup

```SQL
DECLARE @databaseName VARCHAR(50) = 'AdventureWorks2012'

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

### -- Restore database backup (overwrite existing database)

```SQL
DECLARE @databaseName VARCHAR(50) = 'AdventureWorks2012'

DECLARE @backupDirectory VARCHAR(255)

EXEC master.dbo.xp_instance_regread
    N'HKEY_LOCAL_MACHINE'
    , N'Software\Microsoft\MSSQLServer\MSSQLServer'
    , N'BackupDirectory'
    , @backupDirectory OUTPUT

DECLARE @backupFilePath VARCHAR(255) =
    @backupDirectory + '\Full\' + @databaseName + '.bak'

RESTORE DATABASE @databaseName
    FROM DISK = @backupFilePath
    WITH REPLACE
        , STATS = 5

GO
```

## -- Database default locations

```SQL
DECLARE @backupDirectory VARCHAR(255)

EXEC master.dbo.xp_instance_regread
    N'HKEY_LOCAL_MACHINE'
    , N'Software\Microsoft\MSSQLServer\MSSQLServer'
    , N'BackupDirectory'
    , @backupDirectory OUTPUT

SELECT
    @backupDirectory AS BackupDirectory
    , SERVERPROPERTY('instancedefaultdatapath') AS DefaultData
    , SERVERPROPERTY('instancedefaultlogpath') AS DefaultLog
```

---

---

**SQL Server Management Studio**

## User permissions

**SQL Server query to find all permissions/access for all users in a database**\
From <[https://stackoverflow.com/questions/7048839/sql-server-query-to-find-all-permissions-access-for-all-users-in-a-database](https://stackoverflow.com/questions/7048839/sql-server-query-to-find-all-permissions-access-for-all-users-in-a-database)>

```SQL
/*
Security Audit Report
1) List all access provisioned to a sql user or windows user/group directly
2) List all access provisioned to a sql user or windows user/group through a database or application role
3) List all access provisioned to the public role

Columns Returned:
UserName        : SQL or Windows/Active Directory user cccount.  This could also be an Active Directory group.
UserType        : Value will be either 'SQL User' or 'Windows User'.  This reflects the type of user defined for the
                  SQL Server user account.
DatabaseUserName: Name of the associated user as defined in the database user account.  The database user may not be the
                  same as the server user.
Role            : The role name.  This will be null if the associated permissions to the object are defined at directly
                  on the user account, otherwise this will be the name of the role that the user is a member of.
PermissionType  : Type of permissions the user/role has on an object. Examples could include CONNECT, EXECUTE, SELECT
                  DELETE, INSERT, ALTER, CONTROL, TAKE OWNERSHIP, VIEW DEFINITION, etc.
                  This value may not be populated for all roles.  Some built in roles have implicit permission
                  definitions.
PermissionState : Reflects the state of the permission type, examples could include GRANT, DENY, etc.
                  This value may not be populated for all roles.  Some built in roles have implicit permission
                  definitions.
ObjectType      : Type of object the user/role is assigned permissions on.  Examples could include USER_TABLE,
                  SQL_SCALAR_FUNCTION, SQL_INLINE_TABLE_VALUED_FUNCTION, SQL_STORED_PROCEDURE, VIEW, etc.
                  This value may not be populated for all roles.  Some built in roles have implicit permission
                  definitions.
ObjectName      : Name of the object that the user/role is assigned permissions on.
                  This value may not be populated for all roles.  Some built in roles have implicit permission
                  definitions.
ColumnName      : Name of the column of the object that the user/role is assigned permissions on. This value
                  is only populated if the object is a table, view or a table value function.
*/

--List all access provisioned to a sql user or windows user/group directly
SELECT
    [UserName] = CASE princ.[type]
                    WHEN 'S' THEN princ.[name]
                    WHEN 'U' THEN ulogin.[name] COLLATE Latin1_General_CI_AI
                 END,
    [UserType] = CASE princ.[type]
                    WHEN 'S' THEN 'SQL User'
                    WHEN 'U' THEN 'Windows User'
                 END,
    [DatabaseUserName] = princ.[name],
    [Role] = null,
    [PermissionType] = perm.[permission_name],
    [PermissionState] = perm.[state_desc],
    [ObjectType] = obj.type_desc,--perm.[class_desc],
    [ObjectName] = OBJECT_NAME(perm.major_id),
    [ColumnName] = col.[name]
FROM
    --database user
    sys.database_principals princ
LEFT JOIN
    --Login accounts
    sys.login_token ulogin on princ.[sid] = ulogin.[sid]
LEFT JOIN
    --Permissions
    sys.database_permissions perm ON perm.[grantee_principal_id] = princ.[principal_id]
LEFT JOIN
    --Table columns
    sys.columns col ON col.[object_id] = perm.major_id
                    AND col.[column_id] = perm.[minor_id]
LEFT JOIN
    sys.objects obj ON perm.[major_id] = obj.[object_id]
WHERE
    princ.[type] in ('S','U')
UNION
--List all access provisioned to a sql user or windows user/group through a database or application role
SELECT
    [UserName] = CASE memberprinc.[type]
                    WHEN 'S' THEN memberprinc.[name]
                    WHEN 'U' THEN ulogin.[name] COLLATE Latin1_General_CI_AI
                 END,
    [UserType] = CASE memberprinc.[type]
                    WHEN 'S' THEN 'SQL User'
                    WHEN 'U' THEN 'Windows User'
                 END,
    [DatabaseUserName] = memberprinc.[name],
    [Role] = roleprinc.[name],
    [PermissionType] = perm.[permission_name],
    [PermissionState] = perm.[state_desc],
    [ObjectType] = obj.type_desc,--perm.[class_desc],
    [ObjectName] = OBJECT_NAME(perm.major_id),
    [ColumnName] = col.[name]
FROM
    --Role/member associations
    sys.database_role_members members
JOIN
    --Roles
    sys.database_principals roleprinc ON roleprinc.[principal_id] = members.[role_principal_id]
JOIN
    --Role members (database users)
    sys.database_principals memberprinc ON memberprinc.[principal_id] = members.[member_principal_id]
LEFT JOIN
    --Login accounts
    sys.login_token ulogin on memberprinc.[sid] = ulogin.[sid]
LEFT JOIN
    --Permissions
    sys.database_permissions perm ON perm.[grantee_principal_id] = roleprinc.[principal_id]
LEFT JOIN
    --Table columns
    sys.columns col on col.[object_id] = perm.major_id
                    AND col.[column_id] = perm.[minor_id]
LEFT JOIN
    sys.objects obj ON perm.[major_id] = obj.[object_id]
UNION
--List all access provisioned to the public role, which everyone gets by default
SELECT
    [UserName] = '{All Users}',
    [UserType] = '{All Users}',
    [DatabaseUserName] = '{All Users}',
    [Role] = roleprinc.[name],
    [PermissionType] = perm.[permission_name],
    [PermissionState] = perm.[state_desc],
    [ObjectType] = obj.type_desc,--perm.[class_desc],
    [ObjectName] = OBJECT_NAME(perm.major_id),
    [ColumnName] = col.[name]
FROM
    --Roles
    sys.database_principals roleprinc
LEFT JOIN
    --Role permissions
    sys.database_permissions perm ON perm.[grantee_principal_id] = roleprinc.[principal_id]
LEFT JOIN
    --Table columns
    sys.columns col on col.[object_id] = perm.major_id
                    AND col.[column_id] = perm.[minor_id]
JOIN
    --All objects
    sys.objects obj ON obj.[object_id] = perm.[major_id]
WHERE
    --Only roles
    roleprinc.[type] = 'R' AND
    --Only public role
    roleprinc.[name] = 'public' AND
    --Only objects of ours, not the MS objects
    obj.is_ms_shipped = 0
ORDER BY
    princ.[Name],
    OBJECT_NAME(perm.major_id),
    col.[name],
    perm.[permission_name],
    perm.[state_desc],
    obj.type_desc--perm.[class_desc]
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

# Windows Server Backup

Saturday, November 02, 2013\
9:58 AM

```Text
PS C:\Windows\system32> Add-PSSnapin Windows.ServerBackup
PS C:\Windows\system32> $policy = Get-WBPolicy -Editable
PS C:\Windows\system32> Set-WBSchedule $policy 09:15,18:15

Saturday, November 02, 2013 9:15:00 AM
Saturday, November 02, 2013 6:15:00 PM


PS C:\Windows\system32> $policy.Schedule

Saturday, November 02, 2013 9:15:00 AM
Saturday, November 02, 2013 6:15:00 PM


PS C:\Windows\system32> Set-WBPolicy $policy
PS C:\Windows\system32>
```

**Backup Version and Space Management in Windows Server Backup**\
Pasted from <[http://blogs.technet.com/b/filecab/archive/2009/06/22/backup-version-and-space-management-in-windows-server-backup.aspx](http://blogs.technet.com/b/filecab/archive/2009/06/22/backup-version-and-space-management-in-windows-server-backup.aspx)>

**Volsnap Event ID: 25 and Loss of Previous Backups**\
Pasted from <[http://social.technet.microsoft.com/Forums/windowsserver/en-US/204e8cce-cd1f-4ca2-852e-a5f09a77c04c/volsnap-event-id-25-and-loss-of-previous-backups?forum=windowsbackup](http://social.technet.microsoft.com/Forums/windowsserver/en-US/204e8cce-cd1f-4ca2-852e-a5f09a77c04c/volsnap-event-id-25-and-loss-of-previous-backups?forum=windowsbackup)>

Log Name: System\
Source: volsnap\
Date: 12/21/2013 3:00:13 AM\
Event ID: 25\
Task Category: None\
Level: Error\
Keywords: Classic\
User: N/A\
Computer: beast.corp.technologytoolbox.com\
Description:\
The shadow copies of volume [\\\\?\\Volume{4152f97e-b029-41c9-96ce-efa1b6232e54}](\?\Volume{4152f97e-b029-41c9-96ce-efa1b6232e54}) were deleted because the shadow copy storage could not grow in time. Consider reducing the IO load on the system or choose a shadow copy storage volume that is not being shadow copied.\
Event Xml:

```XML
<Event xmlns="http://schemas.microsoft.com/win/2004/08/events/event">
  <System>
    <Provider Name="volsnap" />
    <EventID Qualifiers="49158">25</EventID>
    <Level>2</Level>
    <Task>0</Task>
    <Keywords>0x80000000000000</Keywords>
    <TimeCreated SystemTime="2013-12-21T10:00:13.151434800Z" />
    <EventRecordID>183444</EventRecordID>
    <Channel>System</Channel>
    <Computer>beast.corp.technologytoolbox.com</Computer>
    <Security />
  </System>
  <EventData>
    <Data>\Device\HarddiskVolumeShadowCopy52</Data>
    <Data>\\?\Volume{4152f97e-b029-41c9-96ce-efa1b6232e54}</Data>
    <Binary>000000000200300000000000190006C003000000000000000A000000000000000000000000000000</Binary>
  </EventData>
</Event>
```

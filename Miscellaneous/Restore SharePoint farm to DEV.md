# Restore SharePoint farm to DEV

Saturday, February 21, 2015
6:03 PM

<table>
<tr>
<td valign='top'>
<p>Managed Metadata Service</p>
</td>
<td valign='top'>
<p>Error</p>
</td>
<td valign='top'>
<p>2/21/2015 5:38 PM</p>
</td>
<td valign='top'>
<p>Object Managed Metadata Service failed in event OnPostRestore. For more information, see the spbackup.log or sprestore.log file located in the backup directory.<br />
SqlException: User does not have permission to perform this action.</p>
</td>
</tr>
</table>

From <[http://polaris-dev:22812/_admin/BackupStatus.aspx](http://polaris-dev:22812/_admin/BackupStatus.aspx)>

<table>
<tr>
<td valign='top'>
<p>Search Service Application</p>
</td>
<td valign='top'>
<p>Error</p>
</td>
<td valign='top'>
<p>2/21/2015 5:48 PM</p>
</td>
<td valign='top'>
<p>Object Search Service Application failed in event OnPostRestore. For more information, see the spbackup.log or sprestore.log file located in the backup directory.<br />
SqlException: The database principal owns a database role and cannot be dropped.<br />
User does not have permission to perform this action.</p>
</td>
</tr>
</table>

From <[http://polaris-dev:22812/_admin/BackupStatus.aspx](http://polaris-dev:22812/_admin/BackupStatus.aspx)>

Get-SPWebApplication | Remove-SPWebApplication -RemoveContentDatabases -DeleteIISSite -Confirm:\$false

Get-SPServiceApplication |\
where TypeName -ne "Security Token Service Application" |\
where TypeName -ne "Application Discovery and Load Balancer Service Application" |\
Remove-SPServiceApplication -RemoveData -Confirm:\$false

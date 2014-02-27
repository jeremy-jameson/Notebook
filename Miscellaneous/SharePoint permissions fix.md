# SharePoint permissions fix

Thursday, February 27, 2014
6:25 AM

Get-SPContentDatabase | Add-SPShellAdmin -UserName "TECHTOOLBOX\\SharePoint Administrators"

Add-PSSnapin Microsoft.SharePoint.PowerShell

\$webApp = Get-SPWebApplication [http://team](http://team)

Get-SPSite -WebApplication \$webApp | Set-SPSite -SecondaryOwnerAlias "TECHTOOLBOX\\jjameson"

# PowerShell to set SPListItem properties

Sunday, April 01, 2012
3:13 PM

## Set Source URL in SharePoint

```PowerShell
$ErrorActionPreference = "Stop"

Add-PSSnapin Microsoft.SharePoint.PowerShell -EA 0
```

```PowerShell
cls

$webUrl = "http://ttweb/Docs"

$web = Get-SPWeb $webUrl

$list = $web.Lists["Assets"]

$folderItem = $list.Folders | `
    Where {
        $_.Name -eq "Sales-Team"
    }

$folder = $folderItem.Folder

$query = New-Object Microsoft.SharePoint.SPQuery
$query.Folder = $folder

$list.GetItems($query) |
    Where {
        ($_.Url.EndsWith(".jpg") `
        -and ($_.Name -ne "Steve-Masters.jpg"))
    } |
    ForEach-Object {
        Write-Host ($_.Url + " - " + $_["SourceUrl"])
        $_["SourceUrl"] = "http://www.freedigitalphotos.net/images/view_photog.php?photogid=1499, Image: Ambro / FreeDigitalPhotos.net"
        $_.SystemUpdate()
    }

$web.Dispose()
```

---


## Rename Files

```PowerShell
$ErrorActionPreference = "Continue"
```

```PowerShell
cls

dir 7*.jpg | foreach-object {
    $newFilename = $_.Name.Substring(0, 5)

    $newFilename = "Business-Marketing-Team-" + $newFilename + ".jpg"

    ren $_ $newFilename
}
```

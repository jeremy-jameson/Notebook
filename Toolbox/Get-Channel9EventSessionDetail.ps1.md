# Get-Channel9EventSessionDetail.ps1

Monday, June 27, 2016
4:45 PM

```PowerShell
$ErrorActionPreference = "Stop"

Add-Type -Path "C:\NotBackedUp\Public\Toolbox\HtmlAgilityPack.1.4.9\lib\Net45\HtmlAgilityPack.dll"

If ($cachedPages -eq $null)
{
    $cachedPages = @{}
}

Function New-CachedPage([Uri] $url)
{
    Write-Debug "Ensuring page ($url) is cached..."

    [string] $cacheKey = $url.AbsoluteUri

    If ($cachedPages.ContainsKey($cacheKey) -eq $true)
    {
        return
    }

    Write-Verbose "Downloading page ($url)..."

    $response = Invoke-WebRequest -Uri $url -Verbose:$false

    $cachedPages[$cacheKey] = $response.Content
}

Function Get-Channel9EventPages([Uri] $eventUrl)
{
    Write-Verbose "Getting event pages ($eventUrl)..."

    $doc = Get-HtmlDocument $eventUrl

    [int] $pageCount = 0

    $doc.DocumentNode.SelectNodes("//ul[@class = 'paging']/li/a")  |
        ForEach-Object {
            $anchorText = $_.InnerText

            If ($anchorText -match "^[\d]+$")
            {
                [int] $pageNumber = $anchorText

                If ($pageNumber -gt $pageCount)
                {
                    $pageCount = $pageNumber
                }
            }
        }

    1..$pageCount |
        ForEach-Object {
            $eventPageUrl = New-Object Uri $eventUrl, `
                ("?sort=sequential&direction=desc&page=" + $_)

            New-CachedPage $eventPageUrl

            $eventPage = New-Object PSObject

            $eventPage | Add-Member NoteProperty -Name "Url" `
                -Value $eventPageUrl

            $eventPage
        }
}

Function Get-HtmlDocument([Uri] $url)
{
    Write-Debug "Getting HtmlDocument ($url)..."

    [string] $pageContent = Get-PageContent $url

    $doc = New-Object HtmlAgilityPack.HtmlDocument

    $doc.LoadHtml($pageContent)

    return $doc
}

Function Get-PageContent([Uri] $url)
{
    Write-Debug "Getting page ($url) content..."

    New-CachedPage $url

    return $cachedPages[$url.AbsoluteUri]
}


Function Get-Channel9EventSessions([Uri] $eventUrl)
{
    Write-Debug "Getting event sessions ($eventUrl)..."

    $doc = Get-HtmlDocument $eventUrl

    $doc.DocumentNode.SelectNodes("//div[@class = 'entry-meta']/a")  |
        ForEach-Object {
            $sessionTitle = $_.InnerText.Replace('&#160;', ' ')

            $sessionUrl = New-Object Uri $eventUrl, $_.Attributes['href'].Value

            $session = New-Object PSObject

            $session | Add-Member NoteProperty -Name "Title" `
                -Value $sessionTitle

            $session | Add-Member NoteProperty -Name "Url" `
                -Value $sessionUrl.AbsoluteUri

            $session
        }
}

Function Get-Channel9EventSessionDetail([Uri] $sessionUrl)
{
    Write-Debug "Getting session details ($sessionUrl)..."

    $doc = Get-HtmlDocument $sessionUrl

    $session = New-Object PSObject

    $doc.DocumentNode.SelectSingleNode("//div[@class = 'entry-header item-header']/h1")  |
        ForEach-Object {
            $sessionTitle = $_.InnerText.Replace('&#160;', ' ')

            $session | Add-Member NoteProperty -Name "Title" `
                -Value $sessionTitle
        }

    $doc.DocumentNode.SelectNodes("//ul[@class = 'download']/li/a")  |
        Where-Object { $_.InnerText -eq "High Quality MP4" } |
        ForEach-Object {
            $downloadUrl = New-Object Uri $sessionUrl, $_.Attributes['href'].Value

            $session | Add-Member NoteProperty -Name "DownloadUrl" `
                -Value $downloadUrl.AbsoluteUri
        }

    $session
}

[Uri] $eventUrl = New-Object Uri "https://channel9.msdn.com/events/SharePoint-Conference/2014"

Get-Channel9EventPages $eventUrl |
    ForEach-Object {
        Get-Channel9EventSessions $_.Url
    } |
    ForEach-Object {
        Get-Channel9EventSessionDetail $_.Url
    }
```

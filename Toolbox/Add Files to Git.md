# Add Files to Git

Thursday, August 31, 2017\
5:58 AM

## Import Files to Git.ps1

```PowerShell
Function CreateTaskObject($taskName, $command, $timestamp = (Get-Date))
{
    $result = New-Object -TypeName PSObject

    $result | Add-Member -MemberType NoteProperty -Name Timestamp -Value $timestamp
    $result | Add-Member -MemberType NoteProperty -Name Name -Value $taskName
    $result | Add-Member -MemberType NoteProperty -Name Command -Value $command

    $result
}

Function GetTasksToCommitFiles($commitMessage, $commitDate, $taskName = "Commit Files")
{
    $utcOffsetHours = [TimeZoneInfo]::Local |
        select -ExpandProperty BaseUtcOffset |
        select -ExpandProperty Hours

    $timezoneOffset = $utcOffsetHours.ToString("00") + "00"

    $commitDateInRfc2822Format = `
        $commitDate.ToString("R").Replace("GMT", $timezoneOffset)

    CreateTaskObject $taskName "`$env:GIT_AUTHOR_DATE = '$commitDateInRfc2822Format'" $commitDate
    CreateTaskObject $taskName "`$env:GIT_COMMITTER_DATE = '$commitDateInRfc2822Format'" $commitDate

    CreateTaskObject $taskName "git commit -m `"$commitMessage`"" $commitDate
}

Function GetTasksToInitializeRepository($commitDate, $taskName = "Initialize Repository")
{
    CreateTaskObject $taskName "git init"

    $gitattributesContent = @"
# Auto detect text files and perform LF normalization
* text=auto

# Custom for Visual Studio
*.cs     diff=csharp

# Standard to msysgit
*.doc	 diff=astextplain
*.DOC	 diff=astextplain
*.docx diff=astextplain
*.DOCX diff=astextplain
*.dot  diff=astextplain
*.DOT  diff=astextplain
*.pdf  diff=astextplain
*.PDF	 diff=astextplain
*.rtf	 diff=astextplain
*.RTF	 diff=astextplain
"@

    $gitignoreContent = @"
# Windows image file caches
Thumbs.db
ehthumbs.db

# Folder config file
Desktop.ini

# Recycle Bin used on file shares
`$RECYCLE.BIN/

# Windows Installer files
*.cab
*.msi
*.msm
*.msp

# Windows shortcuts
*.lnk

# =========================
# Operating System Files
# =========================

# OSX
# =========================

.DS_Store
.AppleDouble
.LSOverride

# Thumbnails
._*

# Files that might appear in the root of a volume
.DocumentRevisions-V100
.fseventsd
.Spotlight-V100
.TemporaryItems
.Trashes
.VolumeIcon.icns

# Directories potentially created on remote AFP share
.AppleDB
.AppleDesktop
Network Trash Folder
Temporary Items
.apdisk
"@

    Set-Content -Value $gitattributesContent -Path .gitattributes
    Set-Content -Value $gitignoreContent -Path .gitignore

    CreateTaskObject $taskName "git add .gitattributes .gitignore"

    $commitMessage = "Added .gitattributes & .gitignore files"
    GetTasksToCommitFiles $commitMessage $commitDate $taskName
}
```

```PowerShell
cls
$repoName = "Securitas"

$repoPath = "C:\NotBackedUp\Temp\$repoName"

#robocopy "\\TT-FS01\Users$\jjameson\Desktop\Securitas" $repoPath /E /MIR /NP

#If ((Test-Path "$repoPath\.git") -eq $true)
#{
#    Remove-Item "$repoPath\.git" -Recurse -Force
#}

Push-Location $repoPath

$files = Get-ChildItem . -Exclude .gitattributes, .gitignore -Recurse |
    where { $_.PSisContainer -eq $false } |
    sort LastWriteTime |
    select FullName, Name, LastWriteTime

$taskFilePath = "C:\NotBackedUp\Temp\tmp.csv"

CreateTaskObject "Set Location" "Push-Location '$repoPath'" |
    Export-Csv -Encoding UTF8 -Path $taskFilePath -NoTypeInformation

If ((Test-Path .git) -eq $false)
{
    $commitDate = $files |
        select -First 1 |
        select -ExpandProperty LastWriteTime

    $commitDate = $commitDate.AddMinutes(-3)

    GetTasksToInitializeRepository $commitDate |
        Export-Csv -Path $taskFilePath -Append

}

$files |
    foreach {
        $relativePath = $_.FullName.Replace($repoPath + "\", "")

        $commitMessage = "Added file ($($_.Name))"
        $commitDate = Get-Date $_.LastWriteTime

        $taskName = "Add File"

        CreateTaskObject $taskName "git add `"$relativePath`"" $commitDate

        GetTasksToCommitFiles $commitMessage $commitDate
    } |
    Export-Csv -Path $taskFilePath -Append

CreateTaskObject "Revert Location" "Pop-Location" |
    Export-Csv -Path $taskFilePath -Append

Pop-Location
```

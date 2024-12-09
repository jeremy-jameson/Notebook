# SharePoint

Tuesday, June 02, 2015\
6:30 AM

```PowerShell
cls
```

## # Export failed timer jobs to CSV (revised -- slower but returns all failed jobs)

```PowerShell
(Get-SPFarm).Services |
    % { $_.JobHistoryEntries | ? { $_.Status -eq "Failed" } } |
    sort StartTime |
    select JobDefinitionTitle, ServerName, WebApplicationName, StartTime, EndTime,
        ErrorMessage |
    Export-Csv C:\NotBackedUp\Temp\Failed-Timer-Jobs.csv
```

## Export failed timer jobs to CSV (initial attempt -- fails to return all failed jobs)

> **Note**
>
> In the following script, **\$failedFarmJobs** does not include the following failed jobs reported in Central Administration:
>
> - **eDiscovery In-Place Hold Processing**
> - **User Profile Service Application - User Profile Incremental Synchronization**
> - **Health Statistics Updating**
> - **Search Health Monitoring - Trace Events**
> - **Query Classification Dictionary Update for Search Application Search Service Application.**
> - **Search Custom Dictionaries Update**

```PowerShell
$failedWebAppJobs = Get-SPWebApplication -IncludeCentralAdministration |
    % { $_.JobHistoryEntries | ? { $_.Status -eq "Failed" } }

$failedFarmJobs = ((Get-SPFarm).TimerService).JobHistoryEntries |
    ? { $_.Status -eq "Failed" }

$failedTimerJobs = @()
$failedTimerJobs += $failedWebAppJobs
$failedTimerJobs += $failedFarmJobs

$failedTimerJobs |
    sort StartTime |
    select JobDefinitionTitle, ServerName, WebApplicationName, StartTime, EndTime,
        ErrorMessage |
    Export-Csv C:\NotBackedUp\Temp\Failed-Timer-Jobs.csv
```

```PowerShell
cls
```

## # Start full crawl

```PowerShell
Get-SPEnterpriseSearchServiceApplication |
    Get-SPEnterpriseSearchCrawlContentSource |
    % { $_.StartFullCrawl() }
```

```PowerShell
cls
```

## #Analyze ULS logs using LogParser

```PowerShell
"C:\Program Files (x86)\Log Parser 2.2\logparser.exe" -i:tsv -iCodepage:-1 "SELECT Timestamp, Process, TID, Area, Category, EventID, Level, Message, Correlation INTO LogStaging FROM 'E:\NotBackedUp\SharePoint Logs\*.log'" -o:SQL -oConnString: "server=localhost;Database=SharePointAnalysis;Trusted_Connection=yes;" -iCheckPoint:"E:\NotBackedUp\SharePoint Logs\LogParserCheckpoint.lpc"
```

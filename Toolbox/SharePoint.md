# SharePoint

Tuesday, June 02, 2015
6:30 AM

```PowerShell
cls
```

## # Start full crawl

```PowerShell
Get-SPEnterpriseSearchServiceApplication |
    Get-SPEnterpriseSearchCrawlContentSource |
    % { $_.StartFullCrawl() }


"C:\Program Files (x86)\Log Parser 2.2\logparser.exe" -i:tsv -iCodepage:-1 "SELECT Timestamp, Process, TID, Area, Category, EventID, Level, Message, Correlation INTO LogStaging FROM 'E:\NotBackedUp\SharePoint Logs\*.log'" -o:SQL -oConnString: "server=localhost;Database=SharePointAnalysis;Trusted_Connection=yes;" -iCheckPoint:"E:\NotBackedUp\SharePoint Logs\LogParserCheckpoint.lpc"
```

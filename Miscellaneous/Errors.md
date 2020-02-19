# Errors

Wednesday, September 05, 2012
5:58 AM

```SQL
SET NOCOUNT ON

IF OBJECT_ID('tempdb..#Error') IS NOT NULL DROP TABLE #Error

CREATE TABLE #Error
(
    ErrorId UNIQUEIDENTIFIER NOT NULL PRIMARY KEY
    , TimeUtc DATETIME NOT NULL
    , XmlText NVARCHAR(MAX) NOT NULL
    , XmlDocument XML
    , XmlErrorMessage NVARCHAR(2048)
    , StatusCode INT
    , RemoteAddress NVARCHAR(15)
    , Referrer NVARCHAR(MAX)
)
GO

INSERT INTO #Error
(
    ErrorId
    , StatusCode
    , TimeUtc
    , XmlText
)
SELECT
    ErrorId
    , StatusCode
    , TimeUtc
    , AllXml
FROM Staging.ELMAH_Error
GO

DECLARE @errorId UNIQUEIDENTIFIER

DECLARE c CURSOR LOCAL FOR
SELECT ErrorId
FROM #Error
FOR UPDATE OF XmlDocument

OPEN c

FETCH NEXT FROM c INTO @errorId
WHILE @@FETCH_STATUS = 0
BEGIN

    BEGIN TRY
        UPDATE #Error
        SET XmlDocument = CAST(XmlText AS XML)
        WHERE CURRENT OF c
    END TRY
    BEGIN CATCH
        UPDATE #Error
        SET XmlErrorMessage = ERROR_MESSAGE()
        WHERE CURRENT OF c
    END CATCH

    FETCH NEXT FROM c INTO @errorId
END

CLOSE c
DEALLOCATE c
GO

UPDATE #Error
SET
    RemoteAddress = CONVERT(VARCHAR, XmlDocument.query('data(//item[@name = "REMOTE_ADDR"]/value/@string)'))
    , Referrer = CONVERT(VARCHAR(MAX), XmlDocument.query('data(//item[@name = "HTTP_REFERER"]/value/@string)'))
WHERE
    XmlErrorMessage IS NULL

SELECT RemoteAddress, COUNT(*)
FROM #Error
WHERE TimeUtc > '2013-01-31'
GROUP BY RemoteAddress
ORDER BY COUNT(*) DESC
```

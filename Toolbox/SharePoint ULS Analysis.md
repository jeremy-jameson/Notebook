# SharePoint ULS Analysis

Friday, June 17, 2016
9:01 AM

USE SharePointAnalysis\
GO

DROP TABLE Staging.UlsLogEntry\
DROP TABLE Staging.UlsLog\
GO

CREATE TABLE Staging.UlsLog(\
LogId INT IDENTITY(1,1) NOT NULL,\
Filename NVARCHAR(255) NOT NULL,\
ServerName NVARCHAR(15) NOT NULL,\
CONSTRAINT PK_UlsLog PRIMARY KEY (LogId)\
)

CREATE TABLE Staging.UlsLogEntry(\
LogId INT NOT NULL,\
LineNumber INT NOT NULL,\
LogEntryTimestamp DATETIME2 NOT NULL,\
ServerName NVARCHAR(15) NOT NULL,\
Process NVARCHAR(40) NOT NULL,\
ThreadId NVARCHAR(6) NOT NULL,\
Product NVARCHAR(31) NOT NULL,\
Category NVARCHAR(31) NOT NULL,\
EventId NVARCHAR(5) NOT NULL,\
Level NVARCHAR(11) NOT NULL,\
CorrelationId UNIQUEIDENTIFIER NULL,\
Message NVARCHAR(max) NULL,\
ExecutionTime float NULL,\
CONSTRAINT PK_UlsLogEntry PRIMARY KEY (LogId, LineNumber),\
CONSTRAINT FK_UlsLogEntry_UlsLog\
FOREIGN KEY (LogId)\
REFERENCES Staging.UlsLog(LogId)\
)

GO



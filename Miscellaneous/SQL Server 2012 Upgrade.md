# SQL Server 2012 Upgrade

Wednesday, February 06, 2013\
9:51 AM

Source: TFS Services\
Event ID: 3305\
Event Category: 0\
User: N/A\
Computer: cyclops.corp.technologytoolbox.com\
Event Description: TF53010: The following error has occurred in a Team Foundation component or extension:\
Date (UTC): 2/6/2013 4:23:01 PM\
Machine: CYCLOPS\
Application Domain: TfsJobAgent.exe\
Assembly: Microsoft.TeamFoundation.Framework.Server, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a; v2.0.50727\
Service Host:\
Process Details:\
  Process Name: TFSJobAgent\
  Process Id: 2896\
  Thread Id: 8420\
  Account name: TECHTOOLBOX\\svc-tfs

Detailed Message: TF221122: An error occurred running job Incremental Analysis Database Sync for team project collection or Team Foundation server TEAM FOUNDATION.\
Exception Message: Failed to Process Analysis Database 'Tfs_Analysis'. (type WarehouseException)

Exception Stack Trace:    at Microsoft.TeamFoundation.Warehouse.TFSOlapProcessComponent.ProcessOlap(AnalysisDatabaseProcessingType processingType, WarehouseChanges warehouseChanges, Boolean lastProcessingFailed, Boolean cubeSchemaUpdateNeeded)\
   at Microsoft.TeamFoundation.Warehouse.AnalysisDatabaseSyncJobExtension.RunInternal(TeamFoundationRequestContext requestContext, TeamFoundationJobDefinition jobDefinition, DateTime queueTime, String& resultMessage)\
   at Microsoft.TeamFoundation.Warehouse.WarehouseJobExtension.Run(TeamFoundationRequestContext requestContext, TeamFoundationJobDefinition jobDefinition, DateTime queueTime, String& resultMessage)

Inner Exception Details:

Exception Message: Internal error: The operation terminated unsuccessfully.\
Server: The current operation was cancelled because another operation in the transaction failed.\
Errors in the back-end database access module. The provider 'SQLNCLI10.1' is not registered.\
The following system error occurred:  Class not registered\
Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'Tfs_AnalysisDataSource', Name of 'Tfs_AnalysisDataSource'.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'System_Reason' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'System_Revision' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'Area Team Project Collection' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'Iteration Team Project Collection' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the back-end database access module. The provider 'SQLNCLI10.1' is not registered.\
The following system error occurred:  Class not registered\
Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'Tfs_AnalysisDataSource', Name of 'Tfs_AnalysisDataSource'.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'System_CreatedBy' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'System_ChangedDate__Year' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'System_WorkItemType' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'System_CreatedDate__Year' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'Microsoft_VSTS_Common_Issue' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'Microsoft_VSTS_Common_ActivatedDate__Year' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'Microsoft_VSTS_Common_ResolvedDate__Year' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the back-end database access module. The provider 'SQLNCLI10.1' is not registered.\
The following system error occurred:  Class not registered\
Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'Tfs_AnalysisDataSource', Name of 'Tfs_AnalysisDataSource'.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'System_AssignedTo' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the back-end database access module. The provider 'SQLNCLI10.1' is not registered.\
The following system error occurred:  Class not registered\
Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'Tfs_AnalysisDataSource', Name of 'Tfs_AnalysisDataSource'.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'System_Title' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the back-end database access module. The provider 'SQLNCLI10.1' is not registered.\
The following system error occurred:  Class not registered\
Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'Tfs_AnalysisDataSource', Name of 'Tfs_AnalysisDataSource'.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'System_ChangedBy' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the back-end database access module. The provider 'SQLNCLI10.1' is not registered.\
The following system error occurred:  Class not registered\
Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'Tfs_AnalysisDataSource', Name of 'Tfs_AnalysisDataSource'.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'System_Id' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the back-end database access module. The provider 'SQLNCLI10.1' is not registered.\
The following system error occurred:  Class not registered\
Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'Tfs_AnalysisDataSource', Name of 'Tfs_AnalysisDataSource'.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'Previous State' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the back-end database access module. The provider 'SQLNCLI10.1' is not registered.\
The following system error occurred:  Class not registered\
Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'Tfs_AnalysisDataSource', Name of 'Tfs_AnalysisDataSource'.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'TeamProjectSK' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the back-end database access module. The provider 'SQLNCLI10.1' is not registered.\
The following system error occurred:  Class not registered\
Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'Tfs_AnalysisDataSource', Name of 'Tfs_AnalysisDataSource'.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'Area Path' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the back-end database access module. The provider 'SQLNCLI10.1' is not registered.\
The following system error occurred:  Class not registered\
Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'Tfs_AnalysisDataSource', Name of 'Tfs_AnalysisDataSource'.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'System_State' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.\
Errors in the back-end database access module. The provider 'SQLNCLI10.1' is not registered.\
The following system error occurred:  Class not registered\
Errors in the high-level relational engine. A connection could not be made to the data source with the DataSourceID of 'Tfs_AnalysisDataSource', Name of 'Tfs_AnalysisDataSource'.\
Errors in the OLAP storage engine: An error occurred while the dimension, with the ID of 'vDimTestCaseOverlay', Name of 'Test Case' was being processed.\
Errors in the OLAP storage engine: An error occurred while the 'Iteration Path' attribute of the 'Test Case' dimension from the 'Tfs_Analysis' database was being processed.

Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, FactCurrentWorkItem', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, FactWorkItemHistory', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, v Fact WorkItem To Tree', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, v Fact Linked Current WorkItem', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, Fact WorkItem To Category', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, v Fact WorkItem Changeset', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, Fact Build Project', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, Fact Build Details', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, Fact Code Churn', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, v Fact Test Result Overlay', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, Fact Build Changeset', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, Fact Build Coverage', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, v Fact WorkItem Test Result', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, v Fact Linked Current Work Item Test Case', has been specified but has not been used.\
Warning: Parser: Out of line object 'Binding', referring to ID(s) 'Tfs_Analysis, Team System, Fact Run Coverage', has been specified but has not been used. (type WarehouseException)

Exception Stack Trace:    at Microsoft.TeamFoundation.Warehouse.TFSOlapProcessComponent.ExecuteXmla(String finalXmla)\
   at Microsoft.TeamFoundation.Warehouse.TFSOlapProcessComponent.ProcessOlap(AnalysisDatabaseProcessingType processingType, WarehouseChanges warehouseChanges, Boolean lastProcessingFailed, Boolean cubeSchemaUpdateNeeded)

---


Modify data source (**Tfs_AnalysisDataSource**) to change provider to **SQLNCLI11**

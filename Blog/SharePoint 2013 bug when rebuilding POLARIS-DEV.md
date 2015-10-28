# SharePoint 2013 bug when rebuilding POLARIS-DEV

Tuesday, October 27, 2015
5:43 PM

PS C:\\windows\\system32> # Create SharePoint farm\
PS C:\\windows\\system32>

 Create SharePoint farm\
    Registering features...

[2015-10-27 17:38:17] Create SharePoint farm -

  Farm account: TECHTOOLBOX\\s-sharepoint-dev\
  Configuration database server: POLARIS-DEV\
  Configuration database name: SharePoint_Config\
  Central Administration database name: SharePoint_AdminContent\
  Central Administration port: 22812\
  Authentication provider: Kerberos

Confirm\
Are you sure you want to perform this action?\
Performing the operation "Create Farm.ps1" on target "POLARIS-DEV".\
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):

Unhandled Exception: System.AccessViolationException: Attempted to read or write protected memory. This is often an indication that other memory is corrupt.\
   at Microsoft.SharePoint.Client.ProxyMap.ProcessMetadataProviderTypeAttribute(Assembly azzembly, ClientServiceHost serviceHost)\
   at Microsoft.SharePoint.SPListCollection.GetListByName(String strListName, Boolean bThrowException)\
   at Microsoft.SharePoint.Publishing.Internal.Store.GetListIfExists(SPListCollection lists, String listName)\
   at Microsoft.SharePoint.Publishing.Internal.ProvisioningHelper.AddList(SPListCollection lists, String urlName, String title, String description, Guid featureId, Guid[] previousVersionFeatureIds, Int32 templateType, Boolean& newListCreated)\
   at Microsoft.SharePoint.Publishing.DeploymentFeatureHandler.<>c__DisplayClass1.`<CreateListIfDoesNotExist>`b__0()\
   at Microsoft.Office.Server.Utilities.CultureUtility.RunWithCultureScope(CodeToRunWithCultureScope code)\
   at Microsoft.SharePoint.Publishing.DeploymentFeatureHandler.EnsureContentDeploymentPathsList(SPWeb rootWeb)\
   at Microsoft.SharePoint.Publishing.DeploymentFeatureHandler.FeatureActivated(SPFeatureReceiverProperties receiverProperties)\
   at Microsoft.SharePoint.SPFeature.DoActivationCallout(Boolean fActivate, Boolean fForce)\
   at Microsoft.SharePoint.SPFeature.Activate(SPSite siteParent, SPWeb webParent, SPFeaturePropertyCollection props, SPFeatureActivateFlags activateFlags, Boolean fForce)\
   at Microsoft.SharePoint.SPFeatureCollection.AddInternal(SPFeatureDefinition featdef, Version version, SPFeaturePropertyCollection properties, SPFeatureActivateFlags activateFlags, Boolean force, Boolean fMarkOnly)\
   at Microsoft.SharePoint.SPFeature.ActivateDeactivateFeatureAtWeb(Boolean fActivate, Boolean fEnsure, Guid featid, SPFeatureDefinition featdef, String urlScope, String sProperties, Boolean fForce)\
   at Microsoft.SharePoint.Administration.SPFeatureDefinition.ActivateInCentralAdmin()\
   at Microsoft.SharePoint.Administration.SPFeatureDefinition.Install(SPSite site, String solutionHash)\
   at Microsoft.SharePoint.Administration.SPFeatureDefinitionCollection.AddCore(SPFeatureDefinition featdef, SPSite site, String solutionHash, Boolean fForce, Boolean fDoValidation, String pathToFeatureAndElementManifestXsdFile)\
   at Microsoft.SharePoint.Administration.SPFeatureDefinitionCollection.AddInternal(String relativePathToFeatureManifest\
, Int32 compatibilityLevel, String pathToFeatureAndElementManifestXsdFile, Guid solutionId, String solutionHash, SPSite\
site, Boolean force, Boolean fDoValidation, SPFeatureDefinitionContext featureDefinitionContext)\
   at Microsoft.SharePoint.Administration.SPFeatureDefinitionCollection.ScanForFeaturesCore(Guid solid, Boolean fScanOnly, Boolean fForce, ArrayList& rgpathFailedFeatures)\
   at Microsoft.SharePoint.PowerShell.SPCmdletInstallFeature.FindAndInstallFeatures(Guid solutionId, Boolean bDisplayOnly, Boolean bForce, ArrayList& pathFailedFeatures)\
   at Microsoft.SharePoint.PowerShell.SPCmdletInstallFeature.RetrieveDataObjects()\
   at Microsoft.SharePoint.PowerShell.SPGetCmdletBase`1.InternalProcessRecord()\
   at Microsoft.SharePoint.PowerShell.SPCmdlet.ProcessRecord()\
   at System.Management.Automation.CommandProcessor.ProcessRecord()\
   at System.Management.Automation.CommandProcessorBase.DoExecute()\
   at System.Management.Automation.Internal.PipelineProcessor.SynchronousExecuteEnumerate(Object input, Hashtable errorResults, Boolean enumerate)\
   at System.Management.Automation.PipelineOps.InvokePipeline(Object input, Boolean ignoreInput, CommandParameterInternal[][] pipeElements, CommandBaseAst[] pipeElementAsts, CommandRedirection[][] commandRedirections, FunctionContext funcContext)\
   at System.Management.Automation.Interpreter.ActionCallInstruction`6.Run(InterpretedFrame frame)\
   at System.Management.Automation.Interpreter.EnterTryCatchFinallyInstruction.Run(InterpretedFrame frame)\
   at System.Management.Automation.Interpreter.EnterTryCatchFinallyInstruction.Run(InterpretedFrame frame)\
   at System.Management.Automation.Interpreter.EnterTryCatchFinallyInstruction.Run(InterpretedFrame frame)\
   at System.Management.Automation.Interpreter.Interpreter.Run(InterpretedFrame frame)\
   at System.Management.Automation.Interpreter.LightLambda.RunVoid1[T0](T0 arg0)\
   at System.Management.Automation.PSScriptCmdlet.RunClause(Action`1 clause, Object dollarUnderbar, Object inputToProcess)\
   at System.Management.Automation.PSScriptCmdlet.DoProcessRecord()\
   at System.Management.Automation.CommandProcessor.ProcessRecord()\
   at System.Management.Automation.CommandProcessorBase.DoExecute()\
   at System.Management.Automation.Internal.PipelineProcessor.SynchronousExecuteEnumerate(Object input, Hashtable errorResults, Boolean enumerate)\
   at System.Management.Automation.PipelineOps.InvokePipeline(Object input, Boolean ignoreInput, CommandParameterInternal[][] pipeElements, CommandBaseAst[] pipeElementAsts, CommandRedirection[][] commandRedirections, FunctionContext funcContext)\
   at System.Management.Automation.Interpreter.ActionCallInstruction`6.Run(InterpretedFrame frame)\
   at System.Management.Automation.Interpreter.EnterTryCatchFinallyInstruction.Run(InterpretedFrame frame)\
   at System.Management.Automation.Interpreter.EnterTryCatchFinallyInstruction.Run(InterpretedFrame frame)\
   at System.Management.Automation.Interpreter.Interpreter.Run(InterpretedFrame frame)\
   at System.Management.Automation.Interpreter.LightLambda.RunVoid1[T0](T0 arg0)\
   at System.Management.Automation.DlrScriptCommandProcessor.RunClause(Action`1 clause, Object dollarUnderbar, Object inputToProcess)\
   at System.Management.Automation.DlrScriptCommandProcessor.Complete()\
   at System.Management.Automation.CommandProcessorBase.DoComplete()\
   at System.Management.Automation.Internal.PipelineProcessor.DoCompleteCore(CommandProcessorBase commandRequestingUpstreamCommandsToStop)\
   at System.Management.Automation.Internal.PipelineProcessor.SynchronousExecuteEnumerate(Object input, Hashtable errorResults, Boolean enumerate)\
   at System.Management.Automation.Runspaces.LocalPipeline.InvokeHelper()\
   at System.Management.Automation.Runspaces.LocalPipeline.InvokeThreadProc()\
   at System.Management.Automation.Runspaces.PipelineThread.WorkerProc()\
   at System.Threading.ExecutionContext.RunInternal(ExecutionContext executionContext, ContextCallback callback, Object state, Boolean preserveSyncCtx)\
   at System.Threading.ExecutionContext.Run(ExecutionContext executionContext, ContextCallback callback, Object state, Boolean preserveSyncCtx)\
   at System.Threading.ExecutionContext.Run(ExecutionContext executionContext, ContextCallback callback, Object state)\
   at System.Threading.ThreadHelper.ThreadStart()

10/27/2015 17:41:24.96	powershell.exe (0x10C8)	0x1104	SharePoint Foundation	General	8dyp	Medium	Feature Installation: Installing Feature 'DeploymentLinks' (ID: 'ca2543e6-29a1-40c1-bba9-bd8510a4c17b') into the farm.	06052868-4010-4421-91cf-6af1e39160e2\
10/27/2015 17:41:24.96	powershell.exe (0x10C8)	0x1104	SharePoint Foundation	Monitoring	nasq	High	Entering monitored scope (Feature Installation: Installing Feature 'DeploymentLinks' (ID: 'ca2543e6-29a1-40c1-bba9-bd8510a4c17b') into the farm.). Parent No	06052868-4010-4421-91cf-6af1e39160e2\
10/27/2015 17:41:24.96	powershell.exe (0x10C8)	0x1104	SharePoint Foundation	General	8l0o	Medium	Feature Installation: Calling FeatureInstalled method for Feature 'DeploymentLinks' (ID: 'ca2543e6-29a1-40c1-bba9-bd8510a4c17b', Compatibility Level: 15).	06052868-4010-4421-91cf-6af1e39160e2\
10/27/2015 17:41:24.96	powershell.exe (0x10C8)	0x1104	SharePoint Foundation	Monitoring	b4ly	High	Leaving Monitored Scope (Feature Installation: Installing Feature 'DeploymentLinks' (ID: 'ca2543e6-29a1-40c1-bba9-bd8510a4c17b') into the farm.). Execution Time=0.3801	06052868-4010-4421-91cf-6af1e39160e2\
10/27/2015 17:41:24.96	powershell.exe (0x10C8)	0x1104	SharePoint Foundation	General	co5p	Medium	Feature Activation: Auto-activating Feature 'DeploymentLinks' (ID: 'ca2543e6-29a1-40c1-bba9-bd8510a4c17b') in Central Admin context.	06052868-4010-4421-91cf-6af1e39160e2\
10/27/2015 17:41:24.96	powershell.exe (0x10C8)	0x1104	SharePoint Foundation	Upgrade	ajyw6	High	10/27/2015 17:41:24.96 powershell (0x10C8) 0x1104 SharePoint Foundation Upgrade SPHierarchyManager ajyw6 DEBUG [SPTree Value=SPSite Url=http://polaris-dev:7733] added to dependency cache by lookup 06052868-4010-4421-91cf-6af1e39160e2	06052868-4010-4421-91cf-6af1e39160e2\
10/27/2015 17:41:24.96	powershell.exe (0x10C8)	0x1104	SharePoint Foundation	General	88jb	Medium	Feature Activation: Activating Feature 'DeploymentLinks' (ID: 'ca2543e6-29a1-40c1-bba9-bd8510a4c17b') at URL [http://polaris-dev:7733](http://polaris-dev:7733).	06052868-4010-4421-91cf-6af1e39160e2\
10/27/2015 17:41:24.96	powershell.exe (0x10C8)	0x1104	SharePoint Foundation	General	75fb	Medium	Calling 'FeatureActivated' method of SPFeatureReceiver for Feature 'DeploymentLinks' (ID: 'ca2543e6-29a1-40c1-bba9-bd8510a4c17b').	06052868-4010-4421-91cf-6af1e39160e2\
10/27/2015 17:41:24.96	powershell.exe (0x10C8)	0x1104	Web Content Management	Content Deployment	53yi	Medium	ContentDeployment FeatureActivated: start	06052868-4010-4421-91cf-6af1e39160e2\
10/27/2015 17:41:24.96	powershell.exe (0x10C8)	0x1104	SharePoint Foundation	Database	4ohp	High	Enumerating all sites in SPAdministrationWebApplication.	06052868-4010-4421-91cf-6af1e39160e2\
10/27/2015 17:41:24.96	powershell.exe (0x10C8)	0x1104	SharePoint Foundation	Database	4ohq	Medium	Site Enumeration Stack:    at Microsoft.SharePoint.Administration.SPSiteCollection.get_Item(Int32 index)     at Microsoft.SharePoint.Publishing.Internal.Store.GetAdminSite()     at Microsoft.SharePoint.Publishing.DeploymentFeatureHandler.FeatureActivated(SPFeatureReceiverProperties receiverProperties)     at Microsoft.SharePoint.SPFeature.DoActivationCallout(Boolean fActivate, Boolean fForce)     at Microsoft.SharePoint.SPFeature.Activate(SPSite siteParent, SPWeb webParent, SPFeaturePropertyCollection props, SPFeatureActivateFlags activateFlags, Boolean fForce)     at Microsoft.SharePoint.SPFeatureCollection.AddInternal(SPFeatureDefinition featdef, Version version, SPFeaturePropertyCollection properties, SPFeatureActivateFlags activateFlags, Boolean force, Boolean fMarkOnly)     at Microsoft.SharePoint.SPFeature.ActivateDeactivateFeatureAtWeb(Boolean fActivate, Boolean fEnsure, Guid featid, SPFeatureDefinition featdef, String urlScope, String sProperties, Boolean fForce)     at Microsoft.SharePoint.Administration.SPFeatureDefinition.ActivateInCentralAdmin()     at Microsoft.SharePoint.Administration.SPFeatureDefinition.Install(SPSite site, String solutionHash)     at Microsoft.SharePoint.Administration.SPFeatureDefinitionCollection.AddCore(SPFeatureDefinition featdef, SPSite site, String solutionHash, Boolean fForce, Boolean fDoValidation, String pathToFeatureAndElementManifestXsdFile)     at Microsoft.SharePoint.Administration.SPFeatureDefinitionCollection.AddInternal(String relativePathToFeatureManifest, Int32 compatibilityLevel, String pathToFeatureAndElementManifestXsdFile, Guid solutionId, String solutionHash, SPSite site, Boolean force, Boolean fDoValidation, SPFeatureDefinitionContext featureDefinitionContext)     at Microsoft.SharePoint.Administration.SPFeatureDefinitionCollection.ScanForFeaturesCore(Guid solid, Boolean fScanOnly, Boolean fForce, ArrayList& rgpathFailedFeatures)     at Microsoft.SharePoint.PowerShell.SPCmdletInstallFeature.FindAndInstallFeatures(Guid solutionId, Boolean bDisplayOnly, Boolean bForce, ArrayList& pathFailedFeatures)     at Microsoft.SharePoint.PowerShell.SPCmdletInstallFeature.RetrieveDataObjects()     at Microsoft.SharePoint.PowerShell.SPGetCmdletBase`1.InternalProcessRecord()     at Microsoft.SharePoint.PowerShell.SPCmdlet.ProcessRecord()     at System.Management.Automation.CommandProcessor.ProcessRecord()     at System.Management.Automation.CommandProcessorBase.DoExecute()     at System.Management.Automation.Internal.PipelineProcessor.SynchronousExecuteEnumerate(Object input, Hashtable errorResults, Boolean enumerate)     at System.Management.Automation.PipelineOps.InvokePipeline(Object input, Boolean ignoreInput, CommandParameterInternal[][] pipeElements, CommandBaseAst[] pipeElementAsts, CommandRedirection[][] commandRedirections, FunctionContext funcContext)     at System.Management.Automation.Interpreter.ActionCallInstruction`6.Run(InterpretedFrame frame)     at System.Management.Automation.Interpreter.EnterTryCatchFinallyInstruction.Run(InterpretedFrame frame)     at System.Management.Automation.Interpreter.EnterTryCatchFinallyInstruction.Run(InterpretedFrame frame)     at System.Management.Automation.Interpreter.EnterTryCatchFinallyInstruction.Run(InterpretedFrame frame)     at System.Management.Automation.Interpreter.Interpreter.Run(InterpretedFrame frame)     at System.Management.Automation.Interpreter.LightLambda.RunVoid1[T0](T0 arg0)     at System.Management.Automation.PSScriptCmdlet.RunClause(Action`1 clause, Object dollarUnderbar, Object inputToProcess)     at System.Management.Automation.PSScriptCmdlet.DoProcessRecord()     at System.Management.Automation.CommandProcessor.ProcessRecord()     at System.Management.Automation.CommandProcessorBase.DoExecute()     at System.Management.Automation.Internal.PipelineProcessor.SynchronousExecuteEnumerate(Object input, Hashtable errorResults, Boolean enumerate)     at System.Management.Automation.PipelineOps.InvokePipeline(Object input, Boolean ignoreInput, CommandParameterInternal[][] pipeElements, CommandBaseAst[] pipeElementAsts, CommandRedirection[][] commandRedirections, FunctionContext funcContext)     at System.Management.Automation.Interpreter.ActionCallInstruction`6.Run(InterpretedFrame frame)     at System.Management.Automation.Interpreter.EnterTryCatchFinallyInstruction.Run(InterpretedFrame frame)     at System.Management.Automation.Interpreter.EnterTryCatchFinallyInstruction.Run(InterpretedFrame frame)     at System.Management.Automation.Interpreter.Interpreter.Run(InterpretedFrame frame)     at System.Management.Automation.Interpreter.LightLambda.RunVoid1[T0](T0 arg0)     at System.Management.Automation.DlrScriptCommandProcessor.RunClause(Action`1 clause, Object dollarUnderbar, Object inputToProcess)     at System.Management.Automation.DlrScriptCommandProcessor.Complete()     at System.Management.Automation.CommandProcessorBase.DoComplete()     at System.Management.Automation.Internal.PipelineProcessor.DoCompleteCore(CommandProcessorBase commandRequestingUpstreamCommandsToStop)     at System.Management.Automation.Internal.PipelineProcessor.SynchronousExecuteEnumerate(Object input, Hashtable errorResults, Boolean enumerate)     at System.Management.Automation.Runspaces.LocalPipeline.InvokeHelper()     at System.Management.Automation.Runspaces.LocalPipeline.InvokeThreadProc()     at System.Management.Automation.Runspaces.PipelineThread.WorkerProc()     at System.Threading.ExecutionContext.RunInternal(ExecutionContext executionContext, ContextCallback callback, Object state, Boolean preserveSyncCtx)     at System.Threading.ExecutionContext.Run(ExecutionContext executionContext, ContextCallback callback, Object state, Boolean preserveSyncCtx)     at System.Threading.ExecutionContext.Run(ExecutionContext executionContext, ContextCallback callback, Object state)     at System.Threading.ThreadHelper.ThreadStart()	06052868-4010-4421-91cf-6af1e39160e2\
10/27/2015 17:41:25.00	powershell.exe (0x10C8)	0x1104	Web Content Management	Publishing Provisioning	6wz8	Medium	Adding list Url='Lists/Content Deployment Paths', Title='\$Resources:cmscore,CDPathsListName;' for feature Id='00bfea71-de22-43b2-a848-c05709900100'	06052868-4010-4421-91cf-6af1e39160e2



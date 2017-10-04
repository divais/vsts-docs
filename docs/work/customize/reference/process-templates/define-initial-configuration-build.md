---
title: Define the initial configuration of Team Foundation Build | VSTS & TFS
description: Customize the security permissions for build activities of all team projects that are created with the same process template - Team Foundation Server (TFS)
ms.prod: visual-studio-tfs-dev14
ms.technology: vs-devops-wit
ms.assetid: 93d86d59-dd49-43de-9bab-f4a9e17071a1
ms.manager: douge
ms.author: kaelli
ms.date: 09/08/2017
---


# Define the initial configuration of Team Foundation Build

[!INCLUDE [temp](../../../_shared/customization-phase-0-and-1-plus-version-header.md)]

You can customize a team project's initial build permissions that are used by Team Foundation Build. The build.xml plug-in file specifies the security permissions for build activities of all team projects that are created with the same process template. The following template files are uploaded to the team project database:  
  
> [!NOTE]  
>  Starting with TFS 2013, the build.xml plug-in no longer uploads build template files.  
   
  
The names of the file, the folder, and the plug-in for the default process templates are as follows:  
    
**File name**: Build.xml  
**Folder name**: Build 
**Plug-in name**: Microsoft.ProjectCreationWizard.Build 

  
> [!NOTE]  
> You can change the name of the XML file and the folder name but not the name of the plug-in. The system doesn't include a mechanism for the deployment of client-side plug-ins, policies, or other modifications. If you want to deploy this kind of functionality, you must use your own distribution and installation program.  
  
 In the Build.xml file, you specify one or more tasks and their dependencies. The TSF plug-in file specifies the permissions that are assigned to specific TFS default groups. For information about how to customize the initial security configuration for Lab Management, see [Control access to functional areas](control-access-to-functional-areas.md). For more information about the **task**, **taskXml**, and **dependency** elements, see [Define the tasks to process a plug-in](define-tasks-to-process-a-plug-in.md) and [Define dependencies for task groups and tasks](define-dependencies-plug-ins-groups-tasks.md).  
  
 The following code represents the default build.xml file that is defined for the TFS process templates:  
  
> [!div class="tabbedCodeSnippets"]
```XML 
<?xml version="1.0" encoding="utf-8"?>  
<tasks>  
  <task id="BuildTask" name="Add Build Permissions" plugin="Microsoft.ProjectCreationWizard.Build" completionMessage="Build tasks completed.">  
    <dependencies />  
    <taskXml>  
      <!-- Project-level groups -->  
      <Permission allow="ViewBuilds, ViewBuildDefinition" identity="[$$PROJECTNAME$$]\Readers" />  
      <Permission allow="EditBuildQuality, ViewBuilds, QueueBuilds, ViewBuildDefinition" identity="[$$PROJECTNAME$$]\Contributors" />  
      <Permission allow="DeleteBuilds, DestroyBuilds, EditBuildQuality, ManageBuildQualities, RetainIndefinitely, ViewBuilds, ManageBuildQueue, QueueBuilds, StopBuilds, DeleteBuildDefinition, EditBuildDefinition, ViewBuildDefinition, AdministerBuildPermissions" identity="[$$PROJECTNAME$$]\Build Administrators" />  
      <Permission allow="DeleteBuilds, DestroyBuilds, EditBuildQuality, ManageBuildQualities, RetainIndefinitely, ViewBuilds, ManageBuildQueue, QueueBuilds, StopBuilds, DeleteBuildDefinition, EditBuildDefinition, ViewBuildDefinition, AdministerBuildPermissions" identity="[$$PROJECTNAME$$]\$$PROJECTADMINGROUP$$" />   
      <!-- Collection-level groups -->  
      <Permission allow="EditBuildQuality, ManageBuildQueue, OverrideBuildCheckInValidation, QueueBuilds, UpdateBuildInformation, ViewBuildDefinition, ViewBuilds" identity="$$PROJECTCOLLECTIONBUILDSERVICESGROUP$$" />  
      <Permission allow="ViewBuildDefinition, EditBuildDefinition, DeleteBuildDefinition, QueueBuilds, ManageBuildQueue, StopBuilds, ViewBuilds, EditBuildQuality, RetainIndefinitely, DeleteBuilds, ManageBuildQualities, DestroyBuilds, AdministerBuildPermissions" identity="$$PROJECTCOLLECTIONBUILDADMINSGROUP$$" />  
      <Permission allow="DeleteBuilds, DestroyBuilds, EditBuildQuality, ManageBuildQualities, RetainIndefinitely, ViewBuilds, ManageBuildQueue, QueueBuilds, StopBuilds, DeleteBuildDefinition, EditBuildDefinition, ViewBuildDefinition, AdministerBuildPermissions, OverrideBuildCheckInValidation" identity="$$PROJECTCOLLECTIONADMINGROUP$$" />  
    </taskXml>  
  </task>  
</tasks>  
```  
  
## Related notes  
- [Control access to functional areas](control-access-to-functional-areas.md)
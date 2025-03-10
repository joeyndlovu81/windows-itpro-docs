---
title: Audit Authorization Policy Change (Windows 10)
description: The policy setting, Audit Authorization Policy Change, determines if audit events are generated when specific changes are made to the authorization policy.
ms.assetid: ca0587a2-a2b3-4300-aa5d-48b4553c3b36
ms.reviewer: 
manager: aaroncz
ms.author: vinpa
ms.pagetype: security
ms.prod: windows-client
ms.mktglfcycl: deploy
ms.sitesec: library
ms.localizationpriority: none
author: vinaypamnani-msft
ms.date: 09/06/2021
ms.technology: itpro-security
---

# Audit Authorization Policy Change

Audit Authorization Policy Change allows you to audit assignment and removal of user rights in user right policies, changes in security token object permission, resource attributes changes and Central Access Policy changes for file system objects.

| Computer Type     | General Success | General Failure | Stronger Success | Stronger Failure | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|-------------------|-----------------|-----------------|------------------|------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Domain Controller | IF             | No              | IF              | No               | IF – With Success auditing for this subcategory, you can get information related to changes in user rights policies, or changes of resource attributes or Central Access Policy applied to file system objects.<br>However, if you are using an application or system service that makes changes to system privileges through the AdjustPrivilegesToken API, we do not recommend Success auditing because of the high volume of event “[4703](event-4703.md)(S): A user right was adjusted” that may be generated. As of Windows 10, event 4703 is generated by applications or services that dynamically adjust token privileges. An example of such an application is Microsoft Endpoint Configuration Manager, which makes WMI queries at recurring intervals and quickly generates a large number of 4703 events (with the WMI activity listed as coming from **svchost.exe**).<br>If one of your applications or services is generating a large number of 4703 events, you might find that your event-management software has filtering logic that can automatically discard the recurring events, which would make it easier to work with Success auditing for this category.<br>This subcategory doesn’t have Failure events, so there is no recommendation to enable Failure auditing for this subcategory. |
| Member Server     | IF             | No              | IF              | No               | IF – With Success auditing for this subcategory, you can get information related to changes in user rights policies, or changes of resource attributes or Central Access Policy applied to file system objects.<br>However, if you are using an application or system service that makes changes to system privileges through the AdjustPrivilegesToken API, we do not recommend Success auditing because of the high volume of event “[4703](event-4703.md)(S): A user right was adjusted” that may be generated. As of Windows 10, event 4703 is generated by applications or services that dynamically adjust token privileges. An example of such an application is Microsoft Endpoint Configuration Manager, which makes WMI queries at recurring intervals and quickly generates a large number of 4703 events (with the WMI activity listed as coming from **svchost.exe**).<br>If one of your applications or services is generating a large number of 4703 events, you might find that your event-management software has filtering logic that can automatically discard the recurring events, which would make it easier to work with Success auditing for this category.<br>This subcategory doesn’t have Failure events, so there is no recommendation to enable Failure auditing for this subcategory. |
| Workstation       | IF             | No              | IF              | No               | IF – With Success auditing for this subcategory, you can get information related to changes in user rights policies, or changes of resource attributes or Central Access Policy applied to file system objects.<br>However, if you are using an application or system service that makes changes to system privileges through the AdjustPrivilegesToken API, we do not recommend Success auditing because of the high volume of event “[4703](event-4703.md)(S): A user right was adjusted” that may be generated. As of Windows 10, event 4703 is generated by applications or services that dynamically adjust token privileges. An example of such an application is Microsoft Endpoint Configuration Manager, which makes WMI queries at recurring intervals and quickly generates a large number of 4703 events (with the WMI activity listed as coming from **svchost.exe**).<br>If one of your applications or services is generating a large number of 4703 events, you might find that your event-management software has filtering logic that can automatically discard the recurring events, which would make it easier to work with Success auditing for this category.<br>This subcategory doesn’t have Failure events, so there is no recommendation to enable Failure auditing for this subcategory. |

**Events List:**

-   [4703](event-4703.md)(S): A user right was adjusted.

-   [4704](event-4704.md)(S): A user right was assigned.

-   [4705](event-4705.md)(S): A user right was removed.

-   [4670](event-4670.md)(S): Permissions on an object were changed.

-   [4911](event-4911.md)(S): Resource attributes of the object were changed.

-   [4913](event-4913.md)(S): Central Access Policy on the object was changed.

**Event volume**: Medium to High.


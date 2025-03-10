---
title: Using the SUA Tool (Windows 10)
description: The Standard User Analyzer (SUA) tool can test applications and monitor API calls to detect compatibility issues with the User Account Control (UAC) feature.
ms.reviewer: 
manager: aaroncz
ms.author: frankroj
ms.prod: windows-client
author: frankroj
ms.date: 10/28/2022
ms.topic: article
ms.technology: itpro-deploy
---

# Using the SUA Tool

**Applies to**

-   Windows 10
-   Windows 8.1
-   Windows 8
-   Windows 7
-   Windows Server 2012
-   Windows Server 2008 R2

By using the Standard User Analyzer (SUA) tool, you can test your applications and monitor API calls to detect compatibility issues with the User Account Control (UAC) feature.

The SUA Wizard also addresses UAC-related issues. In contrast to the SUA tool, the SUA Wizard guides you through the process step by step, without the in-depth analysis of the SUA tool. For information about the SUA Wizard, see [Using the SUA Wizard](using-the-sua-wizard.md).

In the SUA tool, you can turn virtualization on and off. When you turn virtualization off, the tested application may function more like the way it does in earlier versions of Windows®.

In the SUA tool, you can choose to run the application as **Administrator** or as **Standard User**. Depending on your selection, you may locate different types of UAC-related issues.

## Testing an Application by Using the SUA Tool

Before you can use the SUA tool, you must install Application Verifier. You must also install the Microsoft® .NET Framework 3.5 or later.

The following flowchart shows the process of using the SUA tool.

![act sua flowchart.](images/dep-win8-l-act-suaflowchart.jpg)

**To collect UAC-related issues by using the SUA tool**

1.  Close any open instance of the SUA tool or SUA Wizard on your computer.

    If there is an existing SUA instance on the computer, the SUA tool opens in log viewer mode instead of normal mode. In log viewer mode, you cannot start applications, which prevents you from collecting UAC issues.

2.  Run the Standard User Analyzer.

3.  In the **Target Application** box, browse to the executable file for the application that you want to analyze, and then double-click to select it.

4.  Clear the **Elevate** check box, and then click **Launch**.

    If a **Permission denied** dialog box appears, click **OK**. The application starts, despite the warning.

5.  Exercise the aspects of the application for which you want to gather information about UAC issues.

6.  Exit the application.

7.  Review the information from the various tabs in the SUA tool. For information about each tab, see [Tabs on the SUA Tool Interface](tabs-on-the-sua-tool-interface.md).

**To review and apply the recommended mitigations**

1.  In the SUA tool, on the **Mitigation** menu, click **Apply Mitigations**.

2.  Review the recommended compatibility fixes.

3.  Click **Apply**.

    The SUA tool generates a custom compatibility-fix database and automatically applies it to the local computer, so that you can test the fixes to see whether they worked.

## Related topics
[Tabs on the SUA Tool Interface](tabs-on-the-sua-tool-interface.md)

[Showing Messages Generated by the SUA Tool](showing-messages-generated-by-the-sua-tool.md)

[Applying Filters to Data in the SUA Tool](applying-filters-to-data-in-the-sua-tool.md)

[Fixing Applications by Using the SUA Tool](fixing-applications-by-using-the-sua-tool.md)
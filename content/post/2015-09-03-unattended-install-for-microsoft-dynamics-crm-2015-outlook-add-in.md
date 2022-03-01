---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2015-09-03T13:42:01Z"
guid: http://trukhanov.com/?p=109
id: 109
tags:
- Microsoft CRM
- SCCM
title: Unattended install for Microsoft Dynamics CRM 2015 Outlook Add-In
url: /2015/09/unattended-install-for-microsoft-dynamics-crm-2015-outlook-add-in/
---
To make fully unattended install package you need to do following steps:

1. Download install package: <http://www.microsoft.com/en-us/download/details.aspx?id=45015>

2. Extract package content:  
`CRM2015-Client-ENU-i386.exe /extract:c:\CRM_TEMP\1\2`  
Do not remove sub folders 1 and 2 from the path, because we need it later

3. Create Redist folder in c:\CRM_TEMP. Do not put it in the sub folders
<!--more-->
4. Create following sub folders in Redist folder:  
```cmd
cd c:\CRM_TEMP\Redist
mkdir dotNETFX
mkdir ReportViewer
mkdir SQLCE
mkdir SQLNativeClient
mkdir SQLSystemCLRTypes
mkdir VCRedist
mkdir VCRedist10
mkdir WindowsIdentityFoundation
```

5. In each folder download [CRM prerequisites](https://technet.microsoft.com/en-us/library/jj126125.aspx)

**dotNETFX:** [NDP452-KB2901907-x86-x64-AllOS-ENU.exe](http://download.microsoft.com/download/E/2/1/E21644B5-2DF2-47C2-91BD-63C560427900/NDP452-KB2901907-x86-x64-AllOS-ENU.exe)

**ReportViewer:** [ReportViewer.msi](http://download.microsoft.com/download/F/B/7/FB728406-A1EE-4AB5-9C56-74EB8BDDF2FF/ReportViewer.msi)

**SQLCE:** [SSCERuntime_x64-ENU.exe](http://download.microsoft.com/download/F/F/D/FFDF76E3-9E55-41DA-A750-1798B971936C/ENU/SSCERuntime_x64-ENU.exe);  [SSCERuntime_x86-ENU.exe](http://download.microsoft.com/download/F/F/D/FFDF76E3-9E55-41DA-A750-1798B971936C/ENU/SSCERuntime_x86-ENU.exe)

**SQLNativeClient (not mentioned in prerequisite article, but required):** [sqlncli.msi (x64)](http://download.microsoft.com/download/3/A/6/3A632674-A016-4E31-A675-94BE390EA739/ENU/x64/sqlncli.msi)[sqlncli.msi (x86)](http://download.microsoft.com/download/3/A/6/3A632674-A016-4E31-A675-94BE390EA739/ENU/x86/sqlncli.msi) &#8211; rename file to sqlncli_x86.msi

**SQLSystemCLRTypes:** [SQLSysClrTypes.msi (x86)](http://download.microsoft.com/download/9/2/C/92CBE583-5250-4AFD-A87A-DA707065CB9E/ENU/x86/SQLSysClrTypes.msi) &#8211; rename file to SQLSysClrTypes_x86.msi; [SQLSysClrTypes.msi (x64)](http://download.microsoft.com/download/9/2/C/92CBE583-5250-4AFD-A87A-DA707065CB9E/ENU/x64/SQLSysClrTypes.msi) &#8211; rename file to SQLSysClrTypes_x64.msi

**VCRedist:** [vcredist_x64.exe](http://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x64.exe);  [vcredist_x86.exe](http://download.microsoft.com/download/2/E/6/2E61CFA4-993B-4DD4-91DA-3737CD5CD6E3/vcredist_x86.exe)

**VCRedist10:** [vcredist_x86.exe](http://download.microsoft.com/download/C/6/D/C6D0FD4E-9E53-4897-9B91-836EBA2AACD3/vcredist_x86.exe);  [vcredist_x64.exe](http://download.microsoft.com/download/A/8/0/A80747C3-41BD-45DF-B505-E9710D2744E0/vcredist_x64.exe)

**WindowsIdentityFoundation:** [Windows6.1-KB974405-x86.msu](http://download.microsoft.com/download/D/7/2/D72FD747-69B6-40B7-875B-C2B40A6B2BDD/Windows6.1-KB974405-x86.msu); [Windows6.1-KB974405-x64.msu](http://download.microsoft.com/download/D/7/2/D72FD747-69B6-40B7-875B-C2B40A6B2BDD/Windows6.1-KB974405-x64.msu)

Also if you need optional offline capability create Microsoft SQL Express sub folders:

**SQLExpr:** [SQLEXPR\_x86\_ENU.exe](http://download.microsoft.com/download/0/1/E/01E0D693-2B4F-4442-9713-27A796B327BD/SQLEXPR_x86_ENU.exe)

**SQLExprRequiredSp:** [SQLEXPR\_x86\_ENU.exe](http://download.microsoft.com/download/0/F/D/0FD88169-F86F-46E1-8B3B-56C44F6E9505/SQLEXPR_x86_ENU.exe)

So the final directory tree should look like:

```
dotNETFX
    NDP452-KB2901907-x86-x64-AllOS-ENU.exe
ReportViewer
    ReportViewer.msi
SQLCE
    SSCERuntime_x64-ENU.exe
    SSCERuntime_x86-ENU.exe
SQLNativeClient
    sqlncli_x64.msi
    sqlncli_x86.msi
SQLSystemCLRTypes
    SQLSysClrTypes_x64.msi
    SQLSysClrTypes_x86.msi
VCRedist
    vcredist_x64.exe
    vcredist_x86.exe
VCRedist10
    vcredist_x64.exe
    vcredist_x86.exe
WindowsIdentityFoundation
    Windows6.1-KB974405-x64.msu
    Windows6.1-KB974405-x86.msu
```

This is the directory tree without offline capability prerequisites.

6. Now we are ready to make test deployment with the following command:

`C:\CRM_TEMP\1\2\SetupClient.exe /q /targetdir "C:\Program Files\Microsoft Dynamics CRM" /lv "C:\Users\Default\AppData\Local\Microsoft\MSCRM\Logs\SCCMcrmsetup.log"`

There is also possibility to create MSI file for deployment, but it is a little bit buggy and not completely unattended. So if you even run the MSI file with /qn key it still starts SetupClient.exe without /q key which tries to show you a dialog to accept license.

You need to keep such sub folder structure, because SetupClient hardcoded to search prerequisites in ../../Redist folder.

From now you can deploy CRM Client with SCCM. Just add c:\CRM_TEMP (or folder where you&#8217;ve moved entire sub folder structure to) as source folder and install with

`1\2\SetupClient.exe /q /targetdir "C:\Program Files\Microsoft Dynamics CRM" /lv "C:\Users\Default\AppData\Local\Microsoft\MSCRM\Logs\SCCMcrmsetup.log"`  
command.

**UPD: **Silent installer may fail (or just hang without any activity) if selected log directory does not exist, so it is better to replace mentioned above command with:

`1\2\SetupClient.exe /q /targetdir "C:\Program Files\Microsoft Dynamics CRM" /lv "%TEMP%\SCCMcrmsetup.log"`  
command.
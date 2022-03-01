---
author: Dmitry Trukhanov
categories:
- TSM
date: "2015-04-29T15:38:11Z"
guid: http://trukhanov.com/?p=90
id: 90
tags:
- TSM
title: Backing up DAG cluster with TSM
url: /2015/04/backing-up-dag-cluster-with-tsm/
---
There is a problem with DAG cluster backup in TSM. When you create script with /PREFERDAGPASSIVE key, your script return error 464 &#8211; nothing to backup. So I&#8217;ve created script to analyze Exchange&#8217;s databases if there is healthy passive copy of database to backup, so you run backup for the only healthy copy of database and for passive healthy copies of database. If there is nothing to backup than script returns 0.
<!--more-->
```powershell
$exc_dir="C:\Program Files\Tivoli\TSM\TDPExchange"
cd $exc_dir  
Get-Date -UFormat "%Y-%m-%d %T" | Out-File -Encoding utf8 -FilePath excfull.log -Append  
add-pssnapin Microsoft.Exchange.Management.PowerShell.E2010  
foreach ($maildatabase in Get-MailboxDatabase -Server $env:COMPUTERNAME)  
{  
    $copies=0;  
    $needtobackup=$False;  
    foreach($dbcopy in Get-MailboxDatabaseCopyStatus $maildatabase)  
    {  
        if(($dbcopy.Status -eq "Mounted") -or ($dbcopy.Status -eq "Healthy"))  
        {  
            $copies+=1  
        }  
        if((!$dbcopy.ActiveCopy) -and ($dbcopy.MailboxServer -eq $env:COMPUTERNAME))  
        {  
            $needtobackup=$True  
        }  
    }  
    if($copies -lt 2)  
    {  
        $needtobackup=$True  
    }  
}

if($needtobackup)  
{  
    .\tdpexcc.exe backup * full /PREFERDAGPASSIVE /tsmoptfile=dsm.opt /CONFIGfile=tdpexc.cfg /logfile=excsch.log | Out-File -Encoding utf8 -FilePath excfull.log -Append  
    $backupresult = $LASTEXITCODE  
    "Return code was $backupresult" | Out-File -Encoding utf8 -FilePath excfull.log -Append  
}else{  
    "Nothing to backup" | Out-File -Encoding utf8 -FilePath excfull.log -Append  
    $backupresult=0  
    "Return code was $backupresult" | Out-File -Encoding utf8 -FilePath excfull.log -Append  
}  
exit $backupresult
```

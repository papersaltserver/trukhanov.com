---
author: Dmitry Trukhanov
categories:
- ProblemFixing
- Windows Server
date: "2011-08-02T09:27:18Z"
guid: http://trukhanov.com/?p=5
id: 5
tags:
- event id 4135
- MS DTC
- windows server 2003
title: Fixing MSDTC errors 4135 4163 4185 4112
url: /2011/08/fixing-msdtc-errors/
---
One of clients had problem with MSDTC service on Windows 2003 server, which couldn&#8217;t start. After short investigation I have found corresponding errors in Application log:

> Event Type:    Error  
> Event Source:    MSDTC  
> Event Category:    LOG  
> Event ID:    4163  
> Date:        2011-08-02  
> Time:        12:02:08 PM  
> User:        N/A  
> Computer:    _servername_  
> Description:  
> MS DTC log file not found. After ensuring that all Resource Managers coordinated by MS DTC have no indoubt transactions, please run msdtc -resetlog to create the log file.
> 
> For more information, see Help and Support Center at http://go.microsoft.com/fwlink/events.asp.
<!--more-->
and

> Event Type: Error  
> Event Source: MSDTC  
> Event Category: TM  
> Event ID: 4185  
> Date: 2011-08-02  
> Time: 12:02:08 PM  
> User: N/A  
> Computer: _servername_  
> Description:  
> MS DTC Transaction Manager start failed. LogInit returned error 0x3.
> 
> For more information, see Help and Support Center at http://go.microsoft.com/fwlink/events.asp. 

and

> Event Type: Error  
> Event Source: MSDTC  
> Event Category: SVC  
> Event ID: 4112  
> Date: 2011-08-02  
> Time: 12:02:08 PM  
> User: N/A  
> Computer: _servername_  
> Description:  
> Could not start the MS DTC Transaction Manager.
> 
> For more information, see Help and Support Center at http://go.microsoft.com/fwlink/events.asp. 

Also System eventlog contained:

> Event Type: Error  
> Event Source: Service Control Manager  
> Event Category: None  
> Event ID: 7024  
> Date: 2011-08-02  
> Time: 12:19:26 PM  
> User: N/A  
> Computer: _servername_  
> Description:  
> The Distributed Transaction Coordinator service terminated with service-specific error 3221229584 (0xC0001010).
> 
> For more information, see Help and Support Center at http://go.microsoft.com/fwlink/events.asp. 

As it is recommended in first event and numerous forums I have tried to run:

`msdtc -resetlog`

but this command generated another error:

> Event Type: Error  
> Event Source: MSDTC  
> Event Category: SVC  
> Event ID: 4135  
> Date: 2011-08-02  
> Time: 12:01:51 PM  
> User: N/A  
> Computer: _servername_  
> Description:  
> Failed to create/reset the MS DTC log file.
> 
> For more information, see Help and Support Center at http://go.microsoft.com/fwlink/events.asp. 

after some search I have found several solutions to this problem, such as activating network access for DTC, but nothing helped. From colleagues I have learn that this server previously was in cluster but then cluster was abolished. So problem was with log file settings of MSDTC and I have decided to find out where Whindows store information about this files. After short search on healthy system I have found registry key **HKEY\_CLASSES\_ROOT\CID\{GUID}\CustomProperties\LOG\Path** where {GUID} some random GUID. Default value of this key pointed to MSDTC log file. After this I have looked through **HKEY\_CLASSES\_ROOT\CID\\** subkeys on problem server and found GUID stored similar settings. Log file settings here pointed to Q:\MsDtc, where Q: previously was quorum drive and not longer existed. After fixing this setting to point correct path, problem has gone.
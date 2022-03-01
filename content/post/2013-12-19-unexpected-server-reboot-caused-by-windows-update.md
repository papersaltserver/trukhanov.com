---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2013-12-19T11:29:16Z"
guid: http://trukhanov.com/?p=42
id: 42
title: Unexpected server reboot caused by windows update
url: /2013/12/unexpected-server-reboot-caused-by-windows-update/
---
Recently I&#8217;ve got unexpected Windows server 2008 R2 reboot with some recovery reason, which is quite uninformative:

> The process C:\Windows\system32\svchost.exe (_servername_) has initiated the restart of computer _servername_ on behalf of user NT AUTHORITY\SYSTEM for the following reason: Operating System: Recovery (Planned)  
> Reason Code: 0x80020002  
> Shutdown Type: restart  
> Comment:
<!--more-->
As time of reboot was very close to 03:00 AM (default windows update installation time) and System event log contained couple of events about update installation, I&#8217;ve decided to check WindowsUpdate.log file.

It has following:

> 2013-12-16    03:17:06:606    832 13c8    AU  AU invoking RebootSystem (OnRebootNow)  
> 2013-12-16    03:17:06:647    832 13c8    Misc    WARNING: SUS Client is rebooting system.  
> 2013-12-16    03:17:06:647    832 13c8    AU  AU invoking RebootSystem (OnRebootRetry)  
> 2013-12-16    03:17:06:651    832 13c8    AU  AU received handle event

So to eliminate such unexpected reboots disable automatic updates for production servers, or deny it automatically reboot server.
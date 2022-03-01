---
author: Dmitry Trukhanov
categories:
- Windows Server
date: "2014-01-10T12:47:16Z"
guid: http://trukhanov.com/?p=47
id: 47
tags:
- ems
- sac
- sol
- windows 2012
title: Enable Text Console (SOL) on Windows Server 2012
url: /2014/01/enable-text-console-sol-on-windows-server-2012/
---
After installing Windows 2012 on old Fujitsu Primergy RX200 we lost ability to manage it with serial console. We&#8217;ve even tried to update iRMC firmware with no luck. Problem was resolved after we enabled EMS (Emergency Management Services) with the following set of commands:

> BCDedit /bootems {Boot\_entry\_id} ON
> 
> bcdedit /ems on
> 
> bcdedit /emssettings EMSPORT:2 EMSBAUDRATE:115200
<!--more-->

Ensure that you&#8217;ve changed `Boot_entry_id` to your ID (you can get it with `bcdedit /v`), 2 to your COM port number (you can get it from your bios settings, or from irmc settings) and 115200 to your bauderate (you can get it from your bios settings, or from irmc settings).

Reboot and enjoy serial access to your server.
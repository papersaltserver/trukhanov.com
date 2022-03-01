---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2013-09-25T20:03:05Z"
guid: http://trukhanov.com/?p=21
id: 21
tags:
- EventID 1033
- msi
- Remote Desktop Session Host
- windows
title: Error 1602 or 1618 during install of any msi packet
url: /2013/09/error-1602-or-1618-during-install-of-any-msi-packet/
---
Recently we&#8217;ve got error installing driver on our Windows 2008 R2 server. It does not provide any specific error dialog, just constantly showing &#8220;Please wait while the application is preparing for the first use&#8221;. After you press Cancel, it gives you error message saying that installation could not be completed because other installation is in progress. The only diagnostic message in the Application log was EventID 1033 form MsiInstaller with message `Installation success or error status: 1602` or `Installation success or error status: 1618`. Which obviously not helpful. Internet search revealed, that this message is quite common and can be caused by many things.  
<!--more-->
Also I have found that installer created MSI*.log which contained the same diagnostic message about status 1602 or 1618, but it also contained another message `Failed to grab execution mutex. System error 258` which also does not look helpful, but quick search for this string found [forum conversation](http://community.flexerasoftware.com/showthread.php?192289-Problems-with-windows-server-2008-r2-and-terminalserver).  
The problem is that our server has Remote Desktop Session Host (Terminal services), which by default forbids chained installations. Chained installation is the method of installation when one msi file calls another one to install some prerequisites. Remote Desktop Session Host can be used by many users, which can try to install many MSI files simultaneously, which potentially can cause some conflicts and broken installations, so Remote Desktop Session Host by default permits only one installation at a time, all other installations are queued for sequential installations. Of cause it is not possible for chained installation, when you first MSI is running and is waiting subsequent msi file to complete, to queue subsequent installations. To fix this issue you need to use Group Policy, either local or domain.  
The required setting is:  
Computer Configuration->Administrative Templates->Windows Components->Remote Desktop Services->Remote Desktop Session Host->Application Compatibility->Turn off Windows Installer RDS Compatibility  
Just set it to &#8220;Enabled&#8221; and run `gpupdate /force`, and rerun your installation.
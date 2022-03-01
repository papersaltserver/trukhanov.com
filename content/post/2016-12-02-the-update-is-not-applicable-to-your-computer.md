---
author: Dmitry Trukhanov
categories:
- Windows Server
date: "2016-12-02T09:00:41Z"
guid: http://trukhanov.com/?p=136
id: 136
tags:
- update
- windows
title: '“The update is not applicable to your computer”'
url: /2016/12/the-update-is-not-applicable-to-your-computer/
---
Recently I&#8217;ve needed to apply 2 patches to our Windows 2008 R2. But both patches show me error &#8220;The update is not applicable to your computer&#8221;. But I knew for sure, that these updates were applicable, because installed file&#8217;s version was lower than the one contained in hotfixes.  
In Setup log I&#8217;ve got &#8220;Windows update could not be installed because of error 2149842967&#8221; error.

The problem was that someone disabled Windows Modules Installer service on this machine, which is TrustedInstaller.exe process. After enabling and running this service, I&#8217;ve installed both patches successfully.
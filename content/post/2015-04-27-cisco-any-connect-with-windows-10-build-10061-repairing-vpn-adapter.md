---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2015-04-27T10:41:57Z"
guid: http://trukhanov.com/?p=83
id: 83
tags:
- Windows 10
title: Cisco Any Connect with Windows 10 build 10061 Repairing vpn adapter
url: /2015/04/cisco-any-connect-with-windows-10-build-10061-repairing-vpn-adapter/
---
Today our network administrator applied latest patch to Cisco ASA. After that, any new connection with AnyConnect was starting update procedure, which failed every time even after reboot. I&#8217;ve uninstalled application and installed the newest version. After that any connection showed:

Repairing vpn adapter
<!--more-->
Which have failed each time. To fix this I&#8217;ve changed settings for Ethernet adapter used by AnyConnect to obtain IP address automatically. After that AnyConnect reinstalled virtual adapter and connected without any issue. My colleague fixed this problem with deletion of `%LOCALAPPDATA%\Cisco\` folder.
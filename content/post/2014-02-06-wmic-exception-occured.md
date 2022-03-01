---
author: Dmitry Trukhanov
categories:
- Windows Server
date: "2014-02-06T07:54:07Z"
guid: http://trukhanov.com/?p=58
id: 58
tags:
- windows
- wmic
title: Wmic exception occured
url: /2014/02/wmic-exception-occured/
---
Some time ago we have faced problem that our script which calls wmic to get some values stopped to work. After a short research we have found that on some servers wmic utility stopped to work, showing exception every time we run any command:

> wmic os get caption  
> ERROR:  
> Description = Exception occurred.
<!--more-->
In the same time wbemtest and powershell worked correctly. The solution was to add `/value` switch to a command:

> wmic os get caption /value  
> Caption=Microsoft Windows Server 2008 R2 Enterprise

For some reason wmic can&#8217;t format output as a table (the same error you can get with `/format: table` switch and get rid of it with `/format:list`), which is default, or can be done with `/all` switch. Hope this helps someone.
---
author: Dmitry Trukhanov
categories:
- TSM
date: "2015-03-30T12:53:04Z"
guid: http://trukhanov.com/?p=79
id: 79
tags:
- TSM
title: TSM ANS0106E error on backup or dsmc run
url: /2015/03/tsm-ans0106e-error-on-backup-or-dsmc-run/
---
Recently we&#8217;ve found that one of our TSM servers stopped to back up all virtual machines. After examination of dsmerror.log files we&#8217;ve found strange messages:

> 03/20/2015 09:48:42 ANS0106E Message index not found for message 2252.  
> 03/20/2015 09:49:01 ANS0106E Message index not found for message 14320.  
> 03/20/2015 09:49:01 ANS0106E Message index not found for message 14180.

And so on.
<!--more-->

After quick search we&#8217;ve found official [IBM article about this error](http://www-01.ibm.com/support/docview.wss?uid=swg21590949 "ANS0106E Message index not found for message xxxxx")

This article suggests you to update or re-install your Backup Archive Client, installed on the TSM server. We&#8217;ve tried that with different versions of client with no result. All clients give the same error: ANS0106E.

After that we&#8217;ve started to analyze dsmc.exe process wint ProcessExplorer and found strange thing, that this process have open handles to `C:\Program Files\Tivoli\TSM\server\tsmdiag` and not `C:\Program Files\Tivoli\TSM\baclient` where we&#8217;ve actually installed our BA Client.

So the problem was, that tsmdiag.exe utility set environment variables DSM\_CONFIG and DSM\_DIR, which affect Backup Archive Client behavior. So to resolve this problem you need to change mentioned environment variables or copy dscenu.txt file from your BAClient folder to tsmdiag folder.
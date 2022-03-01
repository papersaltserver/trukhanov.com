---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2013-11-14T19:10:32Z"
guid: http://trukhanov.com/?p=28
id: 28
tags:
- cluster
- mssql
title: Clustered MS SQL dependency on mounted volume
url: /2013/11/clustered-ms-sql-dependency-on-mounted-volume/
---
Recently I have added new mounted volume to Microsoft cluster (lets say disk 3 mounted at m:\history), and tried to restore some old backup to it. But after I have run SQL script I&#8217;ve immediately got an SQL error:
`Only formatted files on which the cluster resource of the server has a dependency can be used. Either the disk resource containing the file is not present in the cluster group or the cluster resource of the SQL Server does not have a dependency on it.`
<!--more-->
After short searches I&#8217;ve found couple of [links](http://support.microsoft.com/kb/295732/en-us) stating than I need to add my volume to clustered MS SQL service dependencies to operate with this volume. By the way Microsoft&#8217;s manuals does not clarify do you really need to bring clustered MS SQL server offline before adding new dependency or not. One instruction says that you should to do so and others say that you can just add new dependency without interruption.  
But after I&#8217;ve tried to follow this instruction the problem have not gone, and script showed the same error. I&#8217;ve tried to add source file&#8217;s drive (lets say disk 2 mounted to m:\backups) to dependencies as well without luck. The error was resolved only after I&#8217;ve added &#8220;root&#8221; volume M:. But this volume was previously added to original disk dependencies, so disk 3 mounted at m:\history was dependent on disk 1 (M:). So it seems clustered MS SQL does not look through dependency chain and just tests the drive which starts the file path, without worrying about real dependent volume. Hope this will save your time.
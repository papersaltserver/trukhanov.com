---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2017-06-17T13:04:10Z"
guid: http://trukhanov.com/?p=145
id: 145
tags:
- powershell
- windows
- wmi
title: Script for Windows storage audit
url: /2017/06/script-for-windows-storage-audit/
---
I&#8217;ve created PowerShell script to collect all important information about windows physical disks, partitions and volumes (logical disks) and its connections. 
<!--more-->
To use it you should have administrator right on target computer, otherwise only simple information about logical disks will be collected. This limitation is due to MSFT_PartitionToVolume WMI class, which is accessible only by local administrators.

[Audit script download](http://trukhanov.com/files/GetStorage.ps1)
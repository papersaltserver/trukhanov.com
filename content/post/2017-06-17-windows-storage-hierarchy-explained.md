---
author: Dmitry Trukhanov
categories:
- Windows Server
date: "2017-06-17T10:23:42Z"
guid: http://trukhanov.com/?p=142
id: 142
tags:
- windows
- wmi
title: Windows storage hierarchy explained
url: /2017/06/windows-storage-hierarchy-explained/
---
Quick cheat sheet:
1. Physical disk, represented by Win32\_DiskDrive and MSFT\_Disk WMI classes. This is hardware presented to your computer. It is HDD, SSD, FC LUN or iSCSI LUN.  
2. Partition, represented by Win32\_DiskPartition and MSFT\_Partition WMI classes. Partitions are walls that transforming your physical space into rooms, which later can be used to store your data. Physical disk can have 0 or more partitions.  
3. Volume, represented by Win32\_Volume and MSFT\_Volume WMI classes. Volumes are named spaces, from abstract rooms you get living room, kitchen and bedrooms. You format room to make it &#8220;named room&#8221;. Or in other words, you format partition to some file system, i.e. NTFS, FAT or ReFS. Volumes can store data and provide access to it. One partition can have 0 or 1 volume. When partition has 0 volume, in most cases this means that there is no data accessible by user.
<!--more-->
4. Logical Disks, represented by Win32_LogicalDisk. Logical disks are like doors to your named rooms. Without doors you can access your named rooms through windows (mount points) and ventilation (&#8220;start \\?\Volume{GUID}\&#8221; command), and of course it is not always convenient. So giving your volumes disk letter makes it Logical Disks. One volume equal to 0 or 1 logical disks.

 
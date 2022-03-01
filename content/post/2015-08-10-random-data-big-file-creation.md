---
author: Dmitry Trukhanov
categories:
- Windows Server
date: "2015-08-10T14:54:54Z"
guid: http://trukhanov.com/?p=100
id: 100
title: Random data big file creation
url: /2015/08/random-data-big-file-creation/
---
Sometimes you just need a big file. For example to test network speed, of backup speed. And to prevent software of hardware compression you need this file to be absolutely random. Also you do not want to write this file byte by byte, as it can be really slow. Here is my version of powershell script to generate such file:
<!--more-->
```powershell
$chunksize=2*1024*1024 #I use 2 Mb chunks
$filesize = 50*1024*1024*1024 # File size in bytes
[Byte[]]$randombuffer=@(0)*$chunksize #First of all we create buffer filled with zeroes
$Random = New-Object System.Random #Our random number generator
$myfile = New-Object IO.FileStream "c:\testfile", 'Append' #Replace c:\testfile with the proper path
1..($filesize/$chunksize) | %{$Random.NextBytes($randombuffer);$myfile.Write($randombuffer,0,$randombuffer.Length)};$myfile.Close() #Magic
```


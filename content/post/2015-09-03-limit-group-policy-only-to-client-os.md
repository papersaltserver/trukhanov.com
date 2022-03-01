---
author: Dmitry Trukhanov
categories:
- Active Directory
date: "2015-09-03T12:49:13Z"
guid: http://trukhanov.com/?p=107
id: 107
tags:
- Active Directory
- Group policy
- wmi
title: Limit group policy only to client OS
url: /2015/09/limit-group-policy-only-to-client-os/
---
Sometimes you want to apply your policy to every client PC in many OUs. To do such you can use WMI filter. This is especially useful in messy AD structures, where is no dedicated OU for client computer objects.

```
Namespace: root\CIMv2
Query: select * from Win32_OperatingSystem where ProductType="1"
```
<!--more-->
I use here following WMI Class and property:  
<https://msdn.microsoft.com/en-us/library/aa394239(v=vs.85).aspx#properties>

**ProductType**  
Data type: **uint32**  
Access type: Read-only  
Additional system information.  
**Work Station** (1)  
**Domain Controller** (2)  
**Server** (3)

---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2012-02-16T07:25:17Z"
guid: http://trukhanov.com/?p=14
id: 14
tags:
- error 1053
- NetBackUP
- Netbackup client service
- timeout
title: Error 1053 while starting NetBackUP Client Service
url: /2012/02/error-1053-while-starting-netbackup-client-service/
---
Recently I have stuck with error 1053 `The service did not respond to the start or control request in a timely fashion` while starting NetBackUP Client Service. But the error appeared much faster then 30 seconds which is actual service timeout, so the message was incorrect. The problem was that affected server was joined into MSCS to host SQL server group. So to backup MSSQL we run NetBackUP Client Service as domain user with rights to access SQL bases. To start service correctly domain user must have local server administrator right. After I&#8217;ve added this user to local administrator&#8217;s group error gone.
<!--more-->
See you next time
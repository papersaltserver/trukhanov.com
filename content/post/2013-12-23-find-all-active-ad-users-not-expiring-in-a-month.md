---
author: Dmitry Trukhanov
categories:
- Active Directory
- Windows Server
date: "2013-12-23T10:11:13Z"
guid: http://trukhanov.com/?p=44
id: 44
tags:
- acitve directory
- dsquery
title: Find all active AD users not expiring in a month
url: /2013/12/find-all-active-ad-users-not-expiring-in-a-month/
---
Sometimes your AD has a lots of temporary users, with accounts expiring in near future, and to ensure that only legitimate users have never expiring or long expiring accounts you want to audit your accounts. To do so, perform next:  
1. Create test account expiring today
<!--more-->
2. With ADSI edit get the value of `accountExpires` property
3. Add to this value Number\_of\_days*864000000000, write down this value (actually you can create test account expiring on required date, but it is not so fun)  
4. Run the following command:
`dsquery * -filter "(&(objectCategory=person)(objectClass=user)(accountExpires>=130344624000000000)(!(userAccountControl:1.2.840.113556.1.4.803:=2)))"`
where `130344624000000000` is the number from step 3 and `!(userAccountControl:1.2.840.113556.1.4.803:=2)` means that we only want to find enabled users.

**UPD**

In some cases users with expire date &#8220;Never&#8221; have accountExpires=9223372036854775807 but in some cases it is equal to 0. So, correct search query will be:

`dsquery * -filter "(&(objectCategory=person)(objectClass=user)(|(accountExpires>=130720500000000000)(accountExpires=0))(!(userAccountControl:1.2.840.113556.1.4.803:=2)))" -limit 1000`
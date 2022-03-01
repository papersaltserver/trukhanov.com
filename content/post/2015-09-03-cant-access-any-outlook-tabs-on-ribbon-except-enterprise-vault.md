---
author: Dmitry Trukhanov
categories:
- Active Directory
date: "2015-09-03T12:37:40Z"
guid: http://trukhanov.com/?p=105
id: 105
tags:
- Active Directory
- GPP
- Microsoft CRM
title: Can’t access any Outlook tabs on ribbon except Enterprise Vault
url: /2015/09/cant-access-any-outlook-tabs-on-ribbon-except-enterprise-vault/
---
Recently one of our customers installed Microsoft Dynamics CRM Outlook add-in and discovered that after this action Microsoft Outlook became absolutely useless, because after start of the program there are no other tabs except Enterprise Vault, which is also used by this customer. Little research showed that the root of the problem is in some components of earlier version of Microsoft Office. To fix problem you need to delete following registry key:

`HKEY_CLASSES_ROOT\TypeLib\{2DF8D04C-5BFA-101B-BDE5-00AA0044DE52}\2.4`
<!--more-->

To automate this across organization you can use group policy preferences:

Computer Configuration -> Preferences -> Windows settings ->Registry -> New -> Registry Item:

```
Action: Delete
Hive: HKEY_CLASSES_ROOT
Key Path: TypeLib\{2DF8D04C-5BFA-101B-BDE5-00AA0044DE52}\2.4
```
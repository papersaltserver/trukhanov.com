---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2014-01-15T12:13:24Z"
guid: http://trukhanov.com/?p=55
id: 55
title: Find all VMWare VMs created in a given period
url: /2014/01/find-all-vmware-vms-created-in-a-given-period/
---
To find all virtual machines created in a given period use the following SQL script:

```sql
select ev.CREATE_TIME, EV.VM_NAME AS OldName  
from VPX_EVENT EV  
Where (EV.event_type = 'vim.event.VmDeployedEvent' OR EV.EVENT_TYPE = 'vim.event.VmCreatedEvent' OR EV.EVENT_TYPE = 'vim.event.VMClonedEvent') AND EV.CREATE_TIME >= cast('1.01.2013' AS DATETIME) AND EV.CREATE_TIME <= cast('31.01.2014' AS DATETIME)
```
<!--more-->
Output of this script will show all created VMs including deleted ones. To show only existing virtual machines you can use following SQL script:

```sql
select EV.CREATE_TIME, EV.VM_NAME AS OldName, INF.NAME AS CurrName  
from VPX_EVENT EV, VPX_VM_CONFIG_INFO INF  
Where EV.VM_ID=INF.ID AND (EV.event_type = 'vim.event.VmDeployedEvent' OR EV.EVENT_TYPE = 'vim.event.VmCreatedEvent' OR EV.EVENT_TYPE = 'vim.event.VMClonedEvent') AND EV.CREATE_TIME >= cast('01.01.2013' AS DATETIME)
```

As you can see it can help you to find renamed virtual machines as well. One of the biggest problem of this script is that it relies on events, which can be deleted by time when you run script. Unfortunately it is impossible to find the same data any other way. The only other solution is to use custom fields, which have to be created to the moment of need.

```sql
select ev.CREATE_TIME, EV.VM_NAME AS OldName  
from VPX_EVENT EV  
Where (EV.event_type = 'vim.event.VmDeployedEvent' OR EV.EVENT_TYPE = 'vim.event.VmCreatedEvent' OR EV.EVENT_TYPE = 'vim.event.VMClonedEvent') AND EV.CREATE_TIME >= cast('01.01.2013' AS DATETIME) AND EV.CREATE_TIME <= cast('31.01.2014' AS DATETIME)
```
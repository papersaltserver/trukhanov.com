---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2014-01-10T14:22:38Z"
guid: http://trukhanov.com/?p=53
id: 53
title: Restoring MSSQL database to new location from command line with IBM TSM
url: /2014/01/restoring-mssql-database-to-new-location-from-command-line-with-ibm-tsm/
---
Just a simple commands sequence with comments.

> tdpsqlc.exe q tsm SOURCE\_DB\_NAME f /all

write down Database Object Name for needed full backup.
<!--more-->
If you want to recover database backed up with options from dsm.opt file use following command:

> tdpsqlc restore SOURCE\_DB\_NAME  full /into=DESTINATION\_DB\_NAME/RELocate=DATA\_FILE\_NAME,LOG\_FILE\_NAME /to=PATH\_TO\_DATA\_FILE.mdf,PATH\_TO\_LOG\_FILE.ldf /RECOVery=no /replace /object=DATABASEOBJECTNAME

If you want to recover database backed up with options different from dsm.opt, use following:

> tdpsqlc restore SOURCE\_DB\_NAME full /into=DESTINATION\_DB\_NAME /RELocate=DATA\_FILE\_NAME,LOG\_FILE\_NAME /to=PATH\_TO\_DATA\_FILE.mdf,PATH\_TO\_LOG\_FILE.ldf /RECOVery=no /replace /object=DATABASEOBJECTNAME /tsmoptfile=other.opt
> 
> tdpsqlc.exe q tsm SOURCE\_DB\_NAME diff /all

Write down Database Object Name for needed diff backup

> tdpsqlc restore SOURCE\_DB\_NAME diff /into=DESTINATION\_DB\_NAME /RELocate=DATA\_FILE\_NAME,LOG\_FILE\_NAME /to=PATH\_TO\_DATA\_FILE.mdf,PATH\_TO\_LOG\_FILE.ldf /RECOVery=no /replace /object=DATABASEOBJECTNAME
> 
> tdpsqlc.exe q tsm SOURCE\_DB\_NAME log=* /all

Write down Database Object Names for all needed log backups

> tdpsqlc restore SOURCE\_DB\_NAME log=* /into=DESTINATION\_DB\_NAME /RELocate=DATA\_FILE\_NAME,LOG\_FILE\_NAME /to=PATH\_TO\_DATA\_FILE.mdf,PATH\_TO\_LOG\_FILE.ldf /RECOVery=no /object=DATABASEOBJECTNAME

&#8230;repeat for all needed logs

> tdpsqlc restore SOURCE\_DB\_NAME log=* /into=DESTINATION\_DB\_NAME /RELocate=DATA\_FILE\_NAME,LOG\_FILE\_NAME /to=PATH\_TO\_DATA\_FILE.mdf,PATH\_TO\_LOG\_FILE.ldf /RECOVery=yes /object=DATABASEOBJECTNAME

Do not forget to add recovery=yes to last restored log.
---
author: Dmitry Trukhanov
categories:
- Backup
- MSSQL
date: "2014-01-10T13:37:16Z"
guid: http://trukhanov.com/?p=51
id: 51
tags:
- backup
- mssql
- script
title: Listing all MSSQL performed for specific database
url: /2014/01/listing-all-mssql-performed-for-specific-database/
---
Yesterday I&#8217;ve stuck with restoring some MSSQL database, because of incompatibility between used differential and full backups, after I&#8217;ve found correct full backup, restore process completed without any problem. The following error identifies that:

> The log or differential backup cannot be restored because no files are ready to rollforward
<!--more-->
The following script helped me to find out correct backup chain:

```sql
SELECT
TOP 1000
s.database_name,
CASE s.type
WHEN 'D' THEN 'Full'
WHEN 'I' THEN 'Differential'
WHEN 'L' THEN 'Transaction Log'
END AS BackupType,
CAST(DATEDIFF(second, s.backup_start_date,
s.backup_finish_date) AS VARCHAR(4)) + ' Seconds' TimeTaken,
s.backup_start_date,
CAST(s.first_lsn AS VARCHAR(50)) AS first_lsn,
CAST(s.last_lsn AS VARCHAR(50)) AS last_lsn,
m.physical_device_name,
CAST(CAST(s.backup_size / 1000000 AS INT) AS VARCHAR(14)) + ' MB' AS bkSize,
s.server_name,
s.recovery_model
FROM msdb.dbo.backupset s
INNER JOIN msdb.dbo.backupmediafamily m ON s.media_set_id = m.media_set_id
WHERE s.database_name = DB_NAME() -- Remove this line for all the database
ORDER BY backup_start_date DESC, backup_finish_date
GO
```
You might want to replace TOP 100o with bigger value if you have a lots of backups.
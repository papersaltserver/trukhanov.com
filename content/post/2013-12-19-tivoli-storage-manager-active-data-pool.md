---
author: Dmitry Trukhanov
categories:
- Backup
- TSM
date: "2013-12-19T11:07:38Z"
guid: http://trukhanov.com/?p=35
id: 35
tags:
- active data pool
- TSM
title: TSM Active Data pool file number
url: /2013/12/tivoli-storage-manager-active-data-pool/
---
One of the strange features of TSM AD pool is that, when you do first incremental backup (for example 10 files), then change some data (for example 5 files) you&#8217;ll get incorrect number of files with `query occupancy stg=adpool` command. It will show you number of files equal to 15, the same as in your primary storage pool. But if you compare sizes of pool you&#8217;ll see a difference. So do not rely only on file number in your AD storage pool.
<!--more-->

See you next time
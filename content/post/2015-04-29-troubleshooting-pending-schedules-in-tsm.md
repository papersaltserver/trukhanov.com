---
author: Dmitry Trukhanov
categories:
- TSM
date: "2015-04-29T15:09:32Z"
guid: http://trukhanov.com/?p=87
id: 87
tags:
- TSM
title: Troubleshooting Pending schedules in TSM
url: /2015/04/troubleshooting-pending-schedules-in-tsm/
---
Today we&#8217;ve found that one of our servers can make backups manually and works fine with polling mode, but when it is switched to prompting mode then schedule never run. After some investigation we&#8217;ve found <http://www.tsmblog.org/tag/tcpclientport/> because we thought we have a problem with port of CAD. 
<!--more-->
We&#8217;ve found that TSM scheduler does not operate with data from NODES table, which contain correct names and IP addresses, but instead it uses SCHEDULE\_NODE\_ADDRESSES table, which only contains host names and ports. Problem server was in different AD forest and TSM sever did not have correct DNS search suffixes. After fixing DNS issues all Pending schedules disappeared.
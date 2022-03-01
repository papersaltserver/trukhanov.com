---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2015-07-14T12:21:56Z"
guid: http://trukhanov.com/?p=96
id: 96
tags:
- rpc
- tmg
- wmi
title: TMG 2010 and “RPC Server unavailable”
url: /2015/07/tmg-2010-and-rpc-server-unavailable/
---
Recently I have encountered strange TMG behavior. I have permitted all RPC connections to Domain Controllers with system rule and disabled RPC Filter plus switched off &#8220;Enable strict RPC compliance&#8221; option. But my WMI requests and any other RPC traffic from TMG servers to internal resources was still blocked. 
<!--more-->
The error was &#8220;RPC Server Unavailable&#8221;. Some programs gave me error code 1722. The problem was in understanding of traffic direction for access rules containing &#8220;Local host&#8221;. To tell you the truth I still do not understand why it works such way. Even after I&#8217;ve read <http://tmgblog.richardhicks.com/2011/12/05/forefront-tmg-2010-protocol-direction-explained/>. So the short answer to fix this problem &#8211; you need to create new rule allowing RPC traffic with SOURCE=Internal and DESTINATION=Local host. Also after creation you need to disable RPC filtering for created rule.
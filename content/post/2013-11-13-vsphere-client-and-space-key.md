---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2013-11-13T13:33:48Z"
guid: http://trukhanov.com/?p=25
id: 25
tags:
- vmware
- vSphere
title: vSphere Client and space key
url: /2013/11/vsphere-client-and-space-key/
---
Recently we have had a very confusing incident while trying to access one of our production virtual server through vSphere client. As linux servers in default installation redraw its console only on some event (some input or output), every time we open virtual machine with linux to see its console we need to press some key on keyboard. The problem is when you just opened console in the thick client user&#8217;s input focused on the toolbar. More precisely on its first button, which is &#8220;Shutdown&#8221;. When you press this button you&#8217;ll get confirmation dialog and input focus is on the &#8220;OK&#8221; button again. So if just opened server console, without clicking mouse anywhere, and quickly press space key two times, it will immediately shutdown you server, which will cause unplanned downtime to your production system. Very disappointing application design for industry&#8217;s leader solution. By the way, vSphere webclient doesn&#8217;t have such problems.
<!--more-->
See you next time
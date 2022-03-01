---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2015-04-27T11:05:19Z"
guid: http://trukhanov.com/?p=85
id: 85
title: Connecting with android Boxer mail app to hotmail account over ActiveSync
url: /2015/04/connecting-with-android-boxer-mail-app-to-hotmail-account-over-activesync/
---
By default if you try to connect to any Hotmail e-mail with Boxer app (current version 2.0.0), which is a default in CyanogenOS 12s, it set up your account with IMAP and SMTP. Such setup prevents you from using your calendar and contacts. To fix this you should set up your connection with ActiveSync, but such option is not available by default. To set up it correctly you need to do following steps.
<!--more-->
1. Set up your Hotmail account on your PC with Outlook. You need to do this to find server which holds your account. Correctly working e-mail agents can connect to m.outlook.com which re-targets it to correct server, but Boxer is not such software.

2. Open Outlook with previously set up account.

3. In notification area right-click on Outlook icon holding Ctrl button and choose Connection status&#8230;

4. Write down your Server name.

5. Set up Boxer for you_account@hotmail1.com or any other incorrect domain.

6. Choose Exchange

7. Press Cancel for auto detection

8. Correct your username and password

9. Fill in Server field with server name from step 4

10. When you press Next it will be set up.

That&#8217;s all
---
author: Dmitry Trukhanov
categories:
- Uncategorized
date: "2016-01-21T15:30:40Z"
guid: http://trukhanov.com/?p=115
id: 115
mythemes-mythemes-use-post-layout:
- "0"
mythemes-post-layout:
- ""
mythemes-post-sidebar:
- ""
mythemes-post-title:
- "1"
tags:
- Android
- RemixOS
title: Install RemixOS on Hyper-V virtual machine
url: /2016/01/install-remixos-on-hyper-v-virtual-machine/
---
Sometimes you just need quickly deploy Android x86 server in your production environment using Microsoft Hyper-V virtualization. Now you can do it with [Remix OS](http://www.jide.com/en/remixos-for-pc).

Installation to Hyper-V virtual machine is a bit tricky, but possible.
<!--more-->
First of all create Generation 1 VM with at least 6 Gb HDD. I&#8217;ve tested VM with 2 Gb of RAM.

After creation of VM do not power on it.

Connect created VHD disk to any windows machine (Disk Management -> Right Click -> Attach VHD)

Initialize it as MBR disk.

Format it in FAT32 file system.

Detach VHD.

Boot VM with Legacy ISO. Do not choose Resident or Guest mode
[![RemixOS-installboot](https://i1.wp.com/trukhanov.com/wp-content/uploads/2016/01/RemixOS-installboot.png?resize=300%2C262 "RemixOS-installboot")](https://i1.wp.com/trukhanov.com/wp-content/uploads/2016/01/RemixOS-installboot.png "RemixOS-installboot full piture")

Press TAB button. Edit your boot string to:

`/kernel initrd=initrd.img root=/dev/ram0 androidboot.hardware=remix_x86_64 androidboot.selinux=permissive quiet INSTALL=1 nomodeset vga=791`

You can replace 791 with any 16bit color depth code. More codes can be found at [GRUB VGA Modes](http://pierre.baudu.in/other/grub.vga.modes.html "GRUB VGA Modes")

Chose sda1 partition

[![RemixOS-installdisk](https://i0.wp.com/trukhanov.com/wp-content/uploads/2016/01/RemixOS-installdisk.png?resize=300%2C247)](https://i0.wp.com/trukhanov.com/wp-content/uploads/2016/01/RemixOS-installdisk.png)

Do not format it.

Chose yes for GRUB installation.

Skip EFI GRUB2 installation.

Choose yes for making system directory read-write.

Choose yes for creating user data image.

Enter size of data.img. Maximum is 2047 because we formatted our dist to FAT32 file system.

Now you can run you Android-x86. It&#8217;ll take a while, do not panic.
[![RemixOS-installfinish](https://i1.wp.com/trukhanov.com/wp-content/uploads/2016/01/RemixOS-installfinish.png?resize=300%2C247)](https://i1.wp.com/trukhanov.com/wp-content/uploads/2016/01/RemixOS-installfinish.png)[![RemixOS-installlang](https://i0.wp.com/trukhanov.com/wp-content/uploads/2016/01/RemixOS-installlang.png?resize=300%2C248)](https://i0.wp.com/trukhanov.com/wp-content/uploads/2016/01/RemixOS-installlang.png)

Configure it as you wish.

[![RemixOS-installcompleted](https://i0.wp.com/trukhanov.com/wp-content/uploads/2016/01/RemixOS-installcompleted.png?resize=300%2C248)](https://i0.wp.com/trukhanov.com/wp-content/uploads/2016/01/RemixOS-installcompleted.png)

The last thing you need to do is eject ISO from VM and edit created GRUB entries.

Shutdown your VM and eject ISO.

Once again attach VHD to Windows machine.

Edit \grub\menu.lst

You need to delete extra digits after kernel path, for example if you used vga=791 previously you need to replace **kernel791** with **kernel**. Also you need to add desired vga mode again. So your menu.lst should look like:

```cfg
default=0
timeout=6
splashimage=/grub/android-x86.xpm.gz
root (hd0,0)

title Remix OS 2016-01-14
kernel /android-2016-01-14/kernel vga=791 quiet root=/dev/ram0 androidboot.hardware=remix_x86_64 androidboot.selinux=permissive nomodeset SRC=/android-2016-01-14
initrd /android-2016-01-14/initrd.img

title Remix OS 2016-01-14 (Debug mode)
kernel /android-2016-01-14/kernel vga=791 root=/dev/ram0 androidboot.hardware=remix_x86_64 androidboot.selinux=permissive nomodeset DEBUG=2 SRC=/android-2016-01-14
initrd /android-2016-01-14/initrd.img

title Remix OS 2016-01-14 (Debug nomodeset)
kernel /android-2016-01-14/kernel vga=791 nomodeset root=/dev/ram0 androidboot.hardware=remix_x86_64 androidboot.selinux=permissive nomodeset DEBUG=2 SRC=/android-2016-01-14
initrd /android-2016-01-14/initrd.img

title Remix OS 2016-01-14 (Debug video=LVDS-1:d)
kernel /android-2016-01-14/kernel vga=791 video=LVDS-1:d root=/dev/ram0 androidboot.hardware=remix_x86_64 androidboot.selinux=permissive nomodeset DEBUG=2 SRC=/android-2016-01-14
initrd /android-2016-01-14/initrd.img
```

Detach VHD and boot your VM.

[![RemixOS-installbooting](https://i0.wp.com/trukhanov.com/wp-content/uploads/2016/01/RemixOS-installbooting.png?resize=300%2C248)](https://i0.wp.com/trukhanov.com/wp-content/uploads/2016/01/RemixOS-installbooting.png)

Happy Remix OS experience!

**Update (17 Oct 2016):** latest versions of RemixOS have Hyper-V support. If you have problems with mouse pointer capture in latest versions you can try following:

  1. Start Termux terminal
  2. Get root access with `su` command
  3. Stop Hyper-V mouse driver with `rmmod hid_hyperv` command
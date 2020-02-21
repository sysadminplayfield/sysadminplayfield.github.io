---
layout: post
title:  "RHCSA Certification - Part1 - Reset root password"
date:   2020-02-19 08:22:40 -0600
categories: RHCSA CentOS7
---
If the root password is lost, you need to interrupt the boot process and gain access to a chrooted environment.

At grub2 screen, press "**e**" to **edit** the configuration.  
At the end of the line starting with "**linux16**", append the following:

```console
rd.break enforcing=0
```

“**rd.break**” interrupts the boot process.  
“**enforcing=0**” sets temporarily SELinux to "Permissive".

Proceed with **Ctrl+x** to load the new configuration.

As follows, **mount** the /sysroot folder with **read** and **write** access, and change root (**chroot**) to set the new root password:

```
switch_root:/# mount -o rw,remount /sysroot
switch_root:/# chroot /sysroot
sh-4.2# passwd
	Enter new root password:.....
	Confirm new root password:.....
sh-4.2# exit
switch_root:/# exit
```

Log in as root using the new password, and reset the SELinux context of /etc/shadow :

```
root@localhost# restorecon -v /etc/shadow
root@localhost# reboot
```

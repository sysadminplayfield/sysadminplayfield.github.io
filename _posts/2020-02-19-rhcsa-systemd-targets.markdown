---
layout: post
title:  "RHCSA Certification - Part3 - systemd targets"
date:   2020-02-19 08:40:40 -0600
categories: RHCSA CentOS7
---
With **SysVinit**, we talked about **runlevels**, but with **systemd** it is all about **targets**.

Below, the relationship between runlevels and targets:

```
Runlevel	Target											Description
0			runlevel0.target, poweroff.target				Power off
1			runlevel1.target, rescue.target					Non-graphical single-user
2,3,4		runlevel[234].target, multi-user.target			Non-graphical multi-user
5 			runlevel5.target, graphical.target				Graphical multi-user
6 			runlevel6.target, reboot.target					Reboot
```

<br  />
**Check** the system’s current **default target**:

```
 # systemctl get-default
```

If it is not already set to multi-user, **set** the new **default target** as follow:

```
 # systemctl set-default multi-user.target
```

But if you wish to **switch immediately to rescue** (formerly "single user", or "init 1"):

```
 # systemctl isolate rescue
 ```
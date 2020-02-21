---
layout: post
title:  "RHCSA Certification - Part4 - SELinux"
date:   2020-02-19 08:45:40 -0600
categories: RHCSA CentOS7
---
**SElinux** configuration file is **/etc/selinux/config**.  
In this file you can set the default configuration such as the mode (**enforcing**, **permissive**, **disabled**) and the protection type (**targeted**, **minimum**, **mls**)

When enabled, SELinux can be in Permissive mode or Enforcing mode.

<br  />
Check current mode for SELinux:

```
 # getenforce
```

If SELinux current mode is "Enforcing", you can **temporarily** set it to "**Permissive**":

```
 # setenforce permissive
or
 # setenforce 0
```

Or else, if it is set to "Permissive" and you want to **temporarily** set it to "**Enforcing**":

```
 # setenforce permissive
or
 # setenforce 0
```

<br  />
If SELinux is disabled, `setenforce` will not work because it requires relabeling thoe whole filesystem. You can force a relabel with the following command:

```
touch /.autorelabel
```

To **permanently** set an SELinux mode (**persists across reboots**), **edit** the configuration file **/etc/selinux/config**.

<br  />
**SEMANAGE**

**semanage** tool is not present in the minimal install of CentOS 7, we need to install it.

If you can't remember what package provides **semanage** tool, and let’s install it:

```
# yum provides */semanage
    [...]
    policycoreutils-python
    setroubleshoot-server
    [...]
```

You can install either **setroubleshoot-server** or **policycoreutils-python**

```
# yum install -y setroubleshoot-server
or
# yum install -y policycoreutils-python
```

<br  />
**CONTEXTS ON FILES/DIRECTORIES**

**List** available contexts for admin_home:

```
# semanage fcontext -l | grep admin_home
```

**Retreive SELinux context** for /root folder using ls with **-Z** option:

```
# ls -ldZ /root
    dr-xr-x---. root root system-u:object_r:admin_home_t:s0 /root
```

Create `/custom_home` folder, and **set** “**admin_home_t**” **context** on it using **semanage**:

```
# mkdir /custom_home
# semanage fcontext -a -t admin_home_t "/custom_home(/.*)?"
```

**Apply/Update** the new **context** (-v option is for verbose) and check:

```
# restorecon -v /custom_home
# ls -ldZ /custom_home
    dr-xr-x---. root root unconfined_u:object_r:admin_home_t:s0 /custom_home
```

<br  />
**CONTEXTS ON PORTS**

**List** available contexts for ssh:

```
semanage port -l | grep ssh
```

**Allow** listening on an **unconventional** **port** for ssh in SELinux:  

```
# semanage port -at ssh_port_t 2222 -p tcp
or, if you need to change the default port:
# semanage port -mt ssh_port_t 2222 -p tcp
or, if you need to delete the context:
# semanage port -dt ssh_port_t 2222 -p tcp
```


**Edit** **/etc/ssh/sshd_config** to add listening on **port 2222**:

```
# vi /etc/ssh/sshd_config
	[...]
	Port 22
	Port 2222
	[...]
	PermitRootLogin yes
	[...]
```

**Restart sshd**:

```
# systemctl restart sshd
```

**Allow** access in **firewalld**:

```
# firewall-cmd --permanent --add-port=2222/tcp
# firewall-cmd --reload
```

<br  />
**BOOLEANS**

**List** all **selinux booleans** related to nfs:

```
# getsebool -a | grep httpd
```

**Enable **temporarily** a **boolean**:

```
# setsebool httpd_can_sendmail on
or
# setsebool httpd_can_sendmail 1
```

**Disable** **temporarily** a **boolean**:

```
# setsebool httpd_can_sendmail off
or
# setsebool httpd_can_sendmail 0
```

**Enable** **permanently** a **boolean**:

```
# setsebool -P httpd_can_sendmail on
or
# setsebool -P httpd_can_sendmail 1
```
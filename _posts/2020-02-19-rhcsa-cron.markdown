---
layout: post
title:  "RHCSA Certification - Part13 - Cron, at"
date:   2020-02-20 10:42:40 -0600
categories: RHCSA CentOS7
---
**CRON**

Cron can be configured using **crontab**.  
An explanation of the configuration is available in **/etc/crontab** file (see below).

```
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```

Prior to create the crontab job, make sure your command/script is working:

```
# find /var -type f -name "*.log" >> /var/log/varfiles
# cat /var/log/varfiles
```

**Create** the **cron job** using **crontab**. Set it to run **every monday at 1h20am**:

```
# crontab -e
    [...]
    20 1 * * 1 find /var -type f -name "*.log" >> /var/log/varfiles
    [...]
```

**List** the **cronjobs** for user **root**:

```
# crontab -l
```

<br  />
**AT**

**at** tool is a very useful tool to schedule an action at a later time without setting up a cron job.  
This tool is not installed by default in CentOS 7 minimal installation.

Install **at**:

```
# yum install -y at
```

Add **alice** to the file **/etc/at.deny** to **forbid** her to use at tool:

```
# echo alice > /etc/at.deny
```

Add **ben** to the file **/etc/at.allow** to **allow** him to use at tool:

```
# echo ben > /etc/at.allow
```

Let’s **check**:

```
# su - alice
$ at
	You do not have permission to use at.
$ exit
# su - ben
$ at
	Garbled time
```
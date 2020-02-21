---
layout: post
title:  "RHCSA Certification - Part10 - vsftpd, yum custom repository"
date:   2020-02-19 09:40:40 -0600
categories: RHCSA CentOS7
---
Install **vsftpd** (very secure file transfer protocol daemon) and **createrepo** (tool to create repositories):

```
# yum install vsftpd createrepo
```

Start vsftpd.service and enable autostart at boot:

```
# systemctl enable vsftpd.service
# systemctl start vsftpd.service
```

Configure firewalld to allow ftp service (default port 21):

```
# firewall-cmd --permanent --add-service=ftp
# firewall-cmd --reload
```

Create the custom repository on the server with createrepo:

```
# mkdir /var/ftp/pub/customrepo
# createrepo /var/ftp/pub/customrepo
```

<br  />
**CLIENT SETUP**

Just create a new file: **/etc/yum.repos.d/customrepo.repo** :

```
# vi /etc/yum.repos.d/customrepo.repo

[customrepo]
name=Custom Repo
baseurl=ftp://<client_IP_or_hostname>/pub/customrepo
enabled=1
gpgcheck=0
```

Update the repository cache:

```
# yum repolist
```

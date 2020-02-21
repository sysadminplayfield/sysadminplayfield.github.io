---
layout: post
title:  "RHCSA Certification - Part6 - Firewalld"
date:   2020-02-19 09:00:40 -0600
categories: RHCSA CentOS7
---
Verify if firewalld is active:

```
# firewall-cmd --state
running
```

Start firewalld:

```
# systemctl start firewalld.service
```

Set firewalld to start automatically at boot:

```
# systemctl enable firewalld.service
```

Set the firewall **default zone** (Default is Public zone. Common zones are: internal, external, home, dmz, trusted, drop, etc. ...):

```
# firewall-cmd --set-default-zone=dmz
```

**Allow temporarily** a service to the default zone:

```
# firewall-cmd --add-service=ssh
```

**Allow permanently** a service to the default zone:

```
# firewall-cmd --permanent --add-service=ssh
# firewall-cmd --reload
```

**Remove temporarily** a service to the default zone:

```
# firewall-cmd --remove-service=ssh
```

**Remove permanently** a service to the default zone:

```
# firewall-cmd --permanent --remove-service=ssh
# firewall-cmd --reload
```

Add a **port** to a specific zone:

```
# firewall-cmd --permanent --zone=dmz --add-port=8080/tcp
# firewall-cmd --permanent
```

Create a **rich rules**:

```
# firewall-cmd --permanent --add-rich-rule 'rule family=ipv4 source address=192.168.2.2 port port=80 protocol=tcp accept'
# firewall-cmd --permanent --add-rich-rule 'rule service name=ssh log prefix="SSH_" level=debug limit value=2/m reject'
#firewall-cmd --reload
```

Add a **port forwarding** to the default zone:

```
# firewall-cmd --permanent --add-masquerade
# firewall-cmd --permanent --add-forward-port=port=21:proto=tcp:toport=80:toaddr=192.168.2.3
# firewall-cmd --reload
```
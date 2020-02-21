---
layout: post
title:  "RHCSA Certification - Part2 - Configure network interface"
date:   2020-02-19 08:30:40 -0600
categories: RHCSA CentOS7
---
We assume the following:  
- OS: CentOS 7.0  
- Static IP address: 192.168.2.100/24  
- Gateway: 192.168.2.200 (you can also choose a public DNS server such as google 8.8.8.8)  

<br />
Although it is still possible to manually edit the network configuration files in `/etc/sysconfig/network-scripts` directory, this is not the preferred way anymore.  
If you still wish to manually modify the files, do not forget to execute `nmcli con reload` to update changes.

<br />
Let's start the configuration:

Display network configurations with **nmcli**:
 
```
# nmcli c s
```

If the command doesn't display any connection and you don't know the name of the interface, use `ip a` to get it.

Create the new connection:

```
# nmcli con add type ethernet ifname eth0 con-name eth0
# nmcli con mod eth0 ipv4.addresses "192.168.2.100/24 192.168.2.1" \
 			ipv4.dns 192.168.2.200 \
 			ipv4.method manual \
 			connection.autoconnect yes
# nmcli con up eth0
```

Test the new connection:

```
# ping -I eth0 192.168.2.200
```
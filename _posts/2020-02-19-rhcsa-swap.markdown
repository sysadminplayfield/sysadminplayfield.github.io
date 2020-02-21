---
layout: post
title:  "RHCSA Certification - Part12 - Swap"
date:   2020-02-20 10:22:40 -0600
categories: RHCSA CentOS7
---
**Create** a partition on **/dev/sda**:

```
# fdisk /dev/sda
    p (print partition table to see a summary of the partitions already present on /dev/sda)
    n (create new partition)
    p (primary)
    enter (default partition number - 6)
    enter (default 1st block)
    +65M (size of the new partition)
    p (print partition table to make sure no mistake has been made)
    w (write changes made to the partition table)
```

Use **partprobe** to update system’s awareness about the new partition:

```
# partprobe -s
```

**Create** Linux SWAP **filesystem** type on the new partition **/dev/sda6**:

```
# mkswap /dev/sda6
```

Write the swap partition **UUID** to **/etc/fstab** to make it **persistent across reboots**:

```
# blkid | grep /dev/sda6 >> /etc/fstab
# vi /etc/fstab
    [...]
    UUID=</dev/sda6 UUID> swap swap defaults 0 0
    [...]
```

**Mount** the new **swap**:

```
# swapon -a
```

**Check** the **swap**:

```
# blkid | grep swap
or
# swapon -s
or
# cat /proc/swaps
or
# free -h
```
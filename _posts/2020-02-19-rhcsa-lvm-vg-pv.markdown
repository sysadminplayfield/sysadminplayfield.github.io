---
layout: post
title:  "RHCSA Certification - Part11 - Logical volumes: PV, VG, LV"
date:   2020-02-20 10:12:40 -0600
categories: RHCSA CentOS7
---
PV = Physical Volume (can be a partition on a physical drive, or a full disk)  
VG = Volume Group (logical group made of one or more PVs)  
PE = Physical Extent (minimal unit size defined for a VG, default is 4MB)  
LE = Logical Extent (same size as physical, it is used by LVs to set its total size defined by how many extents they are meant to use. e.g: 10*LE=40MB)  
LV = Logical Volume (logical space allocated on a VG, basically, a logical partition)

<br  />
**FDISK**

Let’s create a new physical partition on /dev/sdb:

```
# fdisk /dev/sdb
    p (print partition table on /dev/sdb)
    n (create new partition)
    p (primary partition type)
    enter (default partition number - 1)
    enter (default 1st block)
    +330M (size of the new partition)
    t (partition type)
    enter (default partition number - 1)
    8e (Linux LVM)
    p (print partition table again, last check before writing changes to disk)
    w (write changes made to the partition table)
```

<br  />
**PARTPROBE**

Use **partprobe** to force the system to be aware of the new partition table:

```
# partprobe -s
```

<br  />
**PVCREATE**  
(Optional, because the command `vgcreate` automatically creates the PVs)
Create a PV on /dev/sdb1:

```
# pvcreate /dev/sdb1
```

Let’s check it:

```
# pvs
# pvdisplay
```

<br  />
**VGCREATE**

Create a VG called vg_test, with a PE size of 8MB on /dev/sdb1:

```
# vgcreate -s 8M vg_test /dev/sdb1
```

Let’s check it:

```
# vgs
# vgdisplay
```

<br  />
**LVCREATE**

**Create** an LV called `lv_test`, with a total size of **10 LEs** on **vg_test**:

```
# lvcreate -n lv_test -l 10 vg_test
```

Let’s check it:

```
# lvs
# lvdisplay
```

**Format** the LV filesystem to **xfs** and create a mount point:

```
# mkfs.xfs /dev/vg_test/lv_test
# mkdir /mnt/mountpoint
```

In order to automatically mount `lv_test` LVM at boot time, we need to define UUID (or logical map path), mounting point, filesystem type, and options in **/etc/fstab**:

Let’s append the output of **blkid** to **/etc/fstab**:

```
# blkid | grep lv_test >> /etc/fstab
```

And edit **/etc/fstab** to complete the information (in this example, we use the **UUID** to identify the volume):

```
# vi /etc/fstab
    [...]
    UUID=<UUID-of-lv_test>	/mnt/mountpoint		xfs		defaults 0 0
```

Mount the volume:

```
# mount -av
# mount | grep lv_test
```

<br  />
**VGEXTEND**

Add a new PV to an existing VG:

```
# vgextend vg_test /dev/sdc1
```

<br  />
**LVEXTEND**

**WARNING**: AT EVERY LV RESIZING USING LVEXTEND, FIRST, MAKE SURE TO TEST WITH THE “-t” OPTION.  
IF IT FAILS WITH THE "-t" OPTION, IT'S OK, YOU CAN INVESTIGATE.
BUT IF YOU SKIP THE TEST AND LVEXTEND FAILS, YOU LOOSE ALL YOUR DATA.

**Test** **lvxtend** with the "**-t**" option:

```
# lvextend -r -L +35 /dev/vg_test/lv_test -t
```

If there is no error message, you can proceed:

```
# lvextend -r -L +35 /dev/vg_test/lv_test
```

You can also extend with `-l +100%FREE` to extend an LV to the maximum size provided by the VG:

```
# lvextend -r -l +100%FREE /dev/vg_test/lv_test
```
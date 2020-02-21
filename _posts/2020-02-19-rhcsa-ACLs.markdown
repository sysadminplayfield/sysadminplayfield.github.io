---
layout: post
title:  "RHCSA Certification - Part9 - ACLs"
date:   2020-02-19 09:30:40 -0600
categories: RHCSA CentOS7
---
Usage:

`getfacl <path_to_file_or_folder>`  
`setfacl [options] [(default)user:<user_name>:<rwx_permissions>] <path_to_file_or_folder>`

<br  />
First, lets switch to user mary and create a folder /home/mary/testdir2 :

```
# su - mary
$ mkdir /home/mary/testdir2
```

Use **chown** to change ownership:

```
# chown mary:mary /home/mary/testdir2
```

Use **getfacl** to retrieve the ACLs related to /home/mary/testdir2 :

```
# getfacl /home/mary/testdir2
```

Now, use **setfacl** with **-m** option to modify **standard** and **default** ACLs of /home/mary in order to allow barry to access testdir2 subfolder with **read-only** permissions:

```
# setfacl -m u:barry:rx /home/mary
# setfacl -m du:barry:rx /home/mary
```

Use **setfacl** with option **-m** to modify **standard** and **default** ACLs of /home/mary/testdir2 in order to provide barry with **read** and **write** access:

```
# setfacl -m u:barry:rwx /home/mary/testdir2
# setfacl -m du:barry:rwx /home/mary/testdir2
```

Check newly applied ACLs on folders /home/mary and /home/mary/testdir2 :

```
# getfact /home/mary
# getfact /home/mary/testdir2
```

Finaly, a real-life test to make sure nothing prohibits barry to access testdir2 and to create/edit files:

```
# su - barry
$ touch /home/mary/testdir2/testfile
$ echo "this is a write access test" > /home/mary/testdir2/testfile
$ cat /home/mary/testdir2/testfile
this is a write access test
```
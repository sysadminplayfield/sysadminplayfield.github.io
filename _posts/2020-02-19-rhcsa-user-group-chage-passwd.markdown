---
layout: post
title:  "RHCSA Certification - Part8 - User/group management and basic permissions"
date:   2020-02-19 09:20:40 -0600
categories: RHCSA CentOS7
---
It is easy to create a user with useradd, but if you have to create multiple user accounts, it is much more convenient to do it with a bash script that will loop the command for you.

<br  />
**USERADD**

Usage: `useradd [options] <user_name>`  
This command will create a new user account, and, if no other option is specified, will also create a home folder in `/home/<user_name>`  
To start with, the most important options are:
 -D : defaults (can be altered with additional options)  
 -m : create home directory if it doesn't exist   
 -s : shell (/bin/bash, /sbin/nologin, etc. ...)    
 -e : expire date with format YYYY-MM-DD  
 -f : inactive after creation  
 -g : manual gid number  
 -u : manual uid number 
 -G : groups (coma separated)  
 -r : system account, doesn't create home, mail, usergroup...
And more ... rtfm

<br  />
**CHAGE**

Usage: `chage [options] <user_name>`

The **-l** option will **display** password aging **information**:

```
# chage -l mary
# chage -l barry
```

The **-E** option will set a **password expiration** date for the user:

```
# chage -E 2017-12-31 mary
# chage -E 2017-12-31 barry
```
 
<br  />
**GROUPADD**

Usage: `groupadd [options] <group_name>`

Just **create** the **group** dba:

```
# groupadd dba
```
 
<br  />
**USERMOD**

Usage: `usermod [options] <user_name>`
The **-aG** option means **append Group**. It will basically add a supplementary group to the user.

```
# usermod -aG dba larry
# usermod -aG dba gary
```

<br  />
**CHOWN**

Use **chown** to **change user and/or group** ownership:

```
# chown larry /srv/grouptest
or
# chown larry:dba /srv/grouptest
or
# chown larry: /srv/grouptest
or
# chown :dba /srv/grouptest
```

<br  />
**CHGRP**

Use **chgrp** to **change** the **group** on a file or folder:

```
# chgrp /srv/grouptest dba
```

<br  />
**CHMOD**

Use **chmod** to change permissions to **rwx** (read, write, execute) on **/srv/grouptest** for owner and group members only:

```
# chmod -R 770 /srv/grouptest
or
# chmod -R ug=rwx,o-rwx /srv/grouptest
```

Group collaboration:  
SetGID: All files created belong to the group. All the members of the group can create files, edit or remove any files from the group.

```
# chmod 2770 /srv/grouptest
or
# chmod g+s /srv/grouptest
```

SetUID is not recommended. Users allowed to execute a program use the identity (and permissions) of the owner.

```
# chmod 4770 /srv/grouptest/script.sh
or
# chmod u+s /srv/grouptest/script.sh
```

StickyBit:

```
# chmod 1770 /srv/grouptest/myfile
or
# chmod +t /srv/grouptest/myfile
```

StickyBit with SetGID:  
StickyBit sets additional permissions on a file. It comes very convenient in case of group collaboration because only the owner can delete its files:

```
#chmod 3770 /srv/grouptest/myfile
```

<br  />
**CHATTR**

Change attributes on a file, set the file to immutable (even by root):

```
# chattr +i file
```

<br  />
**GPASSWD**

Usage: `gpasswd [option] <user_name> <group_name>`

Remove user larry from the dba group:

```
# gpasswd -d larry dba
```

<br  />
**USERDEL**

Delete a user:

```
# userdel larry
```

Delete a user with its home directory and mailbox:

```
# userdel -r larry
```

<br  />
**GROUPDEL**

Delete a group:

```
# groupdel dba
```

<br  />
**EXAMPLE**

Create several users in 1 command:

```
# for i in barry harry larry mary gary; do useradd $i; done
```

Check if all the user accounts have properly been created:

```
# tail -5 /etc/passwd
or
# for i in barry harry larry mary gary; do id $i; done
```

Set password for all newly created users by redirecting “password” in the standard input:

```
# for i in barry harry larry mary gary; do echo "password" | passwd $i --stdin; done
```

Just to mention that this is not the safest way to do it !
There are several discussions related to this topic, for example in https://stackoverflow.com/questions/714915/using-the-passwd-command-from-within-a-shell-script  
Also, it may not be ideal to keep this command in the bash history. If you want to run the command without keeping it in history, just add a space at the beginning of the line (works for bash shell, not for zsh).
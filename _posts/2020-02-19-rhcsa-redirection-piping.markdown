---
layout: post
title:  "RHCSA Certification - Part5 - Redirections, STD{IN,OUT,ERR}, |, ||, &&"
date:   2020-02-19 08:50:40 -0600
categories: RHCSA CentOS7
---
stdin: < : code 0 : Standard input  
stdout: > : code 1 : Standard output  
stderr: 2> : code 2 : Standard error  
`|` : redirects the output of a command to another command   
`>>` : append to a file  
`||` : logical OR (if the 1st command fails, do the 2nd anyway)  
`&&` : logical AND (if the 1st command fails, stop)  

Redirect stdout to a file (stderr will be displayed on screen and ):

```
# find /var/log -perm 0755 >/root/perms.txt
```

Redirect stderr to a file (stdout will be displayed on screen):

```
# find /var/log -perm 0755 2> /root/perms.txt
```

Redirect stderr to the target of stdout (here, a file called /root/perms.txt):

```
# find /var/log -perm 0755 > /root/perms.txt 2>&1
```

Redirect the **echo** command to a file:

```
# echo "test" > /root/testfile
```

Append text to a file:

```
# echo " and 2nd test" >> /root/testfile
```

Redirect to stdin (here, the screen):

```
# < testfile
```

Complex command to send a string and attach a file to an mail, and confirm if the command successfully exited:

```
# echo "Hi" | mailx -s "Re: requested file" root < testfile | if [ $? -eq 0 ]; then; echo "Mail has been sent"; else; echo "Error"; fi
```

<br  />
Usage for `**||**` and `**&&**`:

Example for logical OR:

```
rm -Rf somedir || exit_on_error "Failed to remove the directory"
```

Example for logical AND (excludes 2nd part if the first fails):

```
rm -Rf somedir && trace_output "Removed the directory"
```

Example for both logical AND and OR:
```
mailx -s "Re: requested file" root < testfile && echo "Mail has been sent" || echo "Error"
```

```
rm -Rf somedir && trace_output "Removed the directory" || exit_on_error "Failed to remove the directory"
```
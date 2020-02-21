---
layout: post
title:  "RHCSA Certification - Part7 - The $PS1 variable"
date:   2020-02-19 09:10:40 -0600
categories: RHCSA CentOS7
---
The $PS1 variable defines what your primary prompt looks like (in general, the primary prompt is bash – Bourne Again SHell)
You can display your system variable using the echo command, as follows:

```
# echo $PS1
```

<br  />
To find information about the **$PS1** variable when you do not have access to internet, you can check the **bash manual**.  
To do so, just enter `man bash` in your terminal, then type `/PROMPTING` then hit enter, then hit the **n** key twice, and it will jump to the options available for the $PS1 variable.

In our case, we only need 4 options:  
\H = display hostname  
\u = display username  
\w = display working directory  
\$ = display “$” for normal user or “#” for root user

<br  />
Now we can change our **$PS1** variable:

```
# export PS1="\H \u \w \$"
# echo $PS1
 PS1="\H \u \w \$"
```

To make the variable **persistent across reboots**, we need to append it to the `.bashrc` file in the user directory (either **/root** or **/home/$user**):

```
# echo 'export PS1="\H \u \w \$"' >> ~/.bashrc
```

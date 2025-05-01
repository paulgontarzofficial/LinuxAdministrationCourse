# Unit 5 Overview

## Required Materials

PuTTY 
Rocky Server
Root or sudo command access 

## Exercises

1.       mkdir lab_users

2.       cd /lab_users

3.       cat /etc/passwd
	```[root@rocky7 lab_users]# cat /etc/passwd
# Uncomment the following line to enable passwordless root login
# root::0:0:root:/root:/bin/bash
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
tss:x:59:59:Account used for TPM access:/:/usr/sbin/nologin
systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
polkitd:x:998:996:User for polkitd:/:/sbin/nologin
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:65534:65534:Kernel Overflow User:/:/sbin/nologin
systemd-coredump:x:999:997:systemd Core Dumper:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:998:995:User for polkitd:/:/sbin/nologin
saslauth:x:997:76:Saslauthd user:/run/saslauthd:/sbin/nologin
sssd:x:996:994:User for sssd:/:/sbin/nologin
libstoragemgmt:x:992:992:daemon account for libstoragemgmt:/:/usr/sbin/nologin
unbound:x:991:991:Unbound DNS resolver:/etc/unbound:/sbin/nologin
tss:x:59:59:Account used for TPM access:/:/usr/sbin/nologin
pcp:x:990:990:Performance Co-Pilot:/var/lib/pcp:/usr/sbin/nologin
cockpit-ws:x:989:989:User for cockpit web service:/nonexisting:/sbin/nologin
cockpit-wsinstance:x:988:988:User for cockpit-ws instances:/nonexisting:/sbin/nologin
rpc:x:32:32:Rpcbind Daemon:/var/lib/rpcbind:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
clevis:x:987:987:Clevis Decryption Framework unprivileged user:/var/cache/clevis:/usr/sbin/nologin
setroubleshoot:x:986:986:SELinux troubleshoot server:/var/lib/setroubleshoot:/usr/sbin/nologin
grafana:x:985:985:Grafana user account:/usr/share/grafana:/usr/sbin/nologin
pcpqa:x:984:984:PCP Quality Assurance:/var/lib/pcp/testsuite:/bin/bash
rpcuser:x:29:29:RPC Service User:/var/lib/nfs:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/usr/share/empty.sshd:/usr/sbin/nologin
chrony:x:983:983:chrony system user:/var/lib/chrony:/sbin/nologin
tcpdump:x:72:72::/:/sbin/nologin
scott:x:1000:1000:scott:/home/scott:/bin/bash
dhcpd:x:177:177:DHCP server:/:/sbin/nologin
geoclue:x:982:981:User for geoclue:/var/lib/geoclue:/sbin/nologin
rtkit:x:172:172:RealtimeKit:/proc:/sbin/nologin
pipewire:x:981:980:PipeWire System Daemon:/run/pipewire:/usr/sbin/nologin
flatpak:x:980:979:User for flatpak system helper:/:/sbin/nologin
pesign:x:979:978:Group for the pesign signing daemon:/run/pesign:/sbin/nologin
labuser:x:1001:1001::/home/labuser:/bin/bash
scott2:x:1002:1002::/home/scott2:/bin/bash
fisherman:x:1003:1003::/home/fisherman:/bin/bash
wazuh:x:978:977::/var/ossec:/sbin/nologin
mysql:x:1004:1004::/home/mysql:/bin/bash
user1:x:1005:1005::/home/user1:/bin/bash
user2:x:1006:1006::/home/user2:/bin/bash
user3:x:1007:1007::/home/user3:/bin/bash
user17:x:1008:1008::/home/user17:/bin/bash
user4:x:1009:1009::/home/user4:/bin/bash
[root@rocky7 lab_users]#

```
We’ll be examining the contents of this file later

4.       cat /etc/passwd | tail -5
```
[root@rocky7 lab_users]# cat /etc/passwd | tail -5
user1:x:1005:1005::/home/user1:/bin/bash
user2:x:1006:1006::/home/user2:/bin/bash
user3:x:1007:1007::/home/user3:/bin/bash
user17:x:1008:1008::/home/user17:/bin/bash
user4:x:1009:1009::/home/user4:/bin/bash
[root@rocky7 lab_users]#
```

What did this do to the output of the file?

5.        cat /etc/passwd | tail -5 | nl
```
[root@rocky7 lab_users]# cat /etc/passwd | tail -5 | nl
     1  user1:x:1005:1005::/home/user1:/bin/bash
     2  user2:x:1006:1006::/home/user2:/bin/bash
     3  user3:x:1007:1007::/home/user3:/bin/bash
     4  user17:x:1008:1008::/home/user17:/bin/bash
     5  user4:x:1009:1009::/home/user4:/bin/bash
[root@rocky7 lab_users]#

```

6.       cat /etc/passwd | tail -5 | awk -F : ‘{print $1, $3, $7}’
```
[root@rocky18 ~]# cat /etc/passwd | tail -5 | awk -F : '{print $1, $3, $7}'
labuser 1001 /bin/bash
scott2 1002 /bin/bash
fisherman 1003 /bin/bash
wazuh 978 /sbin/nologin
mysql 1004 /bin/bash
[root@rocky18 ~]#

```
What did that do and what do each of the $# represent?
	`- The above command displayed the last five lines of /etc/passwd, and utilizing awk command, awk splits the output into columns and then displays the specified columns that were specified in that awk command. ` 
Can you give the 2nd, 5th, and 6th fields?
```
[root@rocky18 ~]# cat /etc/passwd | tail -5 | awk -F : '{print $2, $5, $6}'
x  /home/labuser
x  /home/scott2
x  /home/fisherman
x  /var/ossec
x  /home/mysql
[root@rocky18 ~]#

```

7.       cat /etc/passwd | tail -5 | awk –F : ‘{print $NF}’
```
[root@rocky18 ~]# cat /etc/passwd | tail -5 | awk -F : '{print $NF}'
/bin/bash
/bin/bash
/bin/bash
/sbin/nologin
/bin/bash
[root@rocky18 ~]#

```

What does this $NF mean? Why might this be useful to us as administrators?
	`- $NF in awk is a built-in variable where it displays the last field. This can be useful for us in /etc/passwd instance to view the last field which happens to be the shell that the user interacts with. 
8.        alias
	```
[root@rocky18 ~]# alias
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias xzegrep='xzegrep --color=auto'
alias xzfgrep='xzfgrep --color=auto'
alias xzgrep='xzgrep --color=auto'
alias zegrep='zegrep --color=auto'
alias zfgrep='zfgrep --color=auto'
alias zgrep='zgrep --color=auto'
[root@rocky18 ~]#

```

look at the things you have aliased. These come from defaults in your .bashrc file. We’ll configure these later.

```
[root@rocky18 ~]# cat .bashrc
# .bashrc

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi

# User specific environment
if ! [[ "$PATH" =~ "$HOME/.local/bin:$HOME/bin:" ]]
then
    PATH="$HOME/.local/bin:$HOME/bin:$PATH"
fi
export PATH

# Uncomment the following line if you don't like systemctl's auto-paging feature:
# export SYSTEMD_PAGER=

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
[root@rocky18 ~]#

```

9.       cd /root
```
[root@rocky18 ~]# cd /root/
[root@rocky18 ~]#

```

10.   ls -l
```
[root@rocky18 ~]# ls -l
total 20
-rw-------. 1 root root 2506 Nov 19  2023 anaconda-ks.cfg
-rw-r--r--. 1 root root  133 Nov 19  2023 anaconda-post.log
-rw-r--r--. 1 root root   23 Aug 15  2024 lab-file
-rw-------. 1 root root 2067 Nov 19  2023 original-ks.cfg
-rw-r--r--. 1 root root   26 Apr 30 20:29 test
[root@rocky18 ~]#

```

11.   ll
```
[root@rocky18 ~]# ll
total 20
-rw-------. 1 root root 2506 Nov 19  2023 anaconda-ks.cfg
-rw-r--r--. 1 root root  133 Nov 19  2023 anaconda-post.log
-rw-r--r--. 1 root root   23 Aug 15  2024 lab-file
-rw-------. 1 root root 2067 Nov 19  2023 original-ks.cfg
-rw-r--r--. 1 root root   26 Apr 30 20:29 test
[root@rocky18 ~]#

```

Output should be similar

12.   unalias ll
```
[root@rocky18 ~]# unalias ll
[root@rocky18 ~]#
```

13.   ll
```
[root@rocky18 ~]# ll
-bash: ll: command not found
[root@rocky18 ~]#
```

you shouldn’t have this command available anymore

14.   ls
```
[root@rocky18 ~]# ls
anaconda-ks.cfg  anaconda-post.log  lab-file  original-ks.cfg  test
[root@rocky18 ~]#
```

15.   unalias ls
```
[root@rocky18 ~]# unalias ls
[root@rocky18 ~]#
```

How did ls change on your screen?
	`When we unalias the ls command, this is taking away the color when showing directories and files.`

No worries, there are two ways to fix the mess you've made. Nothing you've done is permanent, so logging out and reloading a shell (logging back in) would fix this.  
We just put the aliases back.

16. `alias ll='ls -l --color=auto'`
17. `alias ls='ls --color=auto'`
    - Test with `alias` to see them added and also use `ll` and `ls` to see them work properly.
```
[root@rocky18 ~]# alias
alias cp='cp -i'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls -l --color=auto'
alias mv='mv -i'
alias rm='rm -i'
alias xzegrep='xzegrep --color=auto'
alias xzfgrep='xzfgrep --color=auto'
alias xzgrep='xzgrep --color=auto'
alias zegrep='zegrep --color=auto'
alias zfgrep='zfgrep --color=auto'
alias zgrep='zgrep --color=auto'
[root@rocky18 ~]# ll
total 20
-rw-------. 1 root root 2506 Nov 19  2023 anaconda-ks.cfg
-rw-r--r--. 1 root root  133 Nov 19  2023 anaconda-post.log
-rw-r--r--. 1 root root   23 Aug 15  2024 lab-file
-rw-------. 1 root root 2067 Nov 19  2023 original-ks.cfg
-rw-r--r--. 1 root root   26 Apr 30 20:29 test
[root@rocky18 ~]# ls
total 20
-rw-------. 1 root root 2506 Nov 19  2023 anaconda-ks.cfg
-rw-r--r--. 1 root root  133 Nov 19  2023 anaconda-post.log
-rw-r--r--. 1 root root   23 Aug 15  2024 lab-file
-rw-------. 1 root root 2067 Nov 19  2023 original-ks.cfg
-rw-r--r--. 1 root root   26 Apr 30 20:29 test
[root@rocky18 ~]#

```


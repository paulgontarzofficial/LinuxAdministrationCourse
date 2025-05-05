[![](https://github.com/ProfessionalLinuxUsersGroup/img/raw/main/Assets/Logos/ProLUG_Round_Transparent_LOGO.png?raw=true)](https://github.com/ProfessionalLinuxUsersGroup/img/blob/main/Assets/Logos/ProLUG_Round_Transparent_LOGO.png?raw=true)

# Unit 5 Lab - Managing Users and Groups

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u5lab.md#unit-5-lab---managing-users-and-groups)

> If you are unable to finish the lab in the ProLUG lab environment we ask you `reboot` the machine from the command line so that other students will have the intended environment.

### Resources / Important Links

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u5lab.md#resources--important-links)

- [Killercoda Labs](https://killercoda.com/learn)
- [RedHat: User and Group Management](https://www.redhat.com/en/blog/linux-user-group-management)
- [Rocky Linux User Admin Guide](https://docs.rockylinux.org/books/admin_guide/06-users/)
- [Killercoda lab by FishermanGuyBro](https://killercoda.com/fishermanguybro)

### Required Materials

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u5lab.md#required-materials)

- Rocky 9.4+ - ProLUG Lab
    - Or comparable Linux box
- root or sudo command access

#### Downloads

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u5lab.md#downloads)

The lab has been provided for convenience below:

- [📥 u5_lab(`.pdf`)](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/assets/downloads/u5/u5_lab.pdf)
- [📥 u5_lab(`.docx`)](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/assets/downloads/u5/u5_lab.docx)

## Pre-Lab Warm-Up

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u5lab.md#pre-lab-warm-up)

---

Exercises (Warmup to quickly run through your system and practice commands)

1. `mkdir lab_users`
2. `cd /lab_users`
3. `cat /etc/passwd`
    - We'll be examining the contents of this file later
4. `cat /etc/passwd | tail -5`
    - What did this do to the output of the file?
5. `cat /etc/passwd | tail -5 | nl`
6. `cat /etc/passwd | tail -5 | awk -F : ‘{print $1, $3, $7}'`
    - What did that do and what do each of the `$#` represent?
    - Can you give the 2nd, 5th, and 6th fields?
7. `cat /etc/passwd | tail -5 | awk -F : ‘{print $NF}'`
    - What does this `$NF` mean? Why might this be useful to us as administrators?
8. `alias`
    - Look at the things you have aliased.
    - These come from defaults in your `.bashrc` file. We'll configure these later
9. `cd /root`
10. `ls -l`
11. `ll`
    - Output should be similar.
12. `unalias ll`
13. `ll`
    - You shouldn't have this command available anymore.
14. `ls`
15. `unalias ls`
    - How did `ls` change on your screen?

No worries, there are two ways to fix the mess you've made. Nothing you've done is permanent, so logging out and reloading a shell (logging back in) would fix this.  
We just put the aliases back.

16. `alias ll='ls -l --color=auto'`
17. `alias ls='ls --color=auto'`
    - Test with `alias` to see them added and also use `ll` and `ls` to see them work properly.

## Lab 🧪

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u5lab.md#lab-)

---

This lab is designed to help you get familiar with the basics of the systems you will be working on.

Some of you will find that you know the basic material but the techniques here allow you to put it together in a more complex fashion.

It is recommended that you type these commands and do not copy and paste them. Browsers sometimes like to format characters in a way that doesn't always play nice with Linux.

#### The Shadow password suite:

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u5lab.md#the-shadow-password-suite)

There are 4 files that comprise of the shadow password suite. We'll investigate them a bit and look at how they secure the system. The four files are `/etc/passwd`, `/etc/group`, `/etc/shadow`, and `/etc/gshadow`.

1. Look at each of the files and see if you can determine some basic information about them
    
    ```shell
    more /etc/passwd
    more /etc/group
    more /etc/shadow
    more /etc/gshadow
    ```
    
    There is one other file you may want to become familiar with:
    
    ```shell
    more /etc/login.defs
    ```
    
    Check the file permissions:
    
    ```shell
    ls -l /etc/passwd

	Output:
	[root@rocky4 ~]# ls -l /etc/passwd
	-rw-r--r--. 1 root root 3610 May  4 17:37 /etc/passwd
	[root@rocky4 ~]#
	
	[root@rocky4 ~]# ls -l /etc/shadow
	----------. 1 root root 614 Jul  7  2024 /etc/shadow
	[root@rocky4 ~]#
	
	[root@rocky4 ~]# ls -l /etc/gshadow
	----------. 1 root root 361 Jul  5  2024 /etc/gshadow
	[root@rocky4 ~]#
	
	[root@rocky4 ~]# ls -l /etc/group
	-rw-r--r--. 1 root root 1464 May  4 17:47 /etc/group
	[root@rocky4 ~]#

    ```
    
    Do this for each file to see how their permissions are set.
    
    You may note that `/etc/passwd` and `/etc/group` are readable by everyone on the system but `/etc/shadow` and `/etc/gshadow` are not readable by anyone on the system.
    
2. Anatomy of the `/etc/passwd` file `/etc/passwd` is broken down like this, a `:` (colon) delimited file:
    
    |Username|Password|User ID (UID)|Group ID (GID)|User Info|Home Directory|Login Shell|
    |---|---|---|---|---|---|---|
    |`puppet`|`x`|`994`|`991`|`Puppet server daemon`|`/opt/puppetlabs/server/data/puppetserver`|`/sbin/nologin`|
    

	`cat` or `more` the file to verify these are values you see.

	Are there always 7 fields?
	- Yes, there are always going to be 7 fields, although some may not have a numerical value, they instead have an 'x' in place of a numerical value. That 'x' depicted in the second field is a representation of the encrypted password being stored in the /etc/shadow file. 

3. Anatomy of the `/etc/group` file `/etc/group` is broken down like this, a `:` (colon) delimited file:
    
    |Groupname|Password|Group ID|Group Members|
    |---|---|---|---|
    |`puppet`|`x`|`991`|`foreman, foreman-proxy`|
    
    - `cat` or `more` the file to verify these are the values you see. Are there always 4 fields?
	    - For this we see a majority of the groups not having any members listed, thus meaning that we only see 3 fields, however we do notice that there is a colon after the field. If there were any members in the group, we would then see those users listed. 
			kmem:x:9:
			wheel:x:10:scott
			cdrom:x:11:
			mail:x:12:postfix
			man:x:15:
			dialout:x:18:
			floppy:x:19: ```
		
4. We're not going to break down the `g` files, but there are a lot of resources online that can show you this same information. Suffice it to say, the passwords, if they exist, are stored in an md5 digest format up to RHEL 5. RHEL 6,7,8 and 9 use SHA-512 hash. We cannot allow these to be read by just anyone because then they could brute force and try to figure out our passwords.

#### Creating and modifying local users:

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u5lab.md#creating-and-modifying-local-users)

We should take a second to note that the systems you're using are tied into our active directory with Kerberos. You will not be seeing your account in `/etc/passwd`, as that authentication is occurring remotely. You can, however, run `id <username>` to see user information about yourself that you have according to active directory. Your `/etc/login.defs` file is default and contains a lot of the values that control how our next commands work

5. Creating users
    
    ```shell
    useradd user1
    useradd user2
    useradd user3
    ```
    
    Do a quick check on our main files:
    
    ```shell
    tail -5 /etc/passwd
    tail -5 /etc/shadow

[root@rocky4 ~]# tail -5 /etc/passwd
wazuh:x:978:977::/var/ossec:/sbin/nologin
mysql:x:1004:1004::/home/mysql:/bin/bash
user1:x:1005:1005::/home/user1:/bin/bash
user2:x:1006:1006::/home/user2:/bin/bash
user3:x:1007:1007::/home/user3:/bin/bash
[root@rocky4 ~]#


[root@rocky4 ~]# tail -5 /etc/shadow
sshd:!!:19882::::::
polkitd:!!:19910::::::
user1:!!:20213:0:99999:7:::
user2:!!:20213:0:99999:7:::
user3:!!:20213:0:99999:7:::
[root@rocky4 ~]#


[root@rocky4 /]# ls /home
deploy           finance_user1                gbrs              lab_baseline    minikube         scott2      testuser1   user12  user22  zorgul
ecommerce_user1  finance_user2                healthcare_user1  lab_essentials  minikube_user    superman    testuser15  user13  user3
ecommerce_user2  finance_user3                healthcare_user2  labuser         mysql            tech_user1  tony        user14  user33
ecommerce_user3  fisherman                    healthcare_user3  ldap            project_account  tech_user2  user1       user17  user4
engineer         fisherman.2025-01-21.tar.gz  john              linuxengineer   scott            tech_user3  user11      user2   user55
[root@rocky4 /]#

    ```
    
    What `UID` and `GID` were each of these given? Do they match up? Verify your users all have home directories. Where would you check this?
	    user1 UID and GID are 1005
		user2  UID and GID are 1006
		user3 UID and GID are 1007
			- We can check the UID and GID through the /etc/passwd file in the third and fourth field. 
			- Yes, the UID and GID both match each user account. 
			- We can verify whether these users have home directories by typing ls /home into the shell and viewing the output and verifying the directory creations. 
    
    ```shell
    ls /home
    ```
    
    Your users `/home/<username>` directories have hidden files that were all pulled from a directory called `/etc/skel`. If you wanted to test this and verify you might do something like this:
    
    ```shell
    cd /etc/skel
    vi .bashrc
    ```
    
    Use `vi` commands to add the line:
    
    ```shell
    alias dinosaur='echo "Rarw"'
    ```
    
    Your file should now look like this:
    
    ```shell
    # .bashrc
    # Source global definitions
    if [ -f /etc/bashrc ]; then
    . /etc/bashrc
    fi
    alias dinosaur='echo "Rarw"'
    # Uncomment the following line if you don't like systemctl's auto-paging feature:
    # export SYSTEMD_PAGER=
    # User specific aliases and functions
    ```
    
    Save the file with `:wq`.
    
    ```shell
    useradd user4
    su - user4
    dinosaur # Should roar out to the screen
    ```
    
    Doing that changed the `.bashrc` file for all new users that have home directories created on the server. An old trick, when users mess up their login files (all the `.` files), is to move them all to a directory and pull them from `/etc/skel` again. If the user can log in with no problems, you know the problem was something they created.
    
    We can test this with the same steps on an existing user. Pick an existing user and verify they don't have that command
    
    ```shell
    su - user1
    dinosaur # Command not found
    exit
    ```
    
    Then, as root:
    
    ```shell
    cd /home/user1
    mkdir old_dot_files
    mv .* old_dot_files          # Ignore the errors, those are directories
    cp /etc/skel/.* /home/user1  # Ignore the errors, those are directories
    su - user1
    dinosaur # Should 'roar' now because the .bashrc file is new from /etc/skel
    ```
    
6. Creating groups From our `/etc/login.defs` we can see that the default range for UIDs on this system, when created by `useradd` are:
    
    ```shell
    UID_MIN 1000
    UID_MAX 60000
    ```
    
    So an easy way to make sure that we don't get confused on our group numbering is to ensure we create groups outside of that range. This isn't required, but can save you headache in the future.
    
    ```shell
    groupadd -g 60001 project
    tail -5 /etc/group

Output: 
[root@rocky4 user1]# groupadd -g 60001 project
[root@rocky4 user1]# tail -5 /etc/group
fisherman:x:1003:
wazuh:x:977:wazuh
mysql:x:1004:
user1:x:1005:
project:x:60001:
[root@rocky4 user1]#

    ```
    
    You can also make groups the old fashioned way by putting a line right into the `/etc/group` file.  
    Try this:
    
    ```shell
    vi /etc/group
    ```
    
    - Shift+G to go to the bottom of the file.
    - Hit `o` to create a new line and go to insert mode.
    - Add `project2:x:60002:user4`
    - Hit `Esc`
    - `:wq!` to write quit the file explicit force because it's a read only file.
    
    ```shell
    id user 4 # Should now see the project2 in the user's groups
    ```
    
7. Modifying or deleting users So maybe now we need to move our users into that group.
    
    ```shell
    usermod -G project user4
    tail -f /etc/group # Should see user4 in the group

Output:
[root@rocky4 user1]# id user4
uid=1006(user4) gid=1006(user4) groups=1006(user4),60002(project2)
[root@rocky4 user1]#

[root@rocky4 user1]# tail -f /etc/group
pesign:x:978:
labuser:x:1001:
scott2:x:1002:
fisherman:x:1003:
wazuh:x:977:wazuh
mysql:x:1004:
user1:x:1005:
project:x:60001:
project2:x:60002:user4
user4:x:1006:
^C
[root@rocky4 user1]#

    ```
    
    But, maybe we want to add more users and we want to just put them in there:
    
    ```shell
    vi /etc/group
    ```
    
    - `Shift+G` Will take you to the bottom.
    - Hit `i` (will put you into insert mode).
    - Add `,user1,user2` after `user4`.
    - Hit `Esc`.
    - `:wq` to save and exit.  
        Verify your users are in the group now
    
    ```shell
    id user4
    id user1
    id user2

Output:
[root@rocky4 user1]# id user1
uid=1005(user1) gid=1005(user1) groups=1005(user1),60002(project2)
[root@rocky4 user1]# id user2
uid=1007(user2) gid=1007(user2) groups=1007(user2),60002(project2)
[root@rocky4 user1]# id user4
uid=1006(user4) gid=1006(user4) groups=1006(user4),60002(project2)
[root@rocky4 user1]#

    ```
    
8. Test group permissions I included the permissions discussion from an earlier lab because it's important to see how permissions affect what user can see what information.
    
    Currently we have `user1,2,4` belonging to group `project` but not `user3`. So we will verify these permissions are enforced by the filesystem.
    
    ```shell
    mkdir /project
    ls -ld /project
    chown root:project /project
    chmod 775 /project
    ls -ld /project
    ```
    
    If you do this, you now have a directory `/project` and you've changed the group ownership to `/project`. You've also given group `project` users the ability to write into your directory. Everyone can still read from your directory. Check permissions with users:
    
    ```shell
    su - user1
    cd /project
    touch user1
    exit
    su - user3
    cd /project
    touch user3
    exit
    ```
    
    Anyone not in the `project` group doesn't have permissions to write a file into that directory. Now, as the root user:
    
    ```shell
    chmod 770 /project
    ```
    
    Check permissions with users:
    
    ```shell
    su - user1
    cd /project
    touch user1.1
    exit
    su - user3
    cd /project # Should break right about here
    touch user3
    exit
    ```
    
    You can play with these permissions a bit, but there's a lot of information online to help you understand permissions better if you need more resources.
    

#### Working with permissions:

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u5lab.md#working-with-permissions)

Permissions have to do with who can or cannot access (read), edit (write), or execute (xecute)files. Permissions look like this:

```shell
ls -l
```

|Permission|# of Links|UID Owner|Group Owner|Size (b)|Creation Month|Creation Day|Creation Time|Name of File|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|`-rw-r--r--.`|`1`|`scott`|`domain_users`|`58`|`Jun`|`22`|`08:52`|`datefile`|

The primary permissions commands we're going to use are going to be `chmod` (access) and `chown` (ownership).

A quick rundown of how permissions break out:

[![](https://github.com/ProfessionalLinuxUsersGroup/lac/raw/main/src/assets/images/permissions.png)](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/assets/images/permissions.png)

Let's examine some permissions and see if we can't figure out what permissions are allowed:

```shell
ls -ld /home/scott/
drwx------. 5 scott domain_users 4096 Jun 22 09:11 /home/scott/
```

The first character lets you know if the file is a directory, file, or link. In this case we are looking at my home directory.

`rwx`: For UID (me).

- What permissions do I have?
	- You have Read, Write, Execute permissions

`---`: For group.

- Who are they?
	- domain_users
- What can my group do?
	- Your group can do absolutely nothing

`---`: For everyone else.

- What can everyone else do?
	- Absolutely nothing 

Go find some other interesting files or directories and see what you see there.  
Can you identify their characteristics and permissions?


```shell 
[root@rocky14 ~]# ls -ld /etc/shadow
----------. 1 root root 614 Jul  7  2024 /etc/shadow
[root@rocky14 ~]#
```


> Be sure to `reboot` the lab machine from the command line when you are done.
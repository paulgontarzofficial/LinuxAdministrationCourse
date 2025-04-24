
**ProLUG – Unit 4 Operate Running Systems**

**Required Materials**

Rocky or similar server

Root or sudo permissions

**EXERCISES (Warmup to quickly run through your system and familiarize yourself)**

1.       cd ~

2.       ls

3.       mkdir unit4

4.       mkdir unit4/test/round6   #this fails

5.       mkdir -p unit4/test/round6    #this works, think about why? man (mkdir)

6.       cd unit4

7.       ps      #read the man

8.       ps –ef     #what does this show differently?

9.       ps –ef | grep –i root      #what is the PID of the 4th line?

10.   ps –ef | grep –i root | wc –l      #what does this show you and why might it be useful?.

11.   top            #use q to exit. Inside top, use h to find commands you can use to toggle system info

**Pre-LAB – Check to make sure we have a useful tool and where we get it from**

1.       Real quick check for a package that is useful.

rpm –qa | grep –i iostat       #should find nothing

2.       Let’s find what provides iostat by looking in the YUM (we’ll explore more in later lab)

dnf whatprovides iostat

#should tell you that sysstat provides iostat

3.       Let’s check to see if we have it

rpm –qa | grep –i sysstat

4.       If you don’t, lets install it

dnf install sysstat 

5.       Re-check to verify we have it now

rpm –qa | grep –I sysstat

rpm –qi sysstat<version>

iostat             #we’ll look at this more in a bit

Might be good to ensure you have vim on your system now too. This is the same procedure as above.

rpm –qa | grep –i vim

If it’s there, good.

dnf install vim

If it’s not, install it so you can use vimtutor later (if you need help with vi commands)

**LAB**

1.       Gathering system information release and kernel information

a.       cat /etc/*release

b.       uname

c.       uname -a

d.       uname –r

man uname to see what those options mean if you don’t recognize the values

e.       rpm -qa | grep -i kernel     #what is your kernel number? Highlight it (copy in putty)

f.        rpm –qi <kernel from earlier>       #what does this tell you about your kernel? When was the kernel last updated? What license is your kernel released under?

2.       Check the number of disks

a.       fdisk -l

b.       ls /dev/sd*

When might this command be useful? What are we assuming about the disks for this to work? How many disks are there on this system? How do you know if it’s a partition or a disk?

c.       pvs      #what system are we running if we have physical volumes? What other things can we tell with vgs and lvs?

d.       Use pvdisply, vgdisplay, and lvdisplay to look at your carved up volumes. Thinking back to last week’s lab, what might be interesting from each of those?

e.       Try a command like lvdisplay | egrep "Path|Size"   and see what it shows. Does that output look useful? Try to egrep on some other values “separate with | between search items.

Check some quick disk statistics

f.        iostat –d

g.       iostat –d 2     #Wait for a while, then use crtl + c to break. What did this do? Try changing this to a different number.

h.       iostat –d 2 5    #Don’t break this, just wait. What did this do differently? Why might this be useful?

3.       Check the amount of RAM

a.       cat /proc/meminfo

b.       free

c.       free –m

What do each of these commands show you? How are they useful?

4.       Check the number of processors and processor info

a.       cat /proc/cpuinfo

What type of processors do you have? Google to try to see when they were released.

Look at the flags. Sometimes when compiling these are important to know. This is how you check what execution flags your processor works with.

b.       cat /proc/cpuinfo | grep proc | wc –l

Does this command accurately count the processors?

Check some quick processor statistics

c.       iostat –c

d.       iostat –c 2     #Wait for a while, then use crtl + c to break. What did this do? Try changing this to a different number.

e.       iostat –c 2 5    #Don’t break this, just wait. What did this do differently? Why might this be useful?

Does this look familiar? To what we did earlier with iostat?

5.       Check the system uptime

a.       uptime

b.       man uptime

Read the man for uptime and figure out what those 3 numbers represent. Referencing this server, do you think it is under high load? Why or why not?

6.       Check who has recently logged into the server and who is currently in

a.       last

Last is a command that outputs backwards. (Top is most recent). So it is less than useful without using the more command.

b.       last | more

Were you the last person to log in? Who else has logged in today?

c.       w

d.       who

e.       whoami

how many other users are on this system? What does the pts/0 mean on google?

7.       Check running processes and services

a.       ps –aux | more

b.       ps –ef | more

c.       ps –ef | wc –l

d.       Try to use what you’ve learned to see all the processes owned by your user

e.       Try to use what you’ve learned to count up all of those processes owned by your user

8.       Looking at system usage (historical)

a.       Check processing for last day

sar | more

b.       Check memory for the last day

sar –r | more

Sar is a tool that shows the 10 minute weighted average of the system for the last day. Sar is tremendously useful for showing long periods of activity and system load. It is exactly the opposite in it’s usefulness of spikes or high traffic loads. In a 20 minute period of 100% usage a system may just show to averages times of 50% and 50%, never actually giving accurate info. Problems typically need to be proactively tracked by other means, or with scripts, as we will see below.

Sar can also be run interactively. Run the command `yum whatprovides sar` and you will see that it is sysstat. You may have guessed that sar runs almost exactly like iostat.

Try the same commands from earlier, but with their interactive information

sar 2       #crtl + c to break

sar 2 5

or

sar –r 2

sar –r 2 5

c.       Check sar logs for previous daily usage

cd /var/log/sa/

# ls

sa01  sa02  sa03  sa04  sa05  sar01  sar02  sar03  sar04

sar –f sa03 | head

sar –r –f sa03 | head

#should output just the beginning of 3 July (whatever month you’re in). Most Sar data is kept for just one month but is very configurable. Read man sar for more info.

d.       Sar logs are not kept in a readable format, they are binary. So if you needed to dump all the sar logs from a server, you’d have to output it to a file that is readable. You could do something like this:

                                                               i.      Gather information and move to the right location

cd /var/log/sa

pwd

ls

We know the files we want are in this directory and all look like this sa*

                                                             ii.      Build a loop against that list of files

for file in `ls /var/log/sa/sa??`; do echo "reading this file $file"; done

                                                           iii.      Execute that loop with the output command of sar instead of just saying the filename

for file in `ls /var/log/sa/sa?? | sort -n`; do sar -f $file ; done

                                                           iv.      But that is too much scroll, so let’s also send it to a file for later viewing

for file in `ls /var/log/sa/sa?? | sort -n`; do sar -f $file | tee -a /tmp/sar_data_`hostname`; done

                                                             v.      Let’s verify that file is as long as we expect it to be:

ls –l /tmp/sar_data*

cat /tmp/sar_data_<yourhostname> | wc –l

Is it what you expected? You can also visually check it a number of ways

cat /tmp/<filename>

more /tmp/<filename>

**Exploring Cron**

Your system is running the cron daemon. You can check with:

ps –ef | grep –i cron

systemctl status crond

 This is a tool that wakes up between the 1st and 5th second of every minute and checks to see if it has any tasks it needs to run. It checks in a few places in the system for these tasks. It can either read from a crontab or it can execute tasks from files found in the following locations.

/var/spool/cron   is one location you can ls to check if there are any crontabs on your system

The other locations are directories found under

ls -ld /etc/cron*

These should be self-explanatory in their use.

If you want to see if the user you are running has a crontab use the command ` crontab –l `. If you want to edit, (using your default editor, probably vi) use `crontab –e `

We’ll make a quick crontab entry and I’ll point you here ([https://en.wikipedia.org/wiki/Cron](https://en.wikipedia.org/wiki/Cron)) if you’re interested in learning more.

Crontab format looks like this picture so let’s do these steps

![](data:image/png;base64,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KAOel0W+uGSedIDNHcPJGgu5Qqq6gYyADwRnGMfSrFppd9Dr815NdySQSbsL53ygEDA8vb29d1bNFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQA8qCckUbF9P1p1FIY3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQA3Yvp+tGxfT9adRQB/9k=)

1.       crontab –e

2.       Add this line (using vi commands – Revisit vimtutor if you need help with them)

* * * * * echo 'this is my cronjob runnig at' `date` | wall

3.       Verify with crontab –l

4.       Wait to see if it runs and echos out to wall.

5.       cat /var/spool/cron/root   to see that it is actually stored where I said it was.

6.       This will quickly become very annoying, so I recommend removing that line, or commenting it out (#) from that file.

I can change all kinds of things about this to execute at different times. The one above, we executed every minute through all hours, of every day, of every month.

We could also have done some other things:

Every 2 minutes (divisible by any number you need):

*/2 * * * *

The first and 31st minute of each hour:

1,31 * * * *

The first minute of every 4th hour:

1 */4 * * *

There’s a lot there to explore, I recommend looking into [https://en.wikipedia.org/wiki/Cron](https://en.wikipedia.org/wiki/Cron) or [http://www.tldp.org/LDP/lame/LAME/linux-admin-made-easy/using-cron.html](http://www.tldp.org/LDP/lame/LAME/linux-admin-made-easy/using-cron.html) for more information.

That’s all for this week’s lab. There are a lot of uses for all of these tools above. Most of what I’ve shown here, I’d liken to showing you around a tool box. Nothing here is terribly useful in itself, the value comes from knowing the tool exists and then being able to properly apply it to the problem at hand. I hope you enjoyed this lab.
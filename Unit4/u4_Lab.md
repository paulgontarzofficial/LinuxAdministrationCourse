
**ProLUG – Unit 4 Operate Running Systems**

**Required Materials**

Rocky or similar server

Root or sudo permissions

**EXERCISES (Warmup to quickly run through your system and familiarize yourself)**

1.       cd ~

2.       ls

3.       mkdir unit4

4.       mkdir unit4/test/round6   #this fails
		- `It fails due to the -p argument not being specified. in order to create parent directories, we need to pass the -p argument.

5.       mkdir -p unit4/test/round6    #this works, think about why? man (mkdir)
		- `We passed the -p argument, thus we were able to create those directories that were specified. 

6.       cd unit4

7.       ps      #read the man

8.       ps –ef     #what does this show differently?
		`-e: Selects all the processes on the machine. 
		`-f: Puts the output into a full-format listing

9.       ps –ef | grep –i root      #what is the PID of the 4th line?
Output: 
```
[root@rocky7 unit4]# ps -ef | grep -i root
root           1       0  0 Apr24 ?        00:00:12 /sbin/init
root           2       0  0 Apr24 ?        00:00:01 [kthreadd]
root           3       2  0 Apr24 ?        00:00:00 [rcu_gp]
root           4       2  0 Apr24 ?        00:00:00 [rcu_par_gp]
```

According to the output listed above, I see that on the 4th line, we have PID 4 for the process rcu_par_gp. 

10.   ps –ef | grep –i root | wc –l      #what does this show you and why might it be useful?.
```
Output: 
[root@rocky7 unit4]# ps -ef | grep -i root | wc -l
95
```

```
This shows us how many root processes are currently running. This can be useful because we can isolate the processes by the user that has them started, we can view root only privileges. 
```

11.   top            #use q to exit. Inside top, use h to find commands you can use to toggle system info

**Pre-LAB – Check to make sure we have a useful tool and where we get it from**

1.       Real quick check for a package that is useful.

rpm –qa | grep –i iostat       #should find nothing
```
Output: 
[root@rocky7 unit4]# rpm -qa | grep -i iostat
[root@rocky7 unit4]#
```


2.       Let’s find what provides iostat by looking in the YUM (we’ll explore more in later lab)

dnf whatprovides iostat

#should tell you that sysstat provides iostat
Output: 
```
[root@rocky7 unit4]# dnf whatprovides iostat
Last metadata expiration check: 0:30:48 ago on Tue Apr 29 14:55:50 2025.
sysstat-12.5.4-9.el9.x86_64 : Collection of performance monitoring tools for Linux
Repo        : @System
Matched from:
Filename    : /usr/bin/iostat

sysstat-12.5.4-9.el9.x86_64 : Collection of performance monitoring tools for Linux
Repo        : appstream
Matched from:
Filename    : /usr/bin/iostat

[root@rocky7 unit4]#
```


3.       Let’s check to see if we have it

rpm –qa | grep –i sysstat
Output: 
```
[root@rocky7 unit4]# rpm -qa | grep -i sysstat
sysstat-12.5.4-9.el9.x86_64
[root@rocky7 unit4]#
```

4.       If you don’t, lets install it

dnf install sysstat 

5.       Re-check to verify we have it now

rpm –qa | grep –I sysstat

```
rpm –qi sysstat <version>
```

iostat             #we’ll look at this more in a bit

Might be good to ensure you have vim on your system now too. This is the same procedure as above.

rpm –qa | grep –i vim

If it’s there, good.

dnf install vim

If it’s not, install it so you can use vimtutor later (if you need help with vi commands)

**LAB**

1.       Gathering system information release and kernel information

a.       cat /etc/*release
Output: 
```
[root@rocky7 unit4]# cat /etc/*release
NAME="Rocky Linux"
VERSION="9.5 (Blue Onyx)"
ID="rocky"
ID_LIKE="rhel centos fedora"
VERSION_ID="9.5"
PLATFORM_ID="platform:el9"
PRETTY_NAME="Rocky Linux 9.5 (Blue Onyx)"
ANSI_COLOR="0;32"
LOGO="fedora-logo-icon"
CPE_NAME="cpe:/o:rocky:rocky:9::baseos"
HOME_URL="https://rockylinux.org/"
VENDOR_NAME="RESF"
VENDOR_URL="https://resf.org/"
BUG_REPORT_URL="https://bugs.rockylinux.org/"
SUPPORT_END="2032-05-31"
ROCKY_SUPPORT_PRODUCT="Rocky-Linux-9"
ROCKY_SUPPORT_PRODUCT_VERSION="9.5"
REDHAT_SUPPORT_PRODUCT="Rocky Linux"
REDHAT_SUPPORT_PRODUCT_VERSION="9.5"
Rocky Linux release 9.5 (Blue Onyx)
Rocky Linux release 9.5 (Blue Onyx)
Rocky Linux release 9.5 (Blue Onyx)
[root@rocky7 unit4]#
```

b.       uname: Print system information
Output: 
```
[root@rocky7 unit4]# uname
Linux
[root@rocky7 unit4]# `
```

c.       uname -a: Print all information, in the following order
Output: 
```
[root@rocky7 unit4]# uname -a
Linux rocky7 5.14.0-427.42.1.el9_4.x86_64 #1 SMP PREEMPT_DYNAMIC Thu Oct 31 14:01:51 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
[root@rocky7 unit4]#
```


d.       uname –r: Print the kernel release 
Output: 
```
[root@rocky7 unit4]# uname -r
5.14.0-427.42.1.el9_4.x86_64
[root@rocky7 unit4]#
```


man uname to see what those options mean if you don’t recognize the values

e.       rpm -qa | grep -i kernel     #what is your kernel number? Highlight it (copy in putty)
Output: 
```
[root@rocky7 unit4]# rpm -qa | grep -i kernel
kernel-modules-core-5.14.0-427.18.1.el9_4.x86_64
kernel-core-5.14.0-427.18.1.el9_4.x86_64
kernel-modules-5.14.0-427.18.1.el9_4.x86_64
kernel-core-5.14.0-427.42.1.el9_4.x86_64
kernel-modules-core-5.14.0-427.42.1.el9_4.x86_64
kernel-modules-5.14.0-427.42.1.el9_4.x86_64
kernel-core-5.14.0-503.23.1.el9_5.x86_64
kernel-modules-core-5.14.0-503.23.1.el9_5.x86_64
kernel-modules-5.14.0-503.23.1.el9_5.x86_64
```

f.        `rpm –qi <kernel from earlier>`       #what does this tell you about your kernel? When was the kernel last updated? What license is your kernel released under?
Output: 
```
[root@rocky7 unit4]# rpm -qi kernel-core-5.14.0-503.23.1.el9_5.x86_64
Name        : kernel-core
Version     : 5.14.0
Release     : 503.23.1.el9_5
Architecture: x86_64
Install Date: Mon Feb 10 18:37:23 2025
Group       : Unspecified
Size        : 68807354
License     : ((GPL-2.0-only WITH Linux-syscall-note) OR BSD-2-Clause) AND ((GPL-2.0-only WITH Linux-syscall-note) OR BSD-3-Clause) AND ((GPL-2.0-only WITH Linux-syscall-note) OR CDDL-1.0) AND ((GPL-2.0-only WITH Linux-syscall-note) OR Linux-OpenIB) AND ((GPL-2.0-only WITH Linux-syscall-note) OR MIT) AND ((GPL-2.0-or-later WITH Linux-syscall-note) OR BSD-3-Clause) AND ((GPL-2.0-or-later WITH Linux-syscall-note) OR MIT) AND Apache-2.0 AND BSD-2-Clause AND BSD-3-Clause AND BSD-3-Clause-Clear AND GFDL-1.1-no-invariants-or-later AND GPL-1.0-or-later AND (GPL-1.0-or-later OR BSD-3-Clause) AND (GPL-1.0-or-later WITH Linux-syscall-note) AND GPL-2.0-only AND (GPL-2.0-only OR Apache-2.0) AND (GPL-2.0-only OR BSD-2-Clause) AND (GPL-2.0-only OR BSD-3-Clause) AND (GPL-2.0-only OR CDDL-1.0) AND (GPL-2.0-only OR GFDL-1.1-no-invariants-or-later) AND (GPL-2.0-only OR GFDL-1.2-no-invariants-only) AND (GPL-2.0-only WITH Linux-syscall-note) AND GPL-2.0-or-later AND (GPL-2.0-or-later OR BSD-2-Clause) AND (GPL-2.0-or-later OR BSD-3-Clause) AND (GPL-2.0-or-later OR CC-BY-4.0) AND (GPL-2.0-or-later WITH GCC-exception-2.0) AND (GPL-2.0-or-later WITH Linux-syscall-note) AND ISC AND LGPL-2.0-or-later AND (LGPL-2.0-or-later OR BSD-2-Clause) AND (LGPL-2.0-or-later WITH Linux-syscall-note) AND LGPL-2.1-only AND (LGPL-2.1-only OR BSD-2-Clause) AND (LGPL-2.1-only WITH Linux-syscall-note) AND LGPL-2.1-or-later AND (LGPL-2.1-or-later WITH Linux-syscall-note) AND (Linux-OpenIB OR GPL-2.0-only) AND (Linux-OpenIB OR GPL-2.0-only OR BSD-2-Clause) AND Linux-man-pages-copyleft AND MIT AND (MIT OR GPL-2.0-only) AND (MIT OR GPL-2.0-or-later) AND (MIT OR LGPL-2.1-only) AND (MPL-1.1 OR GPL-2.0-only) AND (X11 OR GPL-2.0-only) AND (X11 OR GPL-2.0-or-later) AND Zlib
Signature   : RSA/SHA256, Thu Feb  6 11:30:18 2025, Key ID 702d426d350d275d
Source RPM  : kernel-5.14.0-503.23.1.el9_5.src.rpm
Build Date  : Thu Feb  6 04:36:54 2025
Build Host  : iad1-prod-build001.bld.equ.rockylinux.org
Packager    : Release Engineering <releng@rockylinux.org>
Vendor      : Rocky
URL         : https://www.kernel.org/
Summary     : The Linux kernel
Description :
The kernel package contains the Linux kernel (vmlinuz), the core of any
Linux operating system.  The kernel handles the basic functions
of the operating system: memory allocation, process allocation, device
input and output, etc.
[root@rocky7 unit4]#
```


2.       Check the number of disks

a.       fdisk -l
Output: 
```
[root@rocky7 unit4]# fdisk -l
Disk /dev/xvde: 3 GiB, 3221225472 bytes, 6291456 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/xvdb: 3 GiB, 3221225472 bytes, 6291456 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/xvda: 15 GiB, 16106127360 bytes, 31457280 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/xvdc: 3 GiB, 3221225472 bytes, 6291456 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
[root@rocky7 unit4]#
```


b.       ls /dev/sd*

```
When might this command be useful? 
	- This can be useful to view the available disks and also your partition tables. 
What are we assuming about the disks for this to work? 
	 - ???
How many disks are there on this system? 
	- Currently there are four virtual disks. 
How do you know if it’s a partition or a disk?
	- Disks will have the name of the physical device
		- ex. /dev/sda
	- Partitions will have the physical device name plus a number at the end that represents the partition. 
		- ex. /dev/sda1
```

c.       pvs 

```
what system are we running if we have physical volumes? 
	- If we have physical volumes then we would be using LVM (Logical Volume Manager)

What other things can we tell with vgs and lvs?
	- VGS: Displays information about volume groups. 
	- LVS: Displays information about logical volumes. 

d.       Use pvdisply, vgdisplay, and lvdisplay to look at your carved up volumes. Thinking back to last week’s lab, what might be interesting from each of those?

e.       Try a command like lvdisplay | egrep "Path|Size"   and see what it shows. Does that output look useful? Try to egrep on some other values “separate with | between search items.
```

Check some quick disk statistics

f.        iostat –d
Output: 
```
[root@rocky7 unit4]# iostat -d
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky7)     04/29/25        _x86_64_        (2 CPU)

Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
xvda              0.00         0.01         0.00         0.00       5156          0          0
xvdb              0.00         0.01         0.00         0.00       5044          0          0
xvdc              0.00         0.01         0.00         0.00       5044          0          0
xvde              0.00         0.01         0.00         0.00       5044          0          0


[root@rocky7 unit4]#
```


g.       iostat –d 2     #Wait for a while, then use crtl + c to break. What did this do? Try changing this to a different number.
Output: 
```
[root@rocky7 unit4]# iostat -d 2
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky7)     04/29/25        _x86_64_        (2 CPU)

Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
xvda              0.00         0.01         0.00         0.00       5156          0          0
xvdb              0.00         0.01         0.00         0.00       5044          0          0
xvdc              0.00         0.01         0.00         0.00       5044          0          0
xvde              0.00         0.01         0.00         0.00       5044          0          0


Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
xvda              0.00         0.00         0.00         0.00          0          0          0
xvdb              0.00         0.00         0.00         0.00          0          0          0
xvdc              0.00         0.00         0.00         0.00          0          0          0
xvde              0.00         0.00         0.00         0.00          0          0          0


Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
xvda              0.00         0.00         0.00         0.00          0          0          0
xvdb              0.00         0.00         0.00         0.00          0          0          0
xvdc              0.00         0.00         0.00         0.00          0          0          0
xvde              0.00         0.00         0.00         0.00          0          0          0


Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
xvda              0.00         0.00         0.00         0.00          0          0          0
xvdb              0.00         0.00         0.00         0.00          0          0          0
xvdc              0.00         0.00         0.00         0.00          0          0          0
xvde              0.00         0.00         0.00         0.00          0          0          0

^C
[root@rocky7 unit4]#
```


h.       iostat –d 2 5    
Output: 
```
[root@rocky7 unit4]# iostat -d 2 5
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky7)     04/29/25        _x86_64_        (2 CPU)

Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
xvda              0.00         0.01         0.00         0.00       5156          0          0
xvdb              0.00         0.01         0.00         0.00       5044          0          0
xvdc              0.00         0.01         0.00         0.00       5044          0          0
xvde              0.00         0.01         0.00         0.00       5044          0          0


Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
xvda              0.00         0.00         0.00         0.00          0          0          0
xvdb              0.00         0.00         0.00         0.00          0          0          0
xvdc              0.00         0.00         0.00         0.00          0          0          0
xvde              0.00         0.00         0.00         0.00          0          0          0


Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
xvda              0.00         0.00         0.00         0.00          0          0          0
xvdb              0.00         0.00         0.00         0.00          0          0          0
xvdc              0.00         0.00         0.00         0.00          0          0          0
xvde              0.00         0.00         0.00         0.00          0          0          0


Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
xvda              0.00         0.00         0.00         0.00          0          0          0
xvdb              0.00         0.00         0.00         0.00          0          0          0
xvdc              0.00         0.00         0.00         0.00          0          0          0
xvde              0.00         0.00         0.00         0.00          0          0          0


Device             tps    kB_read/s    kB_wrtn/s    kB_dscd/s    kB_read    kB_wrtn    kB_dscd
xvda              0.00         0.00         0.00         0.00          0          0          0
xvdb              0.00         0.00         0.00         0.00          0          0          0
xvdc              0.00         0.00         0.00         0.00          0          0          0
xvde              0.00         0.00         0.00         0.00          0          0          0


[root@rocky7 unit4]#
```

Don’t break this, just wait. 
```
What did this do differently? 
	- This displays 5 reports at a two second interval for all devices. 
Why might this be useful?
	- This allows us to monitor devices for disk utilization within a given time frame while automatically halting the process if we decide to specify how many reports are given. 
```


3.       Check the amount of RAM

a.       cat /proc/meminfo
Output: 
```
[root@rocky7 unit4]# cat /proc/meminfo
MemTotal:        3985404 kB
MemFree:         1440512 kB
MemAvailable:    1381120 kB
Buffers:               0 kB
Cached:          2143384 kB
SwapCached:            0 kB
Active:          2245272 kB
Inactive:             44 kB
Active(anon):    2192640 kB
Inactive(anon):        0 kB
Active(file):      52632 kB
Inactive(file):       44 kB
Unevictable:           0 kB
Mlocked:               0 kB
SwapTotal:             0 kB
SwapFree:              0 kB
Zswap:                 0 kB
Zswapped:              0 kB
Dirty:                 0 kB
Writeback:             0 kB
AnonPages:        100308 kB
Mapped:           107524 kB
Shmem:           2090708 kB
KReclaimable:      62460 kB
Slab:             178452 kB
SReclaimable:      62460 kB
SUnreclaim:       115992 kB
KernelStack:        3168 kB
PageTables:         2132 kB
SecPageTables:         0 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:     1992700 kB
Committed_AS:    2627140 kB
VmallocTotal:   34359738367 kB
VmallocUsed:       33472 kB
VmallocChunk:          0 kB
Percpu:            26368 kB
HardwareCorrupted:     0 kB
AnonHugePages:     40960 kB
ShmemHugePages:        0 kB
ShmemPmdMapped:        0 kB
FileHugePages:         0 kB
FilePmdMapped:         0 kB
CmaTotal:              0 kB
CmaFree:               0 kB
Unaccepted:            0 kB
HugePages_Total:       0
HugePages_Free:        0
HugePages_Rsvd:        0
HugePages_Surp:        0
Hugepagesize:       2048 kB
Hugetlb:               0 kB
DirectMap4k:      118784 kB
DirectMap2M:     3018752 kB
DirectMap1G:     1048576 kB
[root@rocky7 unit4]#
```


b.       free
Output: 
```
[root@rocky7 unit4]# free
               total        used        free      shared  buff/cache   available
Mem:         3985404     2602920     1441876     2090712     2205848     1382484
Swap:              0           0           0
[root@rocky7 unit4]#
```


c.       free –m
Output: 
```
[root@rocky7 unit4]# free -m
               total        used        free      shared  buff/cache   available
Mem:            3891        2541        1408        2041        2154        1350
Swap:              0           0           0
[root@rocky7 unit4]#
```

What do each of these commands show you? How are they useful?

```
cat /proc/meminfo: Kernel file that provides usage report about memory on the system. It can show you important information that could assist in troubleshooting Linux systems. 

free: Displays the amount of free and used memory in the system. 

free -m: Displays the amount of free and used memory, however the -m argument shows data in Megabytes vice Bytes. 
```

4.       Check the number of processors and processor info

a.       cat /proc/cpuinfo
Output: 
```
[root@rocky19 ~]# cat /proc/cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 45
model name      : Intel(R) Xeon(R) CPU E5-2665 0 @ 2.40GHz
stepping        : 7
microcode       : 0x71a
cpu MHz         : 2400.018
cache size      : 20480 KB
physical id     : 0
siblings        : 1
core id         : 0
cpu cores       : 1
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush acpi mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good n opl cpuid tsc_known_freq pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx hypervisor lahf_lm cpuid_fault pti ssbd ibrs ibpb stibp xsaveop t md_clear flush_l1d
bugs            : cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf mds swapgs itlb_multihit mmio_unknown bhi
bogomips        : 4800.03
clflush size    : 64
cache_alignment : 64
address sizes   : 46 bits physical, 48 bits virtual
power management:

processor       : 1
vendor_id       : GenuineIntel
cpu family      : 6
model           : 45
model name      : Intel(R) Xeon(R) CPU E5-2665 0 @ 2.40GHz
stepping        : 7
microcode       : 0x71a
cpu MHz         : 2400.018
cache size      : 20480 KB
physical id     : 1
siblings        : 1
core id         : 0
cpu cores       : 1
apicid          : 2
initial apicid  : 2
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush acpi mmx fxsr sse sse2 ss ht syscall nx pdpe1gb rdtscp lm constant_tsc rep_good n opl cpuid tsc_known_freq pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt tsc_deadline_timer aes xsave avx hypervisor lahf_lm cpuid_fault pti ssbd ibrs ibpb stibp xsaveop t md_clear flush_l1d
bugs            : cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf mds swapgs itlb_multihit mmio_unknown bhi
bogomips        : 4800.03
clflush size    : 64
cache_alignment : 64
address sizes   : 46 bits physical, 48 bits virtual
power management:

[root@rocky19 ~]#
```


What type of processors do you have? 
	-  Intel(R) Xeon(R) CPU E5-2665 0 @ 2.40GHz

Google to try to see when they were released.
	- March, 2012

Look at the flags. Sometimes when compiling these are important to know. This is how you check what execution flags your processor works with.

b.       cat /proc/cpuinfo | grep proc | wc –l
Output: 
```
[root@rocky19 ~]# cat /proc/cpuinfo | grep proc | wc -l
2
[root@rocky19 ~]#
```

Does this command accurately count the processors?
	- Yes, the command correctly identifies that there are two processors for that system. 

Check some quick processor statistics

c.       iostat –c
Output: 
```
[root@rocky19 ~]# iostat -c
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky19)    04/29/25        _x86_64_        (2 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.07    0.02    0.39    0.07    0.21   99.24



[root@rocky19 ~]#
```


d.       iostat –c 2     
Output: 
```
[root@rocky19 ~]# iostat -c 2
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky19)    04/29/25        _x86_64_        (2 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.07    0.02    0.39    0.07    0.21   99.24



avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.00    0.00    0.25    0.00    0.00   99.75


^C
[root@rocky19 ~]#
```
What did this do? 
	- This is acting the same as before when we were checking disk utilization. We set a timer for iostat to display information every two seconds. 

Try changing this to a different number.

e.       iostat –c 2 5    
Output: 
```
[root@rocky19 ~]# iostat -c 2 5
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky19)    04/29/25        _x86_64_        (2 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.07    0.02    0.39    0.07    0.21   99.24



avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.50    0.00    0.75    0.25    0.25   98.26



avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.00    0.00    0.50    0.00    0.00   99.50



avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.00    0.00    0.25    0.00    0.25   99.50



avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.00    0.00    0.50    0.25    0.25   99.00



[root@rocky19 ~]#
```

What did this do differently? 
	- iostat was instructed to give us a report in a two second interval, for a total of five reports. 

Why might this be useful?
	- We can utilize iostat to monitor our CPU usage. 

Does this look familiar? To what we did earlier with iostat?
	- Yes, very familiar! 

5.       Check the system uptime

a.       uptime
Output: 
```
[root@rocky19 ~]# uptime
 16:58:16 up 2 days,  8:18,  1 user,  load average: 0.00, 0.00, 0.00
[root@rocky19 ~]#
```
b.       man uptime

Read the man for uptime and figure out what those 3 numbers represent. 
	- The three numbers at the end are your average load for every 1, 5, and 15 minutes. 

Referencing this server, do you think it is under high load? Why or why not?
	- No, I do not believe that this server is under a high load due to the fact that the load average over the past 15 minutes is at 0.00. 

6.       Check who has recently logged into the server and who is currently in

a.       last
Output: 
```
[root@rocky19 ~]# last
root     pts/0        192.168.11.245   Tue Apr 29 16:41   still logged in
root     pts/0        192.168.200.25   Mon Apr 28 23:55 - 23:55  (00:00)
root     pts/0        192.168.200.25   Mon Apr 28 23:55 - 23:55  (00:00)
root     pts/0        192.168.200.25   Mon Apr 28 16:15 - 16:41  (00:26)
root     pts/0        192.168.200.25   Mon Apr 28 09:05 - 09:05  (00:00)
root     pts/0        192.168.200.25   Sun Apr 27 23:55 - 23:55  (00:00)
root     pts/0        192.168.200.25   Sun Apr 27 23:55 - 23:55  (00:00)
root     pts/0        192.168.11.245   Sun Apr 27 09:09 - 09:10  (00:01)
reboot   system boot  5.14.0-427.42.1. Sun Apr 27 08:41   still running

wtmp begins Sun Apr 27 08:41:17 2025
[root@rocky19 ~]#
```
Last is a command that outputs backwards. (Top is most recent). So it is less than useful without using the more command.

b.       last | more
Output: 
```
[root@rocky19 ~]# last | more
root     pts/0        192.168.11.245   Tue Apr 29 16:41   still logged in
root     pts/0        192.168.200.25   Mon Apr 28 23:55 - 23:55  (00:00)
root     pts/0        192.168.200.25   Mon Apr 28 23:55 - 23:55  (00:00)
root     pts/0        192.168.200.25   Mon Apr 28 16:15 - 16:41  (00:26)
root     pts/0        192.168.200.25   Mon Apr 28 09:05 - 09:05  (00:00)
root     pts/0        192.168.200.25   Sun Apr 27 23:55 - 23:55  (00:00)
root     pts/0        192.168.200.25   Sun Apr 27 23:55 - 23:55  (00:00)
root     pts/0        192.168.11.245   Sun Apr 27 09:09 - 09:10  (00:01)
reboot   system boot  5.14.0-427.42.1. Sun Apr 27 08:41   still running

wtmp begins Sun Apr 27 08:41:17 2025
[root@rocky19 ~]#
```
Were you the last person to log in? Who else has logged in today?
	- As of right now, I was the only person to have logged in to this server today, however there were multiple logins from yesterday, 28APR25. 

c.       w
Output: 
```
[root@rocky19 ~]# w
 17:03:00 up 2 days,  8:23,  1 user,  load average: 0.00, 0.00, 0.00
USER     TTY        LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0     16:41    0.00s  0.08s  0.01s w
[root@rocky19 ~]#
```

d.       who
Output: 
```
[root@rocky19 ~]# who
root     pts/0        2025-04-29 16:41 (192.168.11.245)
[root@rocky19 ~]#
```

e.       whoami
Output: 
```
[root@rocky19 ~]# whoami
root
[root@rocky19 ~]#
```
how many other users are on this system? What does the pts/0 mean on google?
	- There are currently no other users logged in at this time. 
	- pts/0 is the current "psuedo terminal" the user is logged in on. 

7.       Check running processes and services

a.       ps –aux | more
Output: 
```
[root@rocky19 ~]# ps -aux | more
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.4 107840 16192 ?        Ss   Apr27   0:06 /sbin/init
root           2  0.0  0.0      0     0 ?        S    Apr27   0:00 [kthreadd]
root           3  0.0  0.0      0     0 ?        I<   Apr27   0:00 [rcu_gp]
root           4  0.0  0.0      0     0 ?        I<   Apr27   0:00 [rcu_par_gp]
root           5  0.0  0.0      0     0 ?        I<   Apr27   0:00 [slub_flushwq]
root           6  0.0  0.0      0     0 ?        I<   Apr27   0:00 [netns]
root          10  0.0  0.0      0     0 ?        I<   Apr27   0:00 [mm_percpu_wq]
root          12  0.0  0.0      0     0 ?        I    Apr27   0:00 [rcu_tasks_kthre]
root          13  0.0  0.0      0     0 ?        I    Apr27   0:00 [rcu_tasks_rude_]
root          14  0.0  0.0      0     0 ?        I    Apr27   0:00 [rcu_tasks_trace]
root          15  0.0  0.0      0     0 ?        S    Apr27   0:00 [ksoftirqd/0]
root          16  0.0  0.0      0     0 ?        I    Apr27   0:10 [rcu_preempt]
root          17  0.0  0.0      0     0 ?        S    Apr27   0:01 [migration/0]
root          18  0.0  0.0      0     0 ?        S    Apr27   0:00 [idle_inject/0]
root          20  0.0  0.0      0     0 ?        S    Apr27   0:00 [cpuhp/0]
root          21  0.0  0.0      0     0 ?        S    Apr27   0:00 [cpuhp/1]
root          22  0.0  0.0      0     0 ?        S    Apr27   0:00 [idle_inject/1]
root          23  0.0  0.0      0     0 ?        S    Apr27   0:01 [migration/1]
root          24  0.0  0.0      0     0 ?        S    Apr27   0:00 [ksoftirqd/1]
root          26  0.0  0.0      0     0 ?        I<   Apr27   0:00 [kworker/1:0H-events_highpri]
root          28  0.0  0.0      0     0 ?        S    Apr27   0:00 [kdevtmpfs]
root          29  0.0  0.0      0     0 ?        I<   Apr27   0:00 [inet_frag_wq]
root          30  0.0  0.0      0     0 ?        S    Apr27   0:00 [kauditd]
root          32  0.0  0.0      0     0 ?        S    Apr27   0:00 [khungtaskd]
root          33  0.0  0.0      0     0 ?        S    Apr27   0:00 [oom_reaper]
root          34  0.0  0.0      0     0 ?        I<   Apr27   0:00 [writeback]
root          35  0.0  0.0      0     0 ?        S    Apr27   0:04 [kcompactd0]
root          36  0.0  0.0      0     0 ?        SN   Apr27   0:00 [ksmd]
root          37  0.0  0.0      0     0 ?        SN   Apr27   0:02 [khugepaged]
root          38  0.0  0.0      0     0 ?        I<   Apr27   0:00 [cryptd]
root          39  0.0  0.0      0     0 ?        I<   Apr27   0:00 [kintegrityd]
root          40  0.0  0.0      0     0 ?        I<   Apr27   0:00 [kblockd]
root          41  0.0  0.0      0     0 ?        I<   Apr27   0:00 [blkcg_punt_bio]
root          42  0.0  0.0      0     0 ?        I<   Apr27   0:00 [tpm_dev_wq]
root          43  0.0  0.0      0     0 ?        I<   Apr27   0:00 [md]
root          44  0.0  0.0      0     0 ?        I<   Apr27   0:00 [md_bitmap]
root          45  0.0  0.0      0     0 ?        I<   Apr27   0:00 [edac-poller]
root          46  0.0  0.0      0     0 ?        S    Apr27   0:00 [watchdogd]
root          48  0.0  0.0      0     0 ?        I<   Apr27   0:00 [kworker/1:1H-kblockd]
root          50  0.0  0.0      0     0 ?        S    Apr27   0:00 [kswapd0]
root          55  0.0  0.0      0     0 ?        I<   Apr27   0:00 [kthrotld]
root          62  0.0  0.0      0     0 ?        I<   Apr27   0:00 [acpi_thermal_pm]
root          63  0.0  0.0      0     0 ?        S    Apr27   0:00 [xenbus]
root          64  0.0  0.0      0     0 ?        S    Apr27   0:00 [xenwatch]
root          65  0.0  0.0      0     0 ?        S    Apr27   0:00 [khvcd]
root          66  0.0  0.0      0     0 ?        I<   Apr27   0:00 [kmpath_rdacd]
root          67  0.0  0.0      0     0 ?        I<   Apr27   0:00 [kaluad]
root          68  0.0  0.0      0     0 ?        I<   Apr27   0:00 [mld]
root          69  0.0  0.0      0     0 ?        I<   Apr27   0:00 [ipv6_addrconf]
root          79  0.0  0.0      0     0 ?        I<   Apr27   0:00 [kstrp]
root          84  0.0  0.0      0     0 ?        I<   Apr27   0:00 [zswap-shrink]
root          85  0.0  0.0      0     0 ?        I<   Apr27   0:00 [kworker/u129:0]
root         188  0.0  0.0      0     0 ?        I<   Apr27   0:00 [kworker/0:1H-kblockd]
root         356  0.0  0.0      0     0 ?        I<   Apr27   0:00 [rpciod]
root         357  0.0  0.0      0     0 ?        I<   Apr27   0:00 [xprtiod]
root         447  0.0  0.0      0     0 ?        I<   Apr27   0:00 [ata_sff]
root         448  0.0  0.0      0     0 ?        S    Apr27   0:00 [scsi_eh_0]
root         449  0.0  0.0      0     0 ?        I<   Apr27   0:00 [scsi_tmf_0]
root         450  0.0  0.0      0     0 ?        S    Apr27   0:00 [scsi_eh_1]
root         451  0.0  0.0      0     0 ?        I<   Apr27   0:00 [scsi_tmf_1]
root         487  0.0  0.0      0     0 ?        I<   Apr27   0:00 [kworker/0:2H-kblockd]
root         670  0.0  0.2  27676 11392 ?        Ss   Apr27   0:13 /usr/lib/systemd/systemd-journald
rpc          697  0.0  0.1  14172  5760 ?        Ss   Apr27   0:00 /usr/bin/rpcbind -w -f
root         699  0.0  0.0  19028  2416 ?        S<sl Apr27   0:00 /sbin/auditd
root         728  0.0  0.3  36128 12124 ?        Ss   Apr27   0:00 /usr/lib/systemd/systemd-udevd
dbus         747  0.0  0.1  10840  4864 ?        Ss   Apr27   0:00 /usr/bin/dbus-broker-launch --scope system --audit
dbus         754  0.0  0.0   5152  2944 ?        S    Apr27   0:00 dbus-broker --log 4 --controller 9 --machine-id f99ee81f280c48a39afefda470f07f04 --max-bytes 536870912 --max-fds
4096 --max-matches 131072 --audit
root         757  0.0  1.0 125896 42460 ?        Ssl  Apr27   0:00 /usr/bin/python3 -s /usr/sbin/firewalld --nofork --nopid
root         759  0.0  0.2  21984 11512 ?        Ss   Apr27   0:01 /usr/lib/systemd/systemd-logind
root         760  0.0  0.4 395380 16380 ?        Ssl  Apr27   0:00 /usr/libexec/udisks2/udisksd
polkitd      765  0.0  0.6 2579336 24492 ?       Ssl  Apr27   0:00 /usr/lib/polkit-1/polkitd --no-debug
root         793  0.0  0.4 253768 18432 ?        Ssl  Apr27   1:26 /usr/sbin/NetworkManager --no-daemon
root         824  0.0  0.2  16740  9600 ?        Ss   Apr27   0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root         826  0.0  0.3 1233032 13748 ?       Ssl  Apr27   0:03 /warewulf/wwclient
root         828  0.0  0.0  53320  3444 ?        Ssl  Apr27   0:00 /usr/sbin/gssproxy -D
root         879  0.0  0.0      0     0 ?        I<   Apr27   0:00 [ttm]
root         899  0.0  0.1 164848  7304 ?        Ssl  Apr27   0:16 /usr/sbin/rsyslogd -n
root         911  0.0  0.0      0     0 ?        I    Apr27   0:03 [kworker/u128:6-events_unbound]
root         912  0.0  0.0      0     0 ?        I<   Apr27   0:00 [nfsiod]
root         924  0.0  0.0      0     0 ?        S    Apr27   0:00 [NFSv4 callback]
root         940  0.4  1.7 1446760 71412 ?       Ssl  Apr27  16:13 /opt/promtail/promtail-linux-amd64 -config.file=/opt/promtail/promtail-local-config.yaml
root         942  0.0  0.0   3020  1920 tty1     Ss+  Apr27   0:00 /sbin/agetty -o -p -- \u --noclear - linux
root         943  0.0  0.0   3064  1920 hvc0     Ss+  Apr27   0:00 /sbin/agetty -o -p -- \u --keep-baud 115200,57600,38400,9600 - vt220
root        1062  0.0  0.0      0     0 ?        I    Apr27   0:06 [kworker/u128:2-events_unbound]
root        1101  0.0  0.0      0     0 ?        I<   Apr27   0:00 [tls-strp]
root        6175  0.0  0.0      0     0 ?        I    Apr28   0:02 [kworker/u128:0-rpciod]
root        7069  0.0  0.0      0     0 ?        I    08:05   0:10 [kworker/1:0-events_power_efficient]
root        7972  0.0  0.2  20068 11776 ?        Ss   16:41   0:00 sshd: root [priv]
root        7976  0.0  0.3  23824 13964 ?        Ss   16:41   0:00 /usr/lib/systemd/systemd --user
root        7978  0.0  0.1 109032  7272 ?        S    16:41   0:00 (sd-pam)
root        7992  0.0  0.1  20068  7128 ?        R    16:41   0:00 sshd: root@pts/0
root        7993  0.0  0.0   4840  3968 pts/0    Ss   16:41   0:00 -bash
root        8071  0.0  0.0      0     0 ?        I    16:52   0:00 [kworker/0:0-events]
root        8104  0.0  0.0      0     0 ?        I    17:00   0:00 [kworker/0:1-events]
root        8119  0.0  0.0      0     0 ?        I    17:04   0:00 [kworker/1:1-ata_sff]
root        8127  0.0  0.0      0     0 ?        I    17:10   0:00 [kworker/1:2-ata_sff]
root        8135  0.0  0.1  16776  6272 pts/0    R+   17:13   0:00 ps -aux
root        8136  0.0  0.0   3240  2176 pts/0    R+   17:13   0:00 more

[root@rocky19 ~]# 

b.       ps –ef | more
Output: 
[root@rocky19 ~]# ps -ef | more
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 Apr27 ?        00:00:06 /sbin/init
root           2       0  0 Apr27 ?        00:00:00 [kthreadd]
root           3       2  0 Apr27 ?        00:00:00 [rcu_gp]
root           4       2  0 Apr27 ?        00:00:00 [rcu_par_gp]
root           5       2  0 Apr27 ?        00:00:00 [slub_flushwq]
root           6       2  0 Apr27 ?        00:00:00 [netns]
root          10       2  0 Apr27 ?        00:00:00 [mm_percpu_wq]
root          12       2  0 Apr27 ?        00:00:00 [rcu_tasks_kthre]
root          13       2  0 Apr27 ?        00:00:00 [rcu_tasks_rude_]
root          14       2  0 Apr27 ?        00:00:00 [rcu_tasks_trace]
root          15       2  0 Apr27 ?        00:00:00 [ksoftirqd/0]
root          16       2  0 Apr27 ?        00:00:10 [rcu_preempt]
root          17       2  0 Apr27 ?        00:00:01 [migration/0]
root          18       2  0 Apr27 ?        00:00:00 [idle_inject/0]
root          20       2  0 Apr27 ?        00:00:00 [cpuhp/0]
root          21       2  0 Apr27 ?        00:00:00 [cpuhp/1]
root          22       2  0 Apr27 ?        00:00:00 [idle_inject/1]
root          23       2  0 Apr27 ?        00:00:01 [migration/1]
root          24       2  0 Apr27 ?        00:00:00 [ksoftirqd/1]
root          26       2  0 Apr27 ?        00:00:00 [kworker/1:0H-events_highpri]
root          28       2  0 Apr27 ?        00:00:00 [kdevtmpfs]
root          29       2  0 Apr27 ?        00:00:00 [inet_frag_wq]
root          30       2  0 Apr27 ?        00:00:00 [kauditd]
root          32       2  0 Apr27 ?        00:00:00 [khungtaskd]
root          33       2  0 Apr27 ?        00:00:00 [oom_reaper]
root          34       2  0 Apr27 ?        00:00:00 [writeback]
root          35       2  0 Apr27 ?        00:00:04 [kcompactd0]
root          36       2  0 Apr27 ?        00:00:00 [ksmd]
root          37       2  0 Apr27 ?        00:00:02 [khugepaged]
root          38       2  0 Apr27 ?        00:00:00 [cryptd]
root          39       2  0 Apr27 ?        00:00:00 [kintegrityd]
root          40       2  0 Apr27 ?        00:00:00 [kblockd]
root          41       2  0 Apr27 ?        00:00:00 [blkcg_punt_bio]
root          42       2  0 Apr27 ?        00:00:00 [tpm_dev_wq]
root          43       2  0 Apr27 ?        00:00:00 [md]
root          44       2  0 Apr27 ?        00:00:00 [md_bitmap]
root          45       2  0 Apr27 ?        00:00:00 [edac-poller]
root          46       2  0 Apr27 ?        00:00:00 [watchdogd]
root          48       2  0 Apr27 ?        00:00:00 [kworker/1:1H-kblockd]
root          50       2  0 Apr27 ?        00:00:00 [kswapd0]
root          55       2  0 Apr27 ?        00:00:00 [kthrotld]
root          62       2  0 Apr27 ?        00:00:00 [acpi_thermal_pm]
root          63       2  0 Apr27 ?        00:00:00 [xenbus]
root          64       2  0 Apr27 ?        00:00:00 [xenwatch]
root          65       2  0 Apr27 ?        00:00:00 [khvcd]
root          66       2  0 Apr27 ?        00:00:00 [kmpath_rdacd]
root          67       2  0 Apr27 ?        00:00:00 [kaluad]
root          68       2  0 Apr27 ?        00:00:00 [mld]
root          69       2  0 Apr27 ?        00:00:00 [ipv6_addrconf]
root          79       2  0 Apr27 ?        00:00:00 [kstrp]
root          84       2  0 Apr27 ?        00:00:00 [zswap-shrink]
root          85       2  0 Apr27 ?        00:00:00 [kworker/u129:0]
root         188       2  0 Apr27 ?        00:00:00 [kworker/0:1H-kblockd]
root         356       2  0 Apr27 ?        00:00:00 [rpciod]
root         357       2  0 Apr27 ?        00:00:00 [xprtiod]
root         447       2  0 Apr27 ?        00:00:00 [ata_sff]
root         448       2  0 Apr27 ?        00:00:00 [scsi_eh_0]
root         449       2  0 Apr27 ?        00:00:00 [scsi_tmf_0]
root         450       2  0 Apr27 ?        00:00:00 [scsi_eh_1]
root         451       2  0 Apr27 ?        00:00:00 [scsi_tmf_1]
root         487       2  0 Apr27 ?        00:00:00 [kworker/0:2H-kblockd]
root         670       1  0 Apr27 ?        00:00:13 /usr/lib/systemd/systemd-journald
rpc          697       1  0 Apr27 ?        00:00:00 /usr/bin/rpcbind -w -f
root         699       1  0 Apr27 ?        00:00:00 /sbin/auditd
root         728       1  0 Apr27 ?        00:00:00 /usr/lib/systemd/systemd-udevd
dbus         747       1  0 Apr27 ?        00:00:00 /usr/bin/dbus-broker-launch --scope system --audit
dbus         754     747  0 Apr27 ?        00:00:00 dbus-broker --log 4 --controller 9 --machine-id f99ee81f280c48a39afefda470f07f04 --max-bytes 536870912 --max-fds 4096 --max-matc
hes 131072 --audit
root         757       1  0 Apr27 ?        00:00:00 /usr/bin/python3 -s /usr/sbin/firewalld --nofork --nopid
root         759       1  0 Apr27 ?        00:00:01 /usr/lib/systemd/systemd-logind
root         760       1  0 Apr27 ?        00:00:00 /usr/libexec/udisks2/udisksd
polkitd      765       1  0 Apr27 ?        00:00:00 /usr/lib/polkit-1/polkitd --no-debug
root         793       1  0 Apr27 ?        00:01:26 /usr/sbin/NetworkManager --no-daemon
root         824       1  0 Apr27 ?        00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root         826       1  0 Apr27 ?        00:00:03 /warewulf/wwclient
root         828       1  0 Apr27 ?        00:00:00 /usr/sbin/gssproxy -D
root         879       2  0 Apr27 ?        00:00:00 [ttm]
root         899       1  0 Apr27 ?        00:00:16 /usr/sbin/rsyslogd -n
root         911       2  0 Apr27 ?        00:00:03 [kworker/u128:6-rpciod]
root         912       2  0 Apr27 ?        00:00:00 [nfsiod]
root         924       2  0 Apr27 ?        00:00:00 [NFSv4 callback]
root         940       1  0 Apr27 ?        00:16:13 /opt/promtail/promtail-linux-amd64 -config.file=/opt/promtail/promtail-local-config.yaml
root         942       1  0 Apr27 tty1     00:00:00 /sbin/agetty -o -p -- \u --noclear - linux
root         943       1  0 Apr27 hvc0     00:00:00 /sbin/agetty -o -p -- \u --keep-baud 115200,57600,38400,9600 - vt220
root        1062       2  0 Apr27 ?        00:00:06 [kworker/u128:2-events_unbound]
root        1101       2  0 Apr27 ?        00:00:00 [tls-strp]
root        6175       2  0 Apr28 ?        00:00:02 [kworker/u128:0-events_unbound]
root        7069       2  0 08:05 ?        00:00:10 [kworker/1:0-events_freezable_power_]
root        7972     824  0 16:41 ?        00:00:00 sshd: root [priv]
root        7976       1  0 16:41 ?        00:00:00 /usr/lib/systemd/systemd --user
root        7978    7976  0 16:41 ?        00:00:00 (sd-pam)
root        7992    7972  0 16:41 ?        00:00:00 sshd: root@pts/0
root        7993    7992  0 16:41 pts/0    00:00:00 -bash
root        8071       2  0 16:52 ?        00:00:00 [kworker/0:0-events]
root        8104       2  0 17:00 ?        00:00:00 [kworker/0:1-events]
root        8119       2  0 17:04 ?        00:00:00 [kworker/1:1-ata_sff]
root        8127       2  0 17:10 ?        00:00:00 [kworker/1:2-ata_sff]
root        8138    7993  0 17:14 pts/0    00:00:00 ps -ef
root        8139    7993  0 17:14 pts/0    00:00:00 more
[root@rocky19 ~]#
```

c.       ps –ef | wc –l
Output: 
```
[root@rocky19 ~]# ps -ef | wc -l
99
[root@rocky19 ~]#
```
d.       Try to use what you’ve learned to see all the processes owned by your user
Output: 
```
[root@rocky19 ~]# ps -ef | grep root
root           1       0  0 Apr27 ?        00:00:06 /sbin/init
root           2       0  0 Apr27 ?        00:00:00 [kthreadd]
root           3       2  0 Apr27 ?        00:00:00 [rcu_gp]
root           4       2  0 Apr27 ?        00:00:00 [rcu_par_gp]
root           5       2  0 Apr27 ?        00:00:00 [slub_flushwq]
root           6       2  0 Apr27 ?        00:00:00 [netns]
root          10       2  0 Apr27 ?        00:00:00 [mm_percpu_wq]
root          12       2  0 Apr27 ?        00:00:00 [rcu_tasks_kthre]
root          13       2  0 Apr27 ?        00:00:00 [rcu_tasks_rude_]
root          14       2  0 Apr27 ?        00:00:00 [rcu_tasks_trace]
root          15       2  0 Apr27 ?        00:00:00 [ksoftirqd/0]
root          16       2  0 Apr27 ?        00:00:10 [rcu_preempt]
root          17       2  0 Apr27 ?        00:00:01 [migration/0]
root          18       2  0 Apr27 ?        00:00:00 [idle_inject/0]
root          20       2  0 Apr27 ?        00:00:00 [cpuhp/0]
root          21       2  0 Apr27 ?        00:00:00 [cpuhp/1]
root          22       2  0 Apr27 ?        00:00:00 [idle_inject/1]
root          23       2  0 Apr27 ?        00:00:01 [migration/1]
root          24       2  0 Apr27 ?        00:00:00 [ksoftirqd/1]
root          26       2  0 Apr27 ?        00:00:00 [kworker/1:0H-events_highpri]
root          28       2  0 Apr27 ?        00:00:00 [kdevtmpfs]
root          29       2  0 Apr27 ?        00:00:00 [inet_frag_wq]
root          30       2  0 Apr27 ?        00:00:00 [kauditd]
root          32       2  0 Apr27 ?        00:00:00 [khungtaskd]
root          33       2  0 Apr27 ?        00:00:00 [oom_reaper]
root          34       2  0 Apr27 ?        00:00:00 [writeback]
root          35       2  0 Apr27 ?        00:00:04 [kcompactd0]
root          36       2  0 Apr27 ?        00:00:00 [ksmd]
root          37       2  0 Apr27 ?        00:00:02 [khugepaged]
root          38       2  0 Apr27 ?        00:00:00 [cryptd]
root          39       2  0 Apr27 ?        00:00:00 [kintegrityd]
root          40       2  0 Apr27 ?        00:00:00 [kblockd]
root          41       2  0 Apr27 ?        00:00:00 [blkcg_punt_bio]
root          42       2  0 Apr27 ?        00:00:00 [tpm_dev_wq]
root          43       2  0 Apr27 ?        00:00:00 [md]
root          44       2  0 Apr27 ?        00:00:00 [md_bitmap]
root          45       2  0 Apr27 ?        00:00:00 [edac-poller]
root          46       2  0 Apr27 ?        00:00:00 [watchdogd]
root          48       2  0 Apr27 ?        00:00:00 [kworker/1:1H-kblockd]
root          50       2  0 Apr27 ?        00:00:00 [kswapd0]
root          55       2  0 Apr27 ?        00:00:00 [kthrotld]
root          62       2  0 Apr27 ?        00:00:00 [acpi_thermal_pm]
root          63       2  0 Apr27 ?        00:00:00 [xenbus]
root          64       2  0 Apr27 ?        00:00:00 [xenwatch]
root          65       2  0 Apr27 ?        00:00:00 [khvcd]
root          66       2  0 Apr27 ?        00:00:00 [kmpath_rdacd]
root          67       2  0 Apr27 ?        00:00:00 [kaluad]
root          68       2  0 Apr27 ?        00:00:00 [mld]
root          69       2  0 Apr27 ?        00:00:00 [ipv6_addrconf]
root          79       2  0 Apr27 ?        00:00:00 [kstrp]
root          84       2  0 Apr27 ?        00:00:00 [zswap-shrink]
root          85       2  0 Apr27 ?        00:00:00 [kworker/u129:0]
root         188       2  0 Apr27 ?        00:00:00 [kworker/0:1H-kblockd]
root         356       2  0 Apr27 ?        00:00:00 [rpciod]
root         357       2  0 Apr27 ?        00:00:00 [xprtiod]
root         447       2  0 Apr27 ?        00:00:00 [ata_sff]
root         448       2  0 Apr27 ?        00:00:00 [scsi_eh_0]
root         449       2  0 Apr27 ?        00:00:00 [scsi_tmf_0]
root         450       2  0 Apr27 ?        00:00:00 [scsi_eh_1]
root         451       2  0 Apr27 ?        00:00:00 [scsi_tmf_1]
root         487       2  0 Apr27 ?        00:00:00 [kworker/0:2H-kblockd]
root         670       1  0 Apr27 ?        00:00:13 /usr/lib/systemd/systemd-journald
root         699       1  0 Apr27 ?        00:00:00 /sbin/auditd
root         728       1  0 Apr27 ?        00:00:00 /usr/lib/systemd/systemd-udevd
root         757       1  0 Apr27 ?        00:00:00 /usr/bin/python3 -s /usr/sbin/firewalld --nofork --nopid
root         759       1  0 Apr27 ?        00:00:01 /usr/lib/systemd/systemd-logind
root         760       1  0 Apr27 ?        00:00:00 /usr/libexec/udisks2/udisksd
root         793       1  0 Apr27 ?        00:01:26 /usr/sbin/NetworkManager --no-daemon
root         824       1  0 Apr27 ?        00:00:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root         826       1  0 Apr27 ?        00:00:03 /warewulf/wwclient
root         828       1  0 Apr27 ?        00:00:00 /usr/sbin/gssproxy -D
root         879       2  0 Apr27 ?        00:00:00 [ttm]
root         899       1  0 Apr27 ?        00:00:16 /usr/sbin/rsyslogd -n
root         911       2  0 Apr27 ?        00:00:03 [kworker/u128:6-rpciod]
root         912       2  0 Apr27 ?        00:00:00 [nfsiod]
root         924       2  0 Apr27 ?        00:00:00 [NFSv4 callback]
root         940       1  0 Apr27 ?        00:16:13 /opt/promtail/promtail-linux-amd64 -config.file=/opt/promtail/promtail-local-config.yaml
root         942       1  0 Apr27 tty1     00:00:00 /sbin/agetty -o -p -- \u --noclear - linux
root         943       1  0 Apr27 hvc0     00:00:00 /sbin/agetty -o -p -- \u --keep-baud 115200,57600,38400,9600 - vt220
root        1062       2  0 Apr27 ?        00:00:06 [kworker/u128:2-events_unbound]
root        1101       2  0 Apr27 ?        00:00:00 [tls-strp]
root        6175       2  0 Apr28 ?        00:00:02 [kworker/u128:0-events_unbound]
root        7069       2  0 08:05 ?        00:00:10 [kworker/1:0-events_power_efficient]
root        7972     824  0 16:41 ?        00:00:00 sshd: root [priv]
root        7976       1  0 16:41 ?        00:00:00 /usr/lib/systemd/systemd --user
root        7978    7976  0 16:41 ?        00:00:00 (sd-pam)
root        7992    7972  0 16:41 ?        00:00:00 sshd: root@pts/0
root        7993    7992  0 16:41 pts/0    00:00:00 -bash
root        8071       2  0 16:52 ?        00:00:00 [kworker/0:0-events]
root        8104       2  0 17:00 ?        00:00:00 [kworker/0:1-events]
root        8119       2  0 17:04 ?        00:00:00 [kworker/1:1-ata_sff]
root        8127       2  0 17:10 ?        00:00:00 [kworker/1:2-ata_sff]
root        8143    7993  0 17:15 pts/0    00:00:00 ps -ef
root        8144    7993  0 17:15 pts/0    00:00:00 grep --color=auto root
[root@rocky19 ~]#
```

e.       Try to use what you’ve learned to count up all of those processes owned by your user
Output: 
```
[root@rocky19 ~]# ps -ef | grep root | wc -l
95
[root@rocky19 ~]#
```
8.       Looking at system usage (historical)

a.       Check processing for last day

sar | more
Output: 
```
[root@rocky19 ~]# sar | more
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky19)    04/29/25        _x86_64_        (2 CPU)

00:00:04        CPU     %user     %nice   %system   %iowait    %steal     %idle
00:10:04        all      0.05      0.00      0.39      0.08      0.18     99.29
00:20:04        all      0.04      0.00      0.37      0.08      0.18     99.34
00:30:04        all      0.04      0.00      0.37      0.08      0.18     99.33
00:40:04        all      0.04      0.00      0.38      0.08      0.18     99.32
00:50:04        all      0.04      0.00      0.37      0.08      0.18     99.33
01:00:04        all      0.04      0.00      0.37      0.08      0.18     99.33
01:10:04        all      0.04      0.05      0.39      0.07      0.18     99.27
01:20:04        all      0.04      0.00      0.37      0.08      0.18     99.33
01:30:04        all      0.04      0.00      0.37      0.08      0.18     99.33
01:40:04        all      0.04      0.00      0.37      0.08      0.19     99.32
01:50:04        all      0.04      0.00      0.37      0.07      0.18     99.34
02:00:04        all      0.04      0.00      0.37      0.07      0.18     99.33
02:10:04        all      0.04      0.00      0.38      0.08      0.18     99.32
02:20:04        all      0.04      0.00      0.37      0.08      0.18     99.33
02:30:04        all      0.04      0.00      0.37      0.07      0.18     99.34
02:40:04        all      0.04      0.00      0.37      0.08      0.18     99.32
02:50:04        all      0.04      0.00      0.37      0.07      0.18     99.33
03:00:04        all      0.04      0.21      0.41      0.08      0.18     99.09
03:10:04        all      0.04      0.00      0.37      0.07      0.18     99.33
03:20:04        all      0.04      0.00      0.38      0.07      0.18     99.33
03:30:04        all      0.04      0.00      0.37      0.08      0.18     99.34
03:40:04        all      0.04      0.00      0.37      0.07      0.18     99.33
03:50:04        all      0.04      0.00      0.37      0.08      0.18     99.33
04:00:04        all      0.04      0.00      0.37      0.08      0.18     99.33
04:10:04        all      0.04      0.05      0.39      0.08      0.18     99.27
04:20:04        all      0.04      0.00      0.37      0.07      0.18     99.33
04:30:04        all      0.04      0.00      0.38      0.07      0.18     99.33
04:40:04        all      0.04      0.00      0.37      0.07      0.18     99.34
04:50:04        all      0.04      0.00      0.38      0.08      0.18     99.32
05:00:04        all      0.04      0.00      0.38      0.08      0.18     99.32
05:10:04        all      0.04      0.00      0.38      0.07      0.18     99.33
05:20:04        all      0.04      0.00      0.38      0.08      0.18     99.33
05:30:04        all      0.04      0.00      0.37      0.07      0.18     99.34
05:40:04        all      0.04      0.05      0.37      0.07      0.17     99.30
05:50:04        all      0.04      0.00      0.37      0.07      0.18     99.34
06:00:04        all      0.04      0.00      0.37      0.07      0.18     99.33
06:10:04        all      0.04      0.00      0.37      0.07      0.18     99.34
06:20:04        all      0.04      0.00      0.37      0.07      0.18     99.33
06:30:04        all      0.04      0.00      0.37      0.07      0.18     99.33
06:40:04        all      0.04      0.00      0.37      0.08      0.18     99.33
06:50:04        all      0.04      0.00      0.37      0.07      0.18     99.34
07:00:04        all      0.04      0.79      0.46      0.07      0.18     98.45
07:10:04        all      0.04      0.00      0.37      0.07      0.18     99.33
07:20:04        all      0.04      0.00      0.37      0.08      0.18     99.33
07:30:04        all      0.04      0.00      0.38      0.08      0.18     99.32
07:40:04        all      0.04      0.00      0.37      0.08      0.17     99.34
07:50:04        all      0.04      0.00      0.37      0.08      0.18     99.34
08:00:04        all      0.04      0.00      0.38      0.07      0.18     99.33
08:10:04        all      0.04      0.00      0.38      0.07      0.18     99.33
08:20:04        all      0.04      0.00      0.37      0.08      0.18     99.33
08:30:04        all      0.04      0.00      0.37      0.07      0.18     99.34
08:40:04        all      0.04      0.00      0.36      0.07      0.18     99.35
08:50:04        all      0.04      0.00      0.37      0.07      0.18     99.34
09:00:04        all      0.05      0.05      0.39      0.07      0.18     99.27
09:10:04        all      0.04      0.00      0.37      0.07      0.17     99.35
09:20:04        all      0.04      0.00      0.37      0.07      0.18     99.33
09:30:04        all      0.04      0.00      0.37      0.07      0.18     99.33
09:40:04        all      0.04      0.00      0.37      0.07      0.18     99.32
09:50:04        all      0.04      0.00      0.38      0.08      0.18     99.33
10:00:04        all      0.04      0.00      0.37      0.07      0.18     99.34
10:10:04        all      0.04      0.00      0.37      0.07      0.18     99.33
10:20:04        all      0.04      0.00      0.37      0.07      0.18     99.33
10:30:00        all      0.05      0.25      0.41      0.07      0.18     99.04
10:40:04        all      0.04      0.00      0.37      0.07      0.18     99.34
10:50:04        all      0.04      0.00      0.37      0.07      0.18     99.33
11:00:04        all      0.04      0.00      0.38      0.08      0.18     99.33
11:10:04        all      0.04      0.00      0.37      0.07      0.18     99.34
11:20:04        all      0.04      0.00      0.37      0.07      0.18     99.33
11:30:04        all      0.04      0.00      0.38      0.08      0.18     99.33
11:40:04        all      0.04      0.00      0.37      0.07      0.18     99.34
11:50:04        all      0.04      0.00      0.37      0.08      0.18     99.33
12:00:04        all      0.04      0.00      0.38      0.08      0.18     99.33
12:10:04        all      0.04      0.00      0.37      0.08      0.18     99.34
12:20:04        all      0.04      0.00      0.37      0.07      0.18     99.33
12:30:04        all      0.05      0.05      0.38      0.08      0.18     99.28
12:40:04        all      0.04      0.00      0.37      0.07      0.18     99.34
12:50:04        all      0.04      0.00      0.36      0.07      0.18     99.35
13:00:04        all      0.04      0.00      0.37      0.07      0.18     99.34
13:10:04        all      0.04      0.00      0.37      0.08      0.18     99.33
13:20:04        all      0.04      0.00      0.37      0.07      0.18     99.35
13:30:04        all      0.04      0.00      0.36      0.07      0.17     99.36
13:40:04        all      0.04      0.00      0.38      0.08      0.19     99.32
13:50:04        all      0.04      0.00      0.37      0.07      0.18     99.34
14:00:04        all      0.04      0.00      0.37      0.08      0.18     99.33
14:10:04        all      0.04      0.00      0.38      0.07      0.18     99.33
14:20:04        all      0.04      0.00      0.37      0.07      0.18     99.33
14:30:00        all      0.04      0.25      0.42      0.08      0.18     99.02
14:40:04        all      0.04      0.00      0.37      0.07      0.18     99.34
14:50:04        all      0.04      0.00      0.37      0.07      0.18     99.33
15:00:04        all      0.05      0.00      0.37      0.07      0.17     99.34
15:10:04        all      0.04      0.00      0.36      0.07      0.18     99.34
15:20:04        all      0.04      0.00      0.37      0.08      0.18     99.33
15:30:04        all      0.04      0.00      0.37      0.08      0.18     99.33
15:40:04        all      0.04      0.00      0.38      0.08      0.18     99.32
15:50:04        all      0.04      0.00      0.37      0.07      0.18     99.34
16:00:04        all      0.04      0.00      0.38      0.08      0.19     99.32
16:10:04        all      0.04      0.05      0.39      0.08      0.18     99.27
16:20:04        all      0.04      0.00      0.37      0.08      0.18     99.33
16:30:04        all      0.04      0.00      0.37      0.08      0.18     99.34
16:40:04        all      0.04      0.00      0.36      0.07      0.18     99.35
16:50:44        all      0.09      0.00      0.41      0.07      0.17     99.25
17:00:44        all      0.05      0.00      0.39      0.08      0.18     99.30
17:10:04        all      0.04      0.00      0.38      0.08      0.18     99.32
Average:        all      0.04      0.02      0.38      0.07      0.18     99.31
[root@rocky19 ~]#
```

b.       Check memory for the last day

sar –r | more
Output: 
```
[root@rocky19 ~]# sar -r | more
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky19)    04/29/25        _x86_64_        (2 CPU)

00:00:04    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
00:10:04      1447684   1387692    223232      5.60        16   2140244   2585120     64.86   2231696        64         0
00:20:04      1449252   1389260    221660      5.56        16   2140244   2585120     64.86   2231696        64         0
00:30:04      1448612   1388608    222320      5.58        16   2140248   2585124     64.86   2231800        64         0
00:40:04      1448528   1388524    222404      5.58        16   2140248   2585124     64.86   2231800        64         0
00:50:04      1448528   1388524    222400      5.58        16   2140252   2585128     64.86   2231804        64         0
01:00:04      1448448   1388444    222476      5.58        16   2140256   2585132     64.86   2231808        64         0
01:10:04      1450400   1390396    220508      5.53        16   2140256   2585132     64.86   2231808        64         0
01:20:04      1450196   1390192    220708      5.54        16   2140260   2585136     64.86   2231844        64         0
01:30:04      1450084   1390080    220820      5.54        16   2140260   2585136     64.86   2231812        64         0
01:40:04      1449668   1389664    221236      5.55        16   2140260   2585136     64.86   2231820        64         0
01:50:04      1449612   1389608    221284      5.55        16   2140264   2585140     64.87   2231824        64         0
02:00:04      1449388   1389384    221500      5.56        16   2140268   2585144     64.87   2231828        64         0
02:10:04      1449168   1389164    221712      5.56        16   2140276   2585152     64.87   2231836        64         0
02:20:04      1450548   1390544    220332      5.53        16   2140276   2585152     64.87   2231836        64         0
02:30:04      1449876   1389872    221004      5.55        16   2140276   2585152     64.87   2231736        64         0
02:40:04      1449700   1389696    221176      5.55        16   2140280   2585156     64.87   2231840        64         0
02:50:04      1448860   1388856    221980      5.57        16   2140280   2607404     65.42   2232140        64         0
03:00:04      1475028   1415024    195772      4.91        16   2140284   2585160     64.87   2231844        64         0
03:10:04      1470056   1410052    200756      5.04        16   2140288   2585164     64.87   2231848        64         0
03:20:04      1469972   1409968    200824      5.04        16   2140292   2585168     64.87   2231852        64         0
03:30:04      1469972   1409968    200816      5.04        16   2140296   2585172     64.87   2231904        64         0
03:40:04      1471024   1411020    199756      5.01        16   2140300   2585176     64.87   2231908        64         0
03:50:04      1470608   1410604    200172      5.02        16   2140300   2585176     64.87   2231836        64         0
04:00:04      1470188   1410184    200588      5.03        16   2140304   2585180     64.87   2231916        64         0
04:10:04      1452644   1392640    218124      5.47        16   2140304   2585180     64.87   2231916        64         0
04:20:04      1452312   1392312    218416      5.48        16   2140304   2585180     64.87   2231852        64         0
04:30:04      1452120   1392112    218636      5.49        16   2140312   2585188     64.87   2231860        64         0
04:40:04      1452960   1392952    217796      5.46        16   2140312   2585188     64.87   2231860        64         0
04:50:04      1452768   1392760    217984      5.47        16   2140312   2585188     64.87   2231924        64         0
05:00:04      1452320   1392312    218428      5.48        16   2140316   2585192     64.87   2231928        64         0
05:10:04      1452128   1392120    218620      5.49        16   2140316   2585192     64.87   2231828        64         0
05:20:04      1453348   1393340    217396      5.45        16   2140320   2585196     64.87   2231936        64         0
05:30:04      1452792   1392784    217924      5.47        16   2140320   2585196     64.87   2231936        64         0
05:40:04      1452828   1392824    217876      5.47        16   2140328   2585204     64.87   2231944        64         0
05:50:04      1452496   1392492    218208      5.48        16   2140332   2585208     64.87   2231948        64         0
06:00:04      1452356   1392352    218356      5.48        16   2140332   2585208     64.87   2231948        64         0
06:10:04      1452136   1392132    218576      5.48        16   2140332   2585208     64.87   2231948        64         0
06:20:04      1452108   1392104    218600      5.49        16   2140336   2585212     64.87   2231952        64         0
06:30:04      1451776   1391776    218928      5.49        16   2140336   2585212     64.87   2231952        64         0
06:40:04      1451776   1391776    218920      5.49        16   2140340   2585216     64.87   2231956        64         0
06:50:04      1451720   1391720    218972      5.49        16   2140344   2585220     64.87   2231960        64         0
07:00:04      1496264   1436264    174272      4.37        16   2140356   2585232     64.87   2231972        64         0
07:10:04      1495588   1435588    174988      4.39        16   2140360   2585236     64.87   2231976        64         0
07:20:04      1495252   1435252    175288      4.40        16   2140360   2585236     64.87   2231812        64         0
07:30:04      1494864   1434864    175680      4.41        16   2140360   2585236     64.87   2231912        64         0
07:40:04      1494836   1434836    175704      4.41        16   2140364   2585240     64.87   2231916        64         0
07:50:04      1494836   1434836    175704      4.41        16   2140364   2585240     64.87   2231948        64         0
08:00:04      1494836   1434836    175708      4.41        16   2140372   2585248     64.87   2231824        64         0
08:10:04      1494836   1434836    175708      4.41        16   2140372   2585248     64.87   2231824        64         0
08:20:04      1496344   1436344    174200      4.37        16   2140372   2585248     64.87   2231832        64         0
08:30:04      1495304   1435304    175236      4.40        16   2140376   2585252     64.87   2231928        64         0
08:40:04      1495252   1435252    175256      4.40        16   2140376   2585252     64.87   2231828        64         0
08:50:04      1495252   1435252    175268      4.40        16   2140376   2585252     64.87   2231928        64         0
09:00:04      1483252   1423248    187224      4.70        16   2140388   2585264     64.87   2231940        64         0
09:10:04      1483004   1423000    187484      4.70        16   2140388   2585264     64.87   2231940        64         0
09:20:04      1482668   1422664    187824      4.71        16   2140388   2585264     64.87   2231872        64         0
09:30:04      1482532   1422528    187956      4.72        16   2140392   2585268     64.87   2231944        64         0
09:40:04      1482504   1422500    187984      4.72        16   2140392   2585268     64.87   2231944        64         0
09:50:04      1482336   1422332    188148      4.72        16   2140396   2585272     64.87   2231948        64         0
10:00:04      1482112   1422108    188372      4.73        16   2140396   2585272     64.87   2231948        64         0
10:10:04      1482384   1422380    188096      4.72        16   2140400   2585276     64.87   2231952        64         0
10:20:04      1482328   1422324    188116      4.72        16   2140404   2585280     64.87   2231956        64         0
10:30:00      1464384   1404380    205956      5.17        16   2140428   2585288     64.87   2232020        64         0
10:40:04      1464240   1404236    206164      5.17        16   2140416   2585292     64.87   2231968        64         0
10:50:04      1464188   1404184    206216      5.17        16   2140420   2585296     64.87   2231972        64         0
11:00:04      1463992   1403988    206412      5.18        16   2140420   2585296     64.87   2231972        64         0
11:10:04      1463656   1403652    206744      5.19        16   2140424   2585300     64.87   2231976        64         0
11:20:04      1464944   1404940    205452      5.16        16   2140428   2585304     64.87   2231980        64         0
11:30:04      1464504   1404500    205856      5.17        16   2140428   2585304     64.87   2231980        64         0
11:40:04      1464476   1404472    205880      5.17        16   2140432   2585308     64.87   2231884        64         0
11:50:04      1464368   1404364    205988      5.17        16   2140432   2585308     64.87   2231984        64         0
12:00:04      1464284   1404280    206072      5.17        16   2140432   2585308     64.87   2231884        64         0
12:10:04      1465236   1405232    205116      5.15        16   2140436   2585312     64.87   2231988        64         0
12:20:04      1464760   1404756    205584      5.16        16   2140436   2585312     64.87   2231988        64         0
12:30:04      1464752   1404748    205568      5.16        16   2140448   2585324     64.87   2232080        64         0
12:40:04      1464556   1404552    205736      5.16        16   2140448   2585324     64.87   2232000        64         0
12:50:04      1463884   1403884    206408      5.18        16   2140448   2585324     64.87   2232000        64         0
13:00:04      1464888   1404888    205400      5.15        16   2140452   2585328     64.87   2231904        64         0
13:10:04      1464860   1404856    205436      5.15        16   2140452   2585328     64.87   2232004        64         0
13:20:04      1464412   1404408    205884      5.17        16   2140452   2585328     64.87   2232004        64         0
13:30:04      1464160   1404156    206128      5.17        16   2140456   2585332     64.87   2232008        64         0
13:40:04      1465064   1405060    205220      5.15        16   2140460   2585336     64.87   2232020        64         0
13:50:04      1464672   1404668    205580      5.16        16   2140460   2585336     64.87   2232020        64         0
14:00:04      1464616   1404612    205648      5.16        16   2140464   2585340     64.87   2232024        64         0
14:10:04      1464308   1404304    205956      5.17        16   2140464   2585340     64.87   2232048        64         0
14:20:04      1463948   1403944    206312      5.18        16   2140468   2585344     64.87   2232028        64         0
14:30:00      1501828   1441824    168372      4.22        16   2140476   2585352     64.87   2232092        64         0
14:40:04      1497568   1437564    172624      4.33        16   2140476   2585352     64.87   2232036        64         0
14:50:04      1497484   1437480    172708      4.33        16   2140484   2585360     64.87   2232044        64         0
15:00:04      1497428   1437424    172764      4.33        16   2140484   2585360     64.87   2232048        64         0
15:10:04      1497404   1437400    172788      4.34        16   2140484   2585360     64.87   2231948        64         0
15:20:04      1497404   1437400    172784      4.34        16   2140488   2585364     64.87   2231952        64         0
15:30:04      1497380   1437376    172804      4.34        16   2140488   2585364     64.87   2232052        64         0
15:40:04      1498140   1438136    172040      4.32        16   2140492   2585368     64.87   2232056        64         0
15:50:04      1497380   1437376    172800      4.34        16   2140492   2585368     64.87   2232056        64         0
16:00:04      1496904   1436900    173244      4.35        16   2140492   2585368     64.87   2232056        64         0
16:10:04      1489988   1429984    180132      4.52        16   2140504   2585380     64.87   2232072        64         0
16:20:04      1489680   1429676    180452      4.53        16   2140504   2585380     64.87   2232072        64         0
16:30:04      1489912   1429908    180220      4.52        16   2140504   2585380     64.87   2232072        64         0
16:40:04      1489824   1429820    180304      4.52        16   2140508   2585384     64.87   2232084        64         0
16:50:44      1477612   1417636    191712      4.81        16   2140536   2616756     65.66   2240020        64         0
17:00:44      1477360   1417392    191956      4.82        16   2140540   2616760     65.66   2240048        64         0
17:10:04      1477120   1417208    192064      4.82        16   2140544   2616764     65.66   2240108        64         0
Average:      1470196   1410194    200310      5.03        16   2140379   2586384     64.90   2232169        64         0
[root@rocky19 ~]#
```

Sar is a tool that shows the 10 minute weighted average of the system for the last day. Sar is tremendously useful for showing long periods of activity and system load. It is exactly the opposite in it’s usefulness of spikes or high traffic loads. In a 20 minute period of 100% usage a system may just show to averages times of 50% and 50%, never actually giving accurate info. Problems typically need to be proactively tracked by other means, or with scripts, as we will see below.

Sar can also be run interactively. Run the command `yum whatprovides sar` and you will see that it is sysstat. You may have guessed that sar runs almost exactly like iostat.

```
[root@rocky19 ~]# yum whatprovides sar
Last metadata expiration check: 2:48:20 ago on Tue Apr 29 14:29:59 2025.
sysstat-12.5.4-9.el9.x86_64 : Collection of performance monitoring tools for Linux
Repo        : @System
Matched from:
Filename    : /usr/bin/sar

sysstat-12.5.4-9.el9.x86_64 : Collection of performance monitoring tools for Linux
Repo        : appstream
Matched from:
Filename    : /usr/bin/sar

[root@rocky19 ~]#
```

Try the same commands from earlier, but with their interactive information

sar 2       #crtl + c to break
Output: 
```
[root@rocky19 ~]# sar 2
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky19)    04/29/25        _x86_64_        (2 CPU)

17:18:53        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:18:55        all      0.00      0.00      0.75      0.00      0.25     99.00
17:18:57        all      0.00      0.00      0.50      0.00      0.25     99.25
17:18:59        all      0.00      0.00      0.25      0.25      0.00     99.50
17:19:01        all      0.00      0.00      0.25      0.00      0.50     99.25
17:19:03        all      0.00      0.00      0.50      0.00      0.00     99.50
^C
Average:        all      0.00      0.00      0.45      0.05      0.20     99.30
[root@rocky19 ~]#
```
sar 2 5
Output: 
```
[root@rocky19 ~]# sar 2 5
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky19)    04/29/25        _x86_64_        (2 CPU)

17:19:18        CPU     %user     %nice   %system   %iowait    %steal     %idle
17:19:20        all      0.00      0.00      0.50      0.00      0.25     99.25
17:19:22        all      0.00      0.00      0.25      0.25      0.25     99.25
17:19:24        all      0.00      0.00      0.25      0.00      0.25     99.50
17:19:26        all      0.00      0.00      0.25      0.00      0.25     99.50
17:19:28        all      0.00      0.00      0.50      0.25      0.00     99.25
Average:        all      0.00      0.00      0.35      0.10      0.20     99.35
[root@rocky19 ~]#
```
or

sar –r 2
Output: 
```
[root@rocky19 ~]# sar -r 2
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky19)    04/29/25        _x86_64_        (2 CPU)

17:19:47    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
17:19:49      1475604   1415736    193448      4.85        16   2140548   2617184     65.67   2240352        64         0
17:19:51      1475604   1415736    193448      4.85        16   2140548   2617184     65.67   2240352        64         0
17:19:53      1475604   1415736    193448      4.85        16   2140548   2617184     65.67   2240352        64         0
17:19:55      1475604   1415736    193448      4.85        16   2140548   2617184     65.67   2240380        64         0
17:19:57      1475604   1415736    193448      4.85        16   2140548   2617184     65.67   2240412        64         0
^C
Average:      1475604   1415736    193448      4.85        16   2140548   2617184     65.67   2240370        64         0
[root@rocky19 ~]#
```

sar –r 2 5
Output: 
```
[root@rocky19 ~]# sar -r 2 5
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky19)    04/29/25        _x86_64_        (2 CPU)

17:20:12    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
17:20:14      1475604   1415740    193436      4.85        16   2140556   2617192     65.67   2240364        64         0
17:20:16      1475604   1415740    193436      4.85        16   2140556   2617192     65.67   2240364        64         0
17:20:18      1475604   1415740    193436      4.85        16   2140556   2617192     65.67   2240420        64         0
17:20:20      1475604   1415740    193436      4.85        16   2140556   2617192     65.67   2240420        64         0
17:20:22      1475604   1415740    193436      4.85        16   2140556   2617192     65.67   2240420        64         0
Average:      1475604   1415740    193436      4.85        16   2140556   2617192     65.67   2240398        64         0
[root@rocky19 ~]#
```

c.       Check sar logs for previous daily usage

cd /var/log/sa/

ls
Output: 
```
[root@rocky19 ~]# cd /var/log/sa/
[root@rocky19 sa]# ls
sa27  sa28  sa29  sar27  sar28
[root@rocky19 sa]#

sa01  sa02  sa03  sa04  sa05  sar01  sar02  sar03  sar04
```
sar –f sa03 | head
Output: 
```
[root@rocky19 sa]# sar -f sa28 | head
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky19)    04/28/25        _x86_64_        (2 CPU)

00:00:04        CPU     %user     %nice   %system   %iowait    %steal     %idle
00:10:04        all      0.05      0.00      0.38      0.08      0.18     99.31
00:20:04        all      0.04      0.00      0.36      0.07      0.18     99.35
00:30:04        all      0.04      0.00      0.37      0.08      0.18     99.33
00:40:04        all      0.04      0.00      0.37      0.08      0.18     99.34
00:50:04        all      0.04      0.00      0.37      0.07      0.18     99.34
01:00:04        all      0.04      0.00      0.37      0.08      0.18     99.34
01:10:04        all      0.04      0.00      0.37      0.08      0.18     99.33
[root@rocky19 sa]#
```


sar –r –f sa03 | head
Output: 
```
[root@rocky19 sa]# sar -r -f sa28 | head
Linux 5.14.0-427.42.1.el9_4.x86_64 (rocky19)    04/28/25        _x86_64_        (2 CPU)

00:00:04    kbmemfree   kbavail kbmemused  %memused kbbuffers  kbcached  kbcommit   %commit  kbactive   kbinact   kbdirty
00:10:04      1462756   1402692    210544      5.28        16   2139300   2584252     64.84   2230820        64         0
00:20:04      1462672   1402608    210628      5.28        16   2139300   2584252     64.84   2230820        64         0
00:30:04      1462672   1402608    210620      5.28        16   2139304   2584256     64.84   2230748        64         0
00:40:04      1462308   1402244    210980      5.29        16   2139308   2584260     64.84   2230764        64         0
00:50:04      1463604   1403540    209676      5.26        16   2139316   2584268     64.84   2230772        64         0
01:00:04      1463160   1403096    210116      5.27        16   2139316   2584268     64.84   2230772        64         0
01:10:04      1462544   1402480    210732      5.29        16   2139316   2584268     64.84   2230772        64         0
[root@rocky19 sa]#
```

#should output just the beginning of 3 July (whatever month you’re in). Most Sar data is kept for just one month but is very configurable. Read man sar for more info.

d.       Sar logs are not kept in a readable format, they are binary. So if you needed to dump all the sar logs from a server, you’d have to output it to a file that is readable. You could do something like this:

                                                               i.      Gather information and move to the right location

cd /var/log/sa

pwd

ls
Output: 
```
[root@rocky19 sa]# pwd
/var/log/sa
[root@rocky19 sa]# ls
sa27  sa28  sa29  sar27  sar28
[root@rocky19 sa]#
```


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


------------------------------------------------------------------------------------------------------------------------------

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
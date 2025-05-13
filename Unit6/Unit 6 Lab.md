[![](https://github.com/ProfessionalLinuxUsersGroup/img/raw/main/Assets/Logos/ProLUG_Round_Transparent_LOGO.png?raw=true)](https://github.com/ProfessionalLinuxUsersGroup/img/blob/main/Assets/Logos/ProLUG_Round_Transparent_LOGO.png?raw=true)

# Unit 6 Lab - Firewalls

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#unit-6-lab---firewalls)

> If you are unable to finish the lab in the ProLUG lab environment we ask you `reboot` the machine from the command line so that other students will have the intended environment.

### Resources / Important Links

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#resources--important-links)

- [Killercoda Labs](https://killercoda.com/learn)
- [Firewalld Official Documentation](https://firewalld.org/documentation/)
- [RedHat Firewalld Documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_firewalls_and_packet_filters/using-and-configuring-firewalld_firewall-packet-filters)

### Required Materials

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#required-materials)

- Rocky 9.4+ - ProLUG Lab
    - Or comparable Linux box
- root or sudo command access

#### Downloads

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#downloads)

The lab has been provided for convenience below:

- [📥 u6_lab(`.pdf`)](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/assets/downloads/u6/u6_lab.pdf)
- [📥 u6_lab(`.docx`)](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/assets/downloads/u6/u6_lab.docx)

## Pre-Lab Warm-Up

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#pre-lab-warm-up)

---

Exercises (Warmup to quickly run through your system and practice commands)

1. `cd~`
2. `pwd (should be /home/<yourusername>)`
3. `cd /tmp`
4. `pwd (should be /tmp)`
5. `cd`
6. `pwd (should be /home/<yourusername>)`
7. `mkdir lab_firewalld`
8. `cd lab_firewalld`
9. `touch testfile1`
10. `ls`
11. `touch testfile{2..10}`
12. `ls`
13. `seq 10`
14. `seq 1 10`
15. `seq 1 2 10`
    - man seq and see what each of those values mean. It’s important to know the behavior if you intend to ever use the command, as we often do with counting (for) loops.

No worries, there are two ways to fix the mess you've made. Nothing you've done is permanent, so logging out and reloading a shell (logging back in) would fix this.  
We just put the aliases back.

16. `for i in` seq 1 10`; do touch file$i; done;`
17. `ls`
    - Think about some of those commands and when you might use them. Try to change command #15 to remove all of those files (rm -rf file$i)

## Lab 🧪

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#lab-)

---

This lab is designed to help you get familiar with the basics of the systems you will be working on.

Some of you will find that you know the basic material but the techniques here allow you to put it together in a more complex fashion.

It is recommended that you type these commands and do not copy and paste them. Browsers sometimes like to format characters in a way that doesn't always play nice with Linux.

#### Check Firewall Status and settings:

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#check-firewall-status-and-settings)

A very important thing to note before starting this lab. You’re connected into that server on ssh via port 22. If you do anything to lockout **port 22** in this lab, you will be blocked from that connection and we’ll have to reset it.

1. Check firewall status
    
    ```shell
    [root@schampine ~]# systemctl status firewalld
    ```
    
    Example Output:
    
    ```shell
    firewalld.service - firewalld - dynamic firewall daemon
    Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
    Active: inactive (dead) since Sat 2017-01-21 19:27:10 MST; 2 weeks 6 days ago
     Main PID: 722 (code=exited, status=0/SUCCESS)
    
    Jan 21 19:18:11 schampine firewalld[722]: 2017-01-21 19:18:11 ERROR: COMMAND....
    Jan 21 19:18:13 schampine firewalld[722]: 2017-01-21 19:18:13 ERROR: COMMAND....
    Jan 21 19:18:13 schampine firewalld[722]: 2017-01-21 19:18:13 ERROR: COMMAND....
    Jan 21 19:18:13 schampine firewalld[722]: 2017-01-21 19:18:13 ERROR: COMMAND....
    Jan 21 19:18:13 schampine firewalld[722]: 2017-01-21 19:18:13 ERROR: COMMAND....
    Jan 21 19:18:14 schampine firewalld[722]: 2017-01-21 19:18:14 ERROR: COMMAND....
    Jan 21 19:18:14 schampine firewalld[722]: 2017-01-21 19:18:14 ERROR: COMMAND....
    Jan 21 19:18:14 schampine firewalld[722]: 2017-01-21 19:18:14 ERROR: COMMAND....
    Jan 21 19:27:08 schampine systemd[1]: Stopping firewalld - dynamic firewall.....
    Jan 21 19:27:10 schampine systemd[1]: Stopped firewalld - dynamic firewall ...n.
    ```
    
    **Hint:** Some lines were ellipsized, use -l to show in full.
    

#### If necessary start the firewalld daemon:

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#if-necessary-start-the-firewalld-daemon)

```shell
systemctl start firewalld
```

Output: 

```shell
[root@rocky15 ~]# systemctl start firewalld
[root@rocky15 ~]#

```
#### Set the firewalld daemon to be persistent through reboots:

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#set-the-firewalld-daemon-to-be-persistent-through-reboots)

```shell
systemctl enable firewalld
```

Output: 

```shell
[root@rocky15 ~]# systemctl enable firewalld
[root@rocky15 ~]#

```
Verify with systemctl status firewalld again from **step 1**

#### Check which zones exist:

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#check-which-zones-exist)

```shell
firewall-cmd --get-zones
```


Output: 

```shell
[root@rocky15 ~]# firewall-cmd --get-zones
block dmz drop external home internal nm-shared public trusted work
[root@rocky15 ~]#

```
#### Checking the values within each zone:

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#checking-the-values-within-each-zone)

```shell
firewall-cmd --list-all --zone=public
```


Output: 

```shell
[root@rocky15 ~]# firewall-cmd --list-all --zone=public
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 9100/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
[root@rocky15 ~]#


```

**General Output**

```shell
public (default, active)
interfaces: wlp4s0
sources:
services: dhcpv6-client ssh
ports:
masquerade: no
forward-ports:
icmp-blocks:
rich rules:
```

#### Checking the active and default zones:

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#checking-the-active-and-default-zones)

```shell
firewall-cmd --get-default
```


Output: 

```shell
[root@rocky15 ~]# firewall-cmd --get-default
public
[root@rocky15 ~]#

```
Example Output:

```shell
public
```

Next Command

```shell
firewall-cmd --get-active
```


Output: 

```shell
[root@rocky15 ~]# firewall-cmd --get-active-zones
public
  interfaces: eth0
[root@rocky15 ~]#

```

Example Output:

```shell
public
interfaces: wlp4s0
```

**Note:** this also shows which interface the zone is applied to. Multiple interfaces and zones can be applied

So now you know how to see the values in your firewall. Use steps 4 and 5 to check all the values of the different zones to see how they differ.

#### Set the firewall active and default zones:

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#set-the-firewall-active-and-default-zones)

We know the zones from above, set your firewall to the different active or default zones. Default zones are the ones that will come up when the firewall is restarted.

**Note:** It may be useful to perform an `ifconfig -a` and note your interfaces for the next part

```shell
ifconfig -a | grep -i flags
```


Output: 

```shell
[root@rocky15 ~]# ifconfig -a | grep -i flags
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
[root@rocky15 ~]#

```

Example Output:

```shell
[root@rocky ~]# ifconfig -a | grep -i flags
docker0: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
ens32: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
```

1. Changing the default zones (This is permanent over a reboot, other commands require --permanent switch)

```shell
firewall-cmd --set-default-zone=work
```


Output: 

```shell
[root@rocky15 ~]# firewall-cmd --set-default-zone=work
success
[root@rocky15 ~]#

```

Example Output:

```shell
success
```

Next Command:

```shell
firewall-cmd --get-active-zones
```


Output: 

```shell

[root@rocky15 ~]# firewall-cmd --get-active-zones
work
  interfaces: eth0
[root@rocky15 ~]#

```
Example Output:

```shell
work
    interfaces: wlp4s0
```

Attempt to set it back to the original public zone and verify. Set it to one other zone, verify, then set it back to public.

Output: 

```shell
[root@rocky15 ~]# firewall-cmd --set-default-zone=public
success
[root@rocky15 ~]#
[root@rocky15 ~]# firewall-cmd --get-active-zones
public
  interfaces: eth0
[root@rocky15 ~]#

```

Changing interfaces and assigning different zones (use another interface from your earlier `ifconfig -a`

```shell
firewall-cmd --change-interface=virbr0 --zone dmz
```


Output: 

```shell
[root@rocky15 ~]# firewall-cmd --change-interface=virbr0 --zone dmz
success
[root@rocky15 ~]#
```

Example Output:

```shell
success
```

Next Command:

```shell
firewall-cmd --add-source 192.168.11.0/24 --zone=public
```


Output: 

```shell
[root@rocky15 ~]# firewall-cmd --add-source 192.168.11.0/24 --zone=public
success
[root@rocky15 ~]#

```

Example Output:

```shell
success
```

Next Command:

```shell
firewall-cmd --get-active-zones
```


Output:
```shell
[root@rocky15 ~]# firewall-cmd --get-active-zones
dmz
  interfaces: virbr0
public
  interfaces: eth0
  sources: 192.168.11.0/24
[root@rocky15 ~]#

```

Example Output:

```shell
dmz
   interfaces: virbr0
work
   interfaces: wlp4s0
public
   sources: 192.168.200.0/24
```

#### Working with ports and services:

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6lab.md#working-with-ports-and-services)

We can be even more granular with our ports and services. We can block or allow services by port number, or we can assign port numbers to a service name and then block or allow those service names.

1. List all services assigned in firewalld
    
    ```shell
    firewall-cmd --get-services
    ```
    
    Example Output:
    
    ```shell
    RH-Satellite-6 amanda-client bacula bacula-client dhcp dhcpv6 dhcpv6-client dns freeipa-ldap freeipa-ldaps freeipa-replication ftp high-availability http https imaps ipp ipp-client ipsec iscsi-target kerberos kpasswd ldap ldaps libvirt libvirt-tls mdns mountd ms-wbt mysql nfs ntp openvpn pmcd pmproxy pmwebapi pmwebapis pop3s postgresql proxy-dhcp radius rpc-bind rsyncd samba samba-client smtp ssh telnet tftp tftp-client transmission-client vdsm vnc-server wbem-https
    ```
    
    This next part is just to show you where the service definitions exist. They are simple xml format and can easily be manipulated or changed to make new services. This would require a restart of the firewalld service to re-read this directory.
    
    Next Command:
    
    ```shell
    ls /usr/lib/firewalld/services/
    ```
    
    Example Output:
    
    ```shell
    amanda-client.xml        iscsi-target.xml  pop3s.xml
    bacula-client.xml        kerberos.xml      postgresql.xml
    bacula.xml               kpasswd.xml       proxy-dhcp.xml
    dhcpv6-client.xml        ldaps.xml         radius.xml
    dhcpv6.xml               ldap.xml          RH-Satellite-6.xml
    dhcp.xml                 libvirt-tls.xml   rpc-bind.xml
    dns.xml                  libvirt.xml       rsyncd.xml
    freeipa-ldaps.xml        mdns.xml          samba-client.xml
    freeipa-ldap.xml         mountd.xml        samba.xml
    freeipa-replication.xml  ms-wbt.xml        smtp.xml
    ftp.xml                  mysql.xml         ssh.xml
    high-availability.xml    nfs.xml           telnet.xml
    https.xml                ntp.xml           tftp-client.xml
    http.xml                 openvpn.xml       tftp.xml
    imaps.xml                pmcd.xml          transmission-client.xml
    ipp-client.xml           pmproxy.xml       vdsm.xml
    ipp.xml                  pmwebapis.xml     vnc-server.xml
    ipsec.xml                pmwebapi.xml      wbem-https.xml
    ```
    
    Next Command:
    
    ```shell
    cat /usr/lib/firewalld/services/http.xml
    ```
    
    Example Output:
    
    ```shell
    <?xml version="1.0" encoding="utf-8"?>
    <service>
      <short>WWW (HTTP)</short>
      <description>HTTP is the protocol used to serve Web pages. If you plan to make your Web       server publicly available, enable this option. This option is not required for viewing pages locally or developing Web pages.</description>
      <port protocol="tcp" port="80"/>
    </service>
    ```
    
2. Adding a service or port to a zone
    
    Ensuring we are working on a public zone
    
    ```shell
    firewall-cmd --set-default-zone=public
    ```
    
    Example Output:
    
    ```shell
    success
    ```
    
    Listing Services
    
    ```shell
    firewall-cmd --list-services
    ```
    
    Example Ouput:
    
    ```shell
    dhcpv6-client ssh
    ```
    
    **Note:** We have 2 services
    
    Permanently adding a service with the `--permanent` switch
    
    ```shell
    firewall-cmd --permanent --add-service ftp
    ```
    
    Example Output:
    
    ```shell
    success
    ```
    
    Reloading
    
    ```shell
    firewall-cmd --reload
    ```
    
    Example Output:
    
    ```shell
    success
    ```
    
    Verifying we are in the correct **Zone**
    
    ```shell
    firewall-cmd --get-default-zone
    ```
    
    Example Output:
    
    ```shell
    public
    ```
    
    Verifying that we have successfully added the **FTP** service
    
    ```shell
    firewall-cmd --list-services
    ```
    
    Example Output:
    
    ```shell
    dhcpv6-client ftp ssh
    ```
    
    Alternatively, we can do almost the same thing but not use a defined service name. If I just want to allow port 1147 through for TCP traffic, it is very simple as well.
    
    ```shell
    firewall-cmd --permanent --add-port=1147/tcp
    ```
    
    Example Output:
    
    ```shell
    success
    ```
    
    Reloading once again
    
    ```shell
    firewall-cmd --reload
    ```
    
    Example Output:
    
    ```shell
    success
    ```
    
    Listing open ports now
    
    ```shell
    [root@schampine services]# firewall-cmd --list-ports
    ```
    
    Example Output:
    
    ```shell
    1147/tcp
    ```
    
3. Removing unwanted services or ports
    
    To remove those values and permanently fix the configuration back we simply use remove.
    
    Firstly, we will permanently remove ftp service
    
    ```shell
    firewall-cmd --permanent --remove-service=ftp
    ```
    
    Example Output:
    
    ```shell
    success
    ```
    
    Then we will permanently remove the ports
    
    ```shell
    firewall-cmd --permanent --remove-port=1147/tcp
    ```
    
    Example Output:
    
    ```shell
    success
    ```
    
    Now lets do a reload
    
    ```shell
    firewall-cmd --reload
    ```
    
    Example Output:
    
    ```shell
    success
    ```
    
    Now we can list services again to confirm our work
    
    ```shell
    firewall-cmd --list-services
    ```
    
    Example Output:
    
    ```shell
    dhcpv6-client ssh
    ```
    
    Now we can list ports
    
    ```shell
    firewall-cmd --list-ports
    ```
    
    Example Output:
    
    `Nothing`
    
    Before making any more changes I recommend running the `list` commands above with `>> /tmp/firewall.orig` on them so you have all your original values saved somewhere in case you need them.
    

So now take this and set up some firewalls on the interfaces of your system. Change the default ports and services assigned to your different zones (at least 3 zones) Read the `man firewall-cmd` command or `firewall-cmd -help` to see if there are any other userful things you should know.

> Be sure to `reboot` the lab machine from the command line when you are done.
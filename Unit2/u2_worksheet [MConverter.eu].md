**ProLUG 101**

**Unit 2 Worksheet**

Instructions

Fill out this sheet as you progress through the lab and discussions.
Hold onto all of your work to send to me at the end of the course.

Discussion Questions:

**[Unit 1 Discussion Post 1]{.underline}**: Think about how week 1 went
for you.

- Do you get everything you needed to done?

- Do you need to allocate more time to the course, and if so, how do you
  > plan to do it?

- How well did you take notes during the lecture? Do you need to improve
  > this?

**[Unit 1 Discussion Post 2:]{.underline}**

> Read a blog, check a search engine, or ask an AI about SELINUX. What
> is the significance of contexts? What are the significance of labels?
>
> You follow your company instructions to add a new user to a set of 10
> Linux servers. They cannot access just one (1) of the servers. When
> you review the differences in the servers you see that the server they
> cannot access is running SELINUX. On checking other users have no
> problem getting into the system. You find nothing in the documentation
> (typical) about this different system or how these users are accessing
> it. What do you do? Where do you check? (you may use any online
> resources to help you answer this. This is not a trick and it is not a
> "one answer solution". This is for you to think through.)

Definitions/Terminology

Uptime -- A metric that allows us to show how long any one system has
been running or how much time has passed since last reboot.

Standard input -- Data stream that accepts text as standard input.

Standard output -- Text output from the command to the shell is
delivered via the stdout stream.

Standard error -- Error messages are sent through the stderr data
stream.

Mandatory Access Control -- In relation to SELinux, Mandatory Access
Control policies allow you to define a security policy that provides
granular permissions for users, programs, processes, files, and devices.

Discretionary Access Control -- Discretionary Access Control policies,
provide minimal protection from broken software or from malware running
as a normal user or as root. Access is soley based on user identity and
ownership.

Security contexts (SELINUX) -- Processes and files are labled with an
SELinux context additional information. This could include SELinux user,
role, type, and a level. All of this ties into SELinux because it helps
assist in the ability to make access control decisions.

SELINUX operating modes -- SELinux can run in three modes:

Disabled: Avoids enforcing SELinux Policies that are in place on the
system.

Permissive: Acts as though SELinux is enforcing the policies, however it
is not denying any operations.

Enforcing: Enforces the SELinux policies and acts normally.

Notes During Lecture/Class:

Links:

<https://www.howtogeek.com/435903/what-are-stdin-stdout-and-stderr-on-linux/>

<https://docs.oracle.com/en/operating-systems/oracle-linux/6/porting/section_jsf_zpm_wm.html#:~:text=The%20SELinux%20enhancement%20to%20the,processes%2C%20files%2C%20and%20devices>.

<https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/chap-security-enhanced_linux-selinux_contexts#chap-Security-Enhanced_Linux-SELinux_Contexts>

<https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/sect-security-enhanced_linux-introduction-selinux_modes#sect-Security-Enhanced_Linux-Introduction-SELinux_Modes>

Terms:

Useful tools:

Lab and Assignment

Unit2_ProLUG_LabEssentials2 - To be completed outside of lecture time

Compare SELINUX to Apparmor, following the below 2 tasks:

- Read this article:
  > [[https://www.redhat.com/sysadmin/apparmor-selinux-isolation]{.underline}](https://www.redhat.com/sysadmin/apparmor-selinux-isolation)

- Do this lab:
  > [[https://killercoda.com/killer-shell-cks/scenario/apparmor]{.underline}](https://killercoda.com/killer-shell-cks/scenario/apparmor)

Start thinking about your project ideas (more to come in future weeks):

Topics:

- System Stability

- System Performance

- System Security

- System monitoring

- Kubernetes

- Programming/Automation

> You will research, design, deploy, and document a system that improves
> your administration of Linux systems in some way.

Digging Deeper

- How does troubleshooting differ between system administration and
  > system engineering? To clarify, how might you troubleshoot
  > differently if you know a system was running v. if you're building a
  > new system out?

<!-- -->

- Investigate a troubleshooting methodology, by either google or AI
  > search. Does the methodology fit for you in an IT sense, why or why
  > not?

Reflection Questions

- What questions do you still have about this week?

<!-- -->

- How are you going to use what you've learned in your current role?

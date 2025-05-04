[![](https://github.com/ProfessionalLinuxUsersGroup/img/raw/main/Assets/Logos/ProLUG_Round_Transparent_LOGO.png?raw=true)](https://github.com/ProfessionalLinuxUsersGroup/img/blob/main/Assets/Logos/ProLUG_Round_Transparent_LOGO.png?raw=true)

# Unit 6 Worksheet - Firewalls

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6ws.md#unit-6-worksheet---firewalls)

## Instructions

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6ws.md#instructions)

---

Fill out the worksheet as you progress through the lab and discussions. Hold your worksheets until the end to turn them in as a final submission packet.

### Resources / Important Links

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6ws.md#resources--important-links)

- [Official Firewalld Documentation](https://firewalld.org/documentation/)
- [RedHat Firewalld Documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_firewalls_and_packet_filters/using-and-configuring-firewalld_firewall-packet-filters)
- [Official UFW Documentation](https://help.ubuntu.com/community/UFW)
- [Wikipedia entry for Next-Gen Firewalls](https://en.wikipedia.org/wiki/Next-generation_firewall)

#### Downloads

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6ws.md#downloads)

The worksheet has been provided below. The document(s) can be transposed to the desired format so long as the content is preserved. For example, the `.txt` could be transposed to a `.md` file.

- [📥 u6_worksheet(`.txt`)](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/assets/downloads/u6/u6_worksheet.txt)
- [📥 u6_worksheet(`.docx`)](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/assets/downloads/u6/u6_worksheet.docx)

### Unit 6 Recording

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6ws.md#unit-6-recording)

<iframe style="width: 100%; height: 100%; border: none; aspect-ratio: 16/9; border-radius: 1rem; background:black" src="[https://www.youtube.com/embed/wCVj3qeLTMg?si=ozEReY_17DOyjUAv](https://www.youtube.com/embed/wCVj3qeLTMg?si=ozEReY_17DOyjUAv)" title="Unit 6 Recording - ProLUG Linux Systems Administration Course" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen> </iframe>

#### Discussion Post #1

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6ws.md#discussion-post-1)

Scenario:

> A ticket has come in from an application team. Some of the servers your team built for them last week have not been reporting up to enterprise monitoring and they need it to be able to troubleshoot a current issue, but they have no data. You jump on the new servers and find that your engineer built everything correctly and the agents for node_exporter, ceph_exporter and logstash exporter that your teams use. But, they also have adhered to the new company standard of firewalld must be running. No one has documented the ports that need to be open, so you’re stuck between the new standards and fixing this problem on live systems.

Next, answer these questions here:

1. As you’re looking this up, what terms and concepts are new to you?
    
2. What are the ports that you need to expose? How did you find the answer?
    
3. What are you going to do to fix this on your firewall?
    

#### Discussion Post #2

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6ws.md#discussion-post-2)

Scenario:

> A manager heard you were the one that saved the new application by fixing the firewall. They get your manager to approach you with a request to review some documentation from a vendor that is pushing them hard to run a WAF in front of their web application. You are “the firewall” guy now, and they’re asking you to give them a review of the differences between the firewalls you set up (which they think should be enough to protect them) and what a WAF is doing.

1. What do you know about the differences now?
    
2. What are you going to do to figure out more?
    
3. Prepare a report for them comparing it to the firewall you did in the first discussion.
    

Submit your input by following the link below.

The discussion posts are done in Discord threads. Click the 'Threads' icon on the top right and search for the discussion post.

- [Link to Discussion Posts](https://discord.com/channels/611027490848374811/1365776270800977962)

## Definitions

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6ws.md#definitions)

---

Firewall:

Zone:

Service:

DMZ:

Proxy:

Stateful packet filtering:

Stateless packet filtering:

WAF:

NGFW:

## Digging Deeper

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6ws.md#digging-deeper)

---

1. Read [https://docs.rockylinux.org/zh/guides/security/firewalld-beginners/](https://docs.rockylinux.org/zh/guides/security/firewalld-beginners/)  
    What new things did you learn that you didn’t learn in the lab?  
    What functionality of firewalld are you likely to use in your professional work?

## Reflection Questions

[](https://github.com/ProfessionalLinuxUsersGroup/lac/blob/main/src/u6ws.md#reflection-questions)

---

1. What questions do you still have about this week?
2. How are you going to use what you’ve learned in your current role?
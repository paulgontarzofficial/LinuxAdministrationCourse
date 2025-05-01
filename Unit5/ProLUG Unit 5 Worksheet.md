# Unit 5 Worksheet 

## Instructions
- Fill out this sheet as you progress through the lab and discussions. Hold onto all of your work to send to me at the end of the course.

## Discussion Questions: 

Unit 5 Discussion Post 1**: Review the page: [https://attack.mitre.org/](https://attack.mitre.org/)

1. What terms and concepts are new to you?
		```- Discovery: In the context of the MITRE ATT&CK Knowledge Base, Discovery is the act of the advisary trying to gain more information and map out the network to increase attack surface.```
		```- Command and Control: I personally have heard about Command and Control multiple times, however I have never seen it be done in practice. The definition is an advesary is trying to communicate with compromised systems with the intent to control them. Some adversaries may use Application Layer Protocols, Content Injection, Data Encoding, etc. ```
		```MITRE ATT&CK: Globally accessible knowledge base that allows for a centralized location for all the tactics that are used by the advesaries.   
2. Why, as a system administrator and not directly in security, do you think it’s so important to understand how your systems can be attacked? Isn’t it someone else’s problem to think about that?
		```- On the Job Role, yes, technically it is someone else's job, if a security engineer is also working on the system. I have always heard that "security is everyone's responsibility." If security is everyone's responsibility, then you are inherently goin to be more protected the more eyes you put on a system. This allows for tighter security postures and overall knowledge for the team.  ```
3. What impact to the organization is data exfiltration? Even if you’re not a data owner or data custodian, why is it so important to understand the data on your systems?

**Unit 5 Discussion Post 2:** Find a blog or article on the web that discusses the user environment in Linux. You may want to search for .bashrc or (dot) environment files in Linux.

1. What types of customizations might you setup for your environment? Why?
		```- Some customizations I may add on my .bashrc file would be Alias', these can be super useful for commands that are being used consistantly. 
		-You can also use it to create functions, then call those functions once written to achieve a task.
		For example: 
		Let's say I want to make an alias for the command
		ls -la. WE can set that alias using the following line in our .bashrc
		alias la='ls -la', if we type "la" into the terminal, we will get their output as ls -la.
		-Functions can be used for more complex code when alias' will not work. 
		 ``` 

2. What problems can you anticipate around helping users with their dot files?
		```- The main problem that I can see is that not all Linux distros are created equal. Some Linux distros may use programs that aren't installed on others.
		- Some dotfiles have specific configurations that are tailored to personal preference which could lead to conflicts.
		- Management of your dotfiles can become tedious and overwhelming. ```

## Definition / Terminology

Footprinting
	```- Footprinting is a technique that is utilized to gather as much data as possible on a target to identify vulnerabilities or wekanesses in a system.```

Scanning
	```- Technique that is utilized to scan a system or network for active vulnerabilities. Utilizes the CVE Score to determine whether the vulnerability is scored from Low - Critical. ``` 

Enumeration
	`- When an adversary is enumerating your network, this means that the adversary is "walking" your network or other means of gathering as much information about your company that could lead to a breach.`  

System Hacking
	`- Using technical skills and knowledge to gain access to a system or a network. `

Escalation of Privilege
	` - Also known as Privesc, is a concept within Ethical Hacking that allows an adversary, after gaining initial access, to escalate privileges to an admin/root account.`  

- Rule of least privilege
		`- This rule states that the users should only have access to files that are needed for required daily tasks. 

Covering Tracks
	- `Covering Tracks involves clearing out evidence that may lead to what exactly happened and how you gain access. `

Planting Backdoors
	- `Backdoors allow an adversary, after initial compromise, to remote back into the system at their leisure utilizing the backdoor that was planted on the system. ` 
## Notes During Lecture/Class 

Links:
[Covering Tracks in Ethical Hacking | Useful Codes](https://useful.codes/covering-tracks-in-ethical-hacking/#:~:text=Covering%20tracks%2C%20a%20critical%20phase%20in%20ethical%20hacking%2C,systems%20while%20maintaining%20confidentiality%20and%20preventing%20unintended%20disruptions.)
[What is System Hacking in Ethical Hacking? Types and Process Explained](https://www.eccouncil.org/cybersecurity-exchange/ethical-hacking/system-hacking-definition-types-processes/)
[What is footprinting in ethical hacking?](https://www.techtarget.com/searchsecurity/definition/footprinting)


Terms:

Useful tools:

## Lab and Assignment

Unit 5 Manage Users and Groups - To be completed outside of lecture time

	Map the Internal ProLUG Network (192.168.200.0/24):

1. Map the network from one of the rocky nodes.

Using a template that you build or find from the internet, provide a 1 page summary of what you find in the network.

![[Pasted image 20250430151155.png]]

Begin working on your project from the Project Guide

Topics:

1.      System Stability

2.      System Performance

3.      System Security

4.      System monitoring

5.      Kubernetes

6.      Programming/Automation

You will research, design, deploy, and document a system that improves your administration of Linux systems in some way.
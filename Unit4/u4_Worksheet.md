
**ProLUG 101**

**Unit 4 Worksheet**

# Instructions

Fill out this sheet as you progress through the lab and discussions. Hold onto all of your work to send to me at the end of the course.

# Discussion Questions:

**Unit 4 Discussion Post 1**: Read this article: [https://cio-wiki.org/wiki/Operations_Bridge](https://cio-wiki.org/wiki/Operations_Bridge)

1. What terms and concepts are new to you?
	- Operations Bridge, Root Cause Analysis, Key Performance Indicators. I have seen a majority of the components and features on a network, however never really had the time nor the know how to really get in there and investigate. Also, I work with a lot of windows servers, so things like SCOM and SCCM I am familiar with, not so much any Linux Operations Bridge. 

2. Which pro seems the most important to you? Why?
	- I believe that the Real-time visibility into IT infrastructure and services pro is the most important. This is the most crucial part of having an accurate view in real time with your network, if you don't have real-time visibility you aren't getting the most up to date information, and the incident may have occurred a lot earlier than what is actually being displayed.   

3. Which con seems the most costly, or difficult to overcome to you? Why?
	- Coming from a network that I have had countless issues on, still do, I believe integration with multiple systems and data sources is the most difficult. Especially when it comes to integrating different OS types, this can become a headache with keeping track of everything you need to integrate from one to another. 

**Unit 4 Discussion Post 2:** Your team has no documentation around how to check out a server during an incident. Write out a procedure of what you think an operations person should be doing on the system they suspect is not working properly. This may help, to get you started ([https://zeltser.com/media/docs/security-incident-survey-cheat-sheet.pdf?msc=Cheat+Sheet+Blog](https://zeltser.com/media/docs/security-incident-survey-cheat-sheet.pdf?msc=Cheat+Sheet+Blog)) You may use AI for this, but let us know if you do.
### Step 1: Initial Verification

- **Confirm Incident Report:**
    
    - Verify incident details from monitoring tools, alerts, or user reports.
        
    - Note the time incident was reported.
        

### Step 2: Access Server Safely

- **Access Method:**
    
    - Use secure methods such as SSH/RDP or console access.
        
    - Record timestamp of server access.
        
- **Authentication:**
    
    - Log in using individual admin credentials (do not use generic or shared accounts).
        

### Step 3: Initial Health Check

- **System Resource Check:**
    
    - Run `top`, `htop`, or Task Manager to verify CPU, RAM, Disk, and Network usage.
        
    - Identify processes consuming excessive resources.
        
- **Disk Space Check:**
    
    - Use commands such as `df -h` to verify disk usage.
        

### Step 4: Logs Inspection

- **System Logs:**
    
    - Check system logs (`/var/log/syslog`, `/var/log/messages`).
        
    - Identify errors or anomalies correlating with reported issues.
        
- **Application Logs:**
    
    - Inspect specific application logs for any unusual events or errors.
        

### Step 5: Security Assessment

- **Check for Unauthorized Access:**
    
    - Run `last`, `who`, and audit logs to detect unauthorized login attempts or activities.
        
    - Verify integrity of important system files using tools like Tripwire or AIDE.
        
- **Malware Scan:**
    
    - Conduct a quick scan using antivirus or antimalware software installed.
        

### Step 6: Network Checks

- **Connectivity:**
    
    - Ping critical services and external IPs to confirm network connectivity.
        
    - Verify DNS resolution (`nslookup`, `dig`).
        
- **Open Ports and Connections:**
    
    - Run `netstat -tulnp` or `ss -tulnp` to check active network connections and services.
        

### Step 7: Document Findings

- **Incident Documentation:**
    
    - Record detailed notes, commands executed, findings, and timestamps.
        
    - Note any immediate actions taken.
        

### Step 8: Mitigation Steps

- **Corrective Action:**
    
    - Restart affected services if applicable.
        
    - Adjust resource limits, free up disk space, or isolate the server if security compromise is suspected.
        
- **Escalation:**
    
    - Escalate to senior operations personnel or cybersecurity team if significant security issues are discovered.
        

### Step 9: Verify Resolution

- **Validation Check:**
    
    - Confirm the server's stability and performance after corrective actions.
        
    - Verify resolution with initial complainants or via monitoring tools.
        

### Step 10: Reporting and Follow-up

- **Incident Closure:**
    
    - Update incident tracking system with full details.
        
    - Conduct a post-incident review if necessary to improve future response.
        

---

# Definitions/Terminology

Detection: Identifying potential incidents that may have occurred through monitoring systems, monitoring logs, or utilizing intrusion detection systems. 

Response: Your initial handling of the incident in an effort to remediate the incident at hand. 

Mitigation: Measures that are taken to fix and prevent that incident from reoccurring. 

Reporting: 

Recovery

Remediation

Lessons Learned

After action review

Operations Bridge

# Notes During Lecture/Class:

Links:
https://www.techtarget.com/searchsecurity/definition/incident-response


Terms:

Useful tools:

# Lab and Assignment

Unit4_Lab_Operate_Running_Systems - To be completed outside of lecture time

                Begin working on your project from the Project Guide

                                Topics:

1.      System Stability

2.      System Performance

3.      System Security

4.      System monitoring

5.      Kubernetes

6.      Programming/Automation

You will research, design, deploy, and document a system that improves your administration of Linux systems in some way.

# Digging Deeper

1.      Read about battle drills here ([https://en.wikipedia.org/wiki/Battle_drill](https://en.wikipedia.org/wiki/Battle_drill))

Why might it be important to practice incident handling before an incident occurs?

Why might it be important to understand your tools before an incident occurs?

# Reflection Questions

1.      What questions do you still have about this week?

2.      How much better has your note taken gotten since you started? What do you still need to work on? Have you started using a different tool? Have you taken more notes?
**ProLUG 101**

**Unit 3 Worksheet**

Instructions

Fill out this sheet as you progress through the lab and discussions.
Hold onto all of your work to send to me at the end of the course.

Discussion Questions:

**[Unit 3 Discussion Post 1]{.underline}**: What does the term triage
mean to you? Have you ever had to triage something?

- Triage, in my terms involves an initial situation where there is
  something out of the ordinary and you need to use the knowledge that
  you have to have to make a best educated case on what possibilities
  could have caused this and what the consequences of such situation
  could be.

- Yes, all the time, I currently work as a System Administrator and
  there are days where multiple servers are having issues just due to
  infrastructure limitations. With someone constantly breathing down my
  neck, they demand an answer on what is going on, what is causing it,
  the fix, and what services are not available currently.

- Good concepts were outlined in the good practices section of the
  Operations Security section. This included meetings in-person out of
  view of the attacker, key-based access methods, be explicit in the
  information that you are giving others as some info may be
  confidential, consider what an attacker would do if they saw
  tightening of group policies for example. They may assume that they
  have been comprised.

**[Unit 3 Discussion Post 2:]{.underline}**

> Ask google, find a blog, or ask an AI about high availability. (Here's
> one if you need it:
> [[https://docs.aws.amazon.com/pdfs/whitepapers/latest/real-time-communication-on-aws/real-time-communication-on-aws.pdf#high-availability-and-scalability-on-aws]{.underline}](https://docs.aws.amazon.com/pdfs/whitepapers/latest/real-time-communication-on-aws/real-time-communication-on-aws.pdf))

- What are some important terms you read about?

  1.  \- Redundancy, multiples instances of critical systems to
      > eliminate a single level of failure.

      > \- Failover Mechanisms, Automatic switching to a standby system.

      > \- Load Balancing, distributing workloads across multiple
      > systems to optimize resources.

      > \- Monitoring and Health Checks, continuous monitoring of the
      > network and services to detect failures and trigger recovery
      > procedures.

- Why do you think understanding HA will help you better in the context
  > of triaging incidents?

  1.  \- Understanding HA can assist you with mitigating the downtime
      > for the end user due to traffic being re-routed to the healthy
      > nodes while you work on the faulty node. Also, monitoring allows
      > an administrator to view potential causes, the time, and other
      > useful information that may lead to repairs of the server. This
      > relieves stress on the admin also due to the fact that we have
      > the redundant healthy nodes taking traffic while it gives us
      > time to troubleshoot the issue without the burden of having the
      > end-user unable to access the resources they need.

Definitions/Terminology

Five 9's -- This term refers to the amount of time a service or IT Component is online and usable. The five 9's comes from 99.999% availability annually. This means that on average, you are looking to be unavailable for no more than 5.256 Minutes/yr. 

Single point of failure -- A component that, when goes down or is inoperable, takes out the entire system without any backup solution. 

Key Performance Indicators -- These are measurable values that demonstrate how effectively an organization or group is achieving their objective

SLI -- Service Level Indicator, these are the numbers that are achieved that coincide with your Service Level Objectives.

SLO -- Service Level Objectives, located within a Service Level Agreement, are your promises to what you are providing. 

SLA -- Service Level Agreement, is an agreement between a provider and a client that outlines uptime and responsibility.  

Active-Standby -- Active-Standby involves one active system that is handling all the load and one system that is on standby in case something goes wrong with the primary node. 

Active-Active -- Active-Active Failover utilizes systems that are running concurrently handling traffic at the same time. Both are fully operational, and if one goes down, then you have your second node to pick up the load.

MTTD -- Mean Time to Detect is a measure of time it takes for an organization to identify and issue or incident. 

MTTR -- Mean Time to Repair is a measure of time it takes to repair the system after initial incident. 

MTBF -- Mean Time Between Failure is a measure of time it takes for a system to fail while operating. 

Notes During Lecture/Class:

Links:

- https://www.splunk.com/en_us/blog/learn/five-nines-availability.html
- https://www.relianoid.com/resources/knowledge-base/misc/understanding-active-active-and-active-standby-fail-over/?srsltid=AfmBOophx2R1uImfJUIaaD3uDKzTWWR0r30KJ2H3SUSLsZRCntO4cy1e
- 

Terms:

*triage*---using the knowledge and information available to them to make
educated and informed assumptions about the severity and potential
consequences of the incident.

Useful tools:

Lab and Assignment

Unit3_ProLUG_LVM_and_RAID - To be completed outside of lecture time

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

- If uptime is so important to us, why is it so important to us to also
  > understand how our systems can fail? Why would we focus on the thing
  > that does not drive uptime?

<!-- -->

- Start reading about SLOs:
  > [[https://sre.google/workbook/implementing-slos/]{.underline}](https://sre.google/workbook/implementing-slos/)

> How does this help you operationally? Does it make sense that keeping
> systems within defined parameters will help keep them operating
> longer?

Reflection Questions

- What questions do you still have about this week?

<!-- -->

- How are you going to use what you've learned in your current role?

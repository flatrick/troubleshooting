# Introduction

The main goal of this repository is to provide a source for guides on how to perform troubleshooting, first and foremost for those of you who are trying to figure out what's wrong with a piece of software you don't have the source-code for.  

Some of the things I aim to explain will however be useful tools for developers as well, sometimes a developers idea of what their program is doing might be far from the truth, most oftenly because their assumptions on how the other objects the application works with behave.

# Scope

Even though I feel most at home in a UNIX-environment, my professional work life has primarily had me work with Windows-environments for the past 10 years.  
Therefor this guide will try to be a helping hand for all of you Windows-technicians out there!  
I've primarily worked with troubleshooting .Net, Visual FoxPro and Visual Basic.Net applications, but anything that runs on Windows will be covered here, some in more detail than others based on my own experiences.  
  
If **you** have some tips, guides or ideas for me to dive deeper into, **please**, leave a Issue and let's see what we can create together.

# Aspects and the related tools

I will try to create a fairly generic overview of the most common aspects of an application you might need to troubleshoot and what tools you'd use to accomplish it with.

| Aspect      | Details       | Tools |
| ----------  |:-------------:| -----:|
| App         | Stack-traces, trace-profiles  | Process Monitor - Process Explorer - Task Manager - PerfView - VMMap  |
| CPU         | Performance-capability, resource-usage, (app)context  | Process Monitor - Process Explorer - PerfMon  |
| RAM         | Availability, resource-usage  | Process Monitor - Process Explorer - PerfMon  |
| Registry    | Create, Read, Update, Delete and missing posts  | Process Monitor - RegScanner - RegEdit  |
| Logs        | Logs from both the OS and applications  | eventvwr.msc - snaketail-net - glogg/klogg - Notepad++  |
| Storage     | Utilization, read/write responsetime and bandwidth  | Process Monitor - PerfMon - ATTO Disk Benchmark - IOMeter  |
| Shares      | Utilization, read/write responsetime and bandwidth  |  Process Monitor - fsmgmt.msc - PerfMon - ATTO Disk Benchmark - IOMeter  |
| Network     | Reachable, Utilization, bandwidth, response time, successful delivery  | Process Monitor - Process Explorer - Wireshark - SmartSniff  |
| Webservices | Reachable, responses, response time  | Charles Proxy, Fiddler, Wireshark    |

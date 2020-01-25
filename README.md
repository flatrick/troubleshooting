# Introduction

The main goal of this repository is to provide a source for guides on how to perform troubleshooting, first and foremost for those of you who are trying to figure out what's wrong with a piece of software you don't have the source-code for.  

Some of the things I aim to explain will however be useful tools for developers as well, sometimes a developers idea of what their program is doing might be far from the truth, most oftenly because their assumptions on how the other objects the application works with behave are incorrect.

# Scope

Even though I feel most at home in a UNIX-environment, my professional work life has primarily had me work with Windows-environments for the past 10 years.  
Therefore this guide will try to be a helping hand for all of you Windows-technicians out there!  
I've primarily worked with troubleshooting .Net, Visual FoxPro and Visual Basic.Net applications, but anything that runs on Windows will be covered here, some in more detail than others based on my own experiences.  
  
_If **you** have some tips, guides or ideas for me to dive deeper into, **please**, leave a Issue and let's see what we can create together._

# Aspects to troubleshoot

I will try to create a fairly generic overview of the most common aspects of an application you might need to troubleshoot and what tools you'd use to accomplish it with.
As I finish writing examples on **how** to troubleshoot using a specific tool, I will link to that guide in this list.

## Application

When we're troubleshooting a specific application, there are plenty of things to look at, and it can be quite overwhelming when you're new to the job.
Broadly speaking, there are three things you'll be asked to troubleshoot:

* Application crashes
* Unexpected/Incorrect behaviour
* Bad performance

But to troubleshoot these things, we're going to need to look at quite a few things (see sections below for examples on what and how).
For now, I'll focus on anything directly tied to the application itself.

### Step 1 in all troubleshooting

Always talk to the end-user(s) who experienced the issue, have them describe it in as many details as they can and try to make them reproduce the error while you're watching (either by actually standing next to them or using a remote-session tool such as **Teamviewer**).  
Using a tool such as **Snagit** for screen-recording will be useful if you need to show a developer exactly what happens from the second a user starts the application until the error occurs.  
**Remember this: any detail given by the end-user must be considered unreliable until you can reproduce the error yourself**.  
If/when you can reproduce the error on a separate computer of your own, you're most certainly working with something where it might be worth involving the developers of the software itself, if possible. But there are still things we can do before attempting to get a developer to assist us!

### Misbehaving application

If you're not experiencing application crashes, hopefully the application will have stored information about something going wrong in one of its own logs so it's time to start going through these.
Remember to always keep track of the clock when the error(s) occurs, this will be vital when we're going through the log-files to be able to identify things that happened before the error, during the error and just after the error.
If the application stores its logs in plain text-files, you might want to use tools such as **Notepad++**, **glogg/klogg** or similiar; these can be configured to help you more easily spot error/warning/info/debug messages or help you clean a very cluttered log-file.

If the logs don't contain any valuable information (shame on you developer(s) who aren't properly logging stuff!), it's time to use tools that monitor what our application is doing.

### Application crashes

**Application crashes** will leave clues as to why they crashed, but depending on the technology used, our methods will differ.
For **.Net** applications, Windows will store the exception(s) in **Event Viewer** _(in the Application-log)_.
Depending on how the application is written, the exception(s) containing the actual root cause might have been captured in the code and _**hopefully**_ been stored in a log for the application.

For **non-.Net** applications, Windows will create a (small) memory-dump that can be viewed with tools such as **AppCrashView** or **WinDbg**, parts of that information will be visible in **Event Viewer** _(in the Application-log)_.
Sometimes, that smaller memory-dump won't be enough, but then you can configure Windows to give you full memory-dumps.  
But beware, when we're starting to dig into memory dumps, it will start to get really difficult, even for a lot of developers. But I will mention it since it's important to know what you can do to continue attempting to find the cause of an issue.  
**Take note that these full memory dumps can, _and probably will_, contain sensitive information about the user and/or anything the user was looking at, so handle that dump with utmost care and don't spread it online.**

If the logs or memory-dumps aren't really giving you any good clues as to what happened, it's time to start pulling out some debugging tools.

### Tools for when the logs aren't enough

I will start by saying this: I'm not exaggerating when I say that the vast majority of issues I've troubleshooted, I've done using this single tool.  
Performance, misbehaving or full application crashes can all be troubleshooted using Windows SysInternals Process Monitor. Sounds too good to be true?  
Well, then you're in for a wild ride as you start going through the guides I'll share here and start to see for yourself just how much can be analyzed and identified using this tool.  

* Windows SysInternals - Process Monitor
* Windows SysInternals - Process Explorer
* Windows SysInternals - VMMap
* NirSoft - WinCrashReport
* NirSoft - HeapMemView
* NirSoft - DeviceIOView
* PerfMon.msc
* Eventvwr.msc

## Operating System

* BlueScreenView _(this is if the entire O/S crashes while using the application in question)_

### Registry

* Windows SysInternals - Process Monitor
* NirSoft - Regscanner
* NirSoft - RegFromApp

### Network

* Windows SysInternals - Process Monitor
* Windows SysInternals - Process Explorer
* NirSoft - SmartSniff
* PerfMon.msc
* ping
* tracert
* PortQry
* netstat

### Storage - File System (FS)

There are a few details that are very specific for storage accessed over the network (as in SMB Shares) so I will address those where it's appropriate.
But here are some of the tools you'll be using to troubleshoot anything regarding working with files:

* Windows SysInternals - Process Monitor
* Windows SysInternals - Process Explorer
* NirSoft - ProcessActivityView
* PerfMon.msc

#### Local Storage

#### Network Storage

## External services

### Depedencies

### Database

#### Microsoft SQL Server

### Webservice

REST-API:s, SOAP-API:s, GrapQL-API:s and countless alternatives to come.
For applications to handle the vast amount of users, plenty choose to split into multiple services to be able to split the load over multiple computers.
This creates the need for a way to communicate, and some of the ones that are heavily used today are known as REST, SOAP and GraphQL.  

For a support-technician, it can be vital to know **what** they are and **why** they're being used, even more so, we need to know **how** they're being used so we know **when** they're not doing what they're expected to.
But we also need to know **how** to see what's going on and that part I can help you with, the others is up to you to investigate.

# MOVE ME INTO CATEGORIES ABOVE

| Aspect      | Details       | Tools |
| ----------  |:-------------:| -----:|
| Application | Stack-traces, trace-profiles, loaded DLLs, files locked/processed, (app)context | Process Monitor - Process Explorer - PerfView - VMMap  |
| CPU         | Performance-capability, resource-usage, (app)context  | Process Monitor - Process Explorer - PerfMon  |
| RAM         | Free memory, memory-usage  | Process Monitor - Process Explorer - PerfMon - VMMap  |
| Registry    | Create, Read, Update, Delete and missing posts  | Process Monitor - RegScanner - RegEdit  |
| Logs        | Logs from both the OS and applications  | eventvwr.msc - snaketail-net - glogg/klogg - Notepad++  |
| Storage     | Utilization, read/write responsetime and bandwidth  | Process Monitor - PerfMon - ATTO Disk Benchmark - IOMeter  |
| Shares      | Utilization, read/write responsetime and bandwidth  |  Process Monitor Keep track of how well we reach our goals so we can adjust our plans and/or goals (increase/decrease the expected weight lost per X amount of days OR make further adjustments to what we eat)- fsmgmt.msc - PerfMon - ATTO Disk Benchmark - IOMeter  |
| Network     | Reachable, Utilization, bandwidth, response time, successful delivery  | Process Monitor - Process Explorer - Wireshark - SmartSniff - ping - tracert - PortQry - netstat |
| Webservices | Reachable, responses, response time  | Charles Proxy, Fiddler, Wireshark    |
| Database (MSSQL) | Blocking queries, Query execution time, Index issues  | SSMS - SQL Server Profiler - sp_Blitz - PerfMon  |

# The tools

* Microsoft
  * PerfMon.msc
  * Eventvwr.msc
  * Fsmgmt.msc
  * Task Manager
  * RegEdit
  * ping
  * tracert
  * PortQry
  * netstat
  * PerfView
  * WinDbg
  * SSMS (SQL Server Management Studio)
  * SQL Server Profiler
* Windows SysInternals
  * Process Monitor
  * Process Explorer
  * VMMap
* NirSoft
  * RegScanner
  * BlueScreenView
  * WinCrashReport
  * RegFromApp _(this tool can partially be replaced by SysInternal's Process Monitor)_
  * ProcessActivityView _(this tool can entirely be replaced by SysInternal's Process Monitor)_
  * HeapMemView
  * DeviceIOView
  * SmartSniff

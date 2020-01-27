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
**Remember this: _any detail given by the end-user must be considered unreliable_ until you can reproduce the error yourself**.  
If/when you can reproduce the error on a separate computer of your own, you're most certainly working with something where it can be worth involving the developers of the software itself, if possible. But there are still things we can do before attempting to get a developer to assist us!

Here is a list of questions I try to find the answer for when I have an error I don't know the cause for:

1. What application is being used? _Version, integrations used and other relevant details_
1. Where in the application is the user working?
1. What has the user done up until the error occurred?
1. What is the user attempting to do?
1. What result/error is the user getting?
1. What result was the user supposed to get?
1. At what time (down to the second if possible) did this occur?
1. Can the user reproduce the error?
    1. Can the user reproduce the error on every/multiple logins?
1. Can you reproduce the error in the same installation?
    1. Can you reproduce the error in a separate installation?

### Misbehaving application

If you're not experiencing application crashes, hopefully the application will have stored information about something going wrong in one of its own logs so it's time to start going through these.
Remember to always keep track of the clock when the error(s) occurs, this will be vital when we're going through the log-files to be able to identify things that happened before the error, during the error and just after the error.
If the application stores its logs in plain text-files, you might want to use tools such as **Notepad++**, **klogg** or similiar; these can be configured to help you more easily spot error/warning/info/debug messages or help you clean a very cluttered log-file.

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
Performance, misbehaving or full application crashes can all be troubleshooted using Windows SysInternals Process Monitor.  
The cost is a lot of data having to sift through to find what you're looking for, but it's worth it. After a while, you'll develop a sense for what to filter out and what to keep, but I will do my best in future guides to try and give you some of that knowledge without having to go through hours of figuring things out on your own. 

Besides this magnificent tool, I will also give some shoutouts to other tools that I'll also try to describe how to work with and why you would want to use them.
Here is a list of tools to get a better understanding of what a application is up to:

* Windows SysInternals - Process Monitor
* Windows SysInternals - Process Explorer
* Windows SysInternals - VMMap
* NirSoft - WinCrashReport
* NirSoft - HeapMemView
* NirSoft - DeviceIOView
* PerfMon.msc
* Eventvwr.msc
* PerfView

## Operating System

Sometimes, a application will cause a error so severe that the entire O/S crashes. In these situations, depending on how Windows has been configured, you might need to change the settings in Windows as to how it handles Blue Screens.  
I generally recommend turning off the automatic reboot so you can see the error message and reboot yourself.  
NirSoft provides a handy tool, BlueScreenView, for looking at the last (and previous) blue screen memory dumps that has been created and it can help you try to figure out what caused the crash of the O/S.

* BlueScreenView

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

Local storage in Windows, or more specifically, unit drives that the O/S identifies as local, Windows will allow you to cached read/writes to.  
While using **Process Monitor** (using **Enable Advanced Output**, which you should) you'll likely see FASTIO_* operations that will succeed and fail every now and then when it comes to local storage.  
Fast I/O read requests will cause the System-process to convert any read-request of less than 128KB into specifically 128KB; this is quite important to remember when you're trying to figure out performance issues with I/O-access that aren't on what Windows considers local storage.  

As an example, around 2014, I was trying to figure out why database-reads were so incredibly slow when the dBase5-based database existed on a shared folder.  
I happened to see that System was doing larger reads than the application itself was asking for when I was running the same test but with all files on my local drive.  
The application was asking for about 4KB per read-request, but I saw that System always read atleast 128KB, which meant that only the first read-request took took about 1~9 milliseconds, and the following 124KB took only a fraction of a millisecond to complete.

#### Network Storage

These days, with SMB v1 having serious security flaws no one wants to risk having in production, this is perhaps unneccessary to discuss, but as an example, I will describe things I've done because of issues that SMB v2 and later poses for files that multiple computers write to with very short intervals (possible multiple writes within a second).

The problem we were seeing at multiple customer sites was file corruption, but we could never really with certainty explain why they occured or why some customers saw the issue more often than others. 
What we did know was this; all versions of Windows Server after window Server 2003 would sooner or later cause file corruptions. Windows Small Business Server 2011 was especially horrible we'd later learn, but that's a, partially, different story. 
We also knew, I'm not sure how we knew it at the time, but if we forced the customers to put the shared files on servers that were configured to only share files using SMB v1, and even better, with Opportunistic Locking (OpLocks) disabled, the issues almost always entirely dissappeared.
But then we had the issue of horrible read-performance to those shares (write as well, but read was especially bad) which was causing major complaints from the customers.

I went down a deep hole trying to figure out just how to squeeze out better performance, and also trying to understand in detail why the performance was so serverely hampered when we had the file-share configured in a manner that didn't cause corruptions.
And the answer to that question is: Fast I/O
More specifically, with SMB v1 forced and OpLocks disabled, Windows wouldn't cache anything locally so any read and writes had to go directly to the server. This 

## External services

### Dependencies

### Database

#### Microsoft SQL Server

### Webservice

REST-API:s, SOAP-API:s, GrapQL-API:s and countless alternatives to come.
For applications to handle the vast amount of users, plenty choose to split into multiple services to be able to split the load over multiple computers.
This creates the need for a way to communicate, and some of the ones that are heavily used today are known as REST, SOAP and GraphQL.  

For a support-technician, it can be vital to know **what** they are and **why** they're being used, even more so, we need to know **how** they're being used so we know **when** they're not doing what they're expected to.  
But we also need to know **how** to see what's going on and that last part I can help you with, the others is up to you to investigate since the application can use one or more of them at the same time.

# The tools in one long list

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
* [Windows SysInternals](SysInternals/ReadMe.md)
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
* Brent Ozar
  * sp_Blitz
  * sp_BlitzFirst
  * sp_BlitzWho
  * sp_BlitzCache
  * sp_BlitzIndex
  * sp_BlitzQueryStore
* Fiddler
* Charles Proxy
* ATTO Disk Benchmark
* snaketail-net
* glogg / klogg
* Notepad++
* IOMeter
* Wireshark

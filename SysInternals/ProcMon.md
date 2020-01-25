# Process Monitor (Windows Sysinternals)

[Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) is the tool that you'll hate for the overwhelming amount of data long before you begin to love it. But when you surpass that hurdle, when you start getting a feeling for what data you really need to capture and what to filter away, it will become a dear friend that will help you through some dark times!

* It can help you find bottlenecks in performance in more ways than one
  * I/O-performance in terms of response time per every single I/O-request (both local storage and network)
  * I/O-performance by showing amount of bytes per operation _(combined with the field **Duration** we can calculate Bytes Per Second)_
  * CPU-performance by measuring the amount of time spent between two operations 
  * Total amount of CPU time spent in either **User Time** or **Kernel Time** (which is also used to show much CPU was used at a specific time-point in the trace)
  * **Network response time/bandwidth** by looking at **duration time** for a operation of X amount of bytes
  * Stack traces for every captured event (so you can see which context an application has spent the majority of it's time with)
* It will let you see which parts of the registry the application(s) has worked with
  * if the key(s) existed to begin with
  * if any keys were updated, created, read or deleted
  * if we lacked the permissions to do a specific operation or not
* It will let you see which files the application(s) has worked with
  * if the file(s) existed to begin with
  * if any files were updated, created, read or deleted
  * if we lacked the permissions to do a specific operation or not
* It will give you a crude understanding of what network access the application(s) made
  * Amount of data transferred in both directions
  * If a connection was successful or not
  * Which IP (and sometimes hostname) the application(s) communicated with
  * A crude identification of what type of network access the application(s) made
* It can show you which DLLs/images the application(s) loaded, from startup until termination
  * Every single captured event will show what DLLs/images are currently loaded into the applications memory
* It can show you when the application(s) create/exit threads and how much CPU time the thread used during it's lifetime _(as long as Process Monitor was running during the threads full execution)_
* Since Process Monitor stores a stack-trace for _every single collected row of data_, it can give you a very comprehensive overview of what the application(s) spent the majority of their time doing based on it's own inner contexts. **This is mostly relevant for developers, but that also means that if you've captured a trace when a application is behaving badly, that trace can be highly useful for the developers to fix the issue you're experiencing, errors and performance-bottlenecks alike**. But another thing this will give you is the ability to see if some external DLL/image is what the application is spending the majority of its time waiting for so this is useful, even for non-programmers!
* All programs running during the capture is also visible under Process Tree which can give you an idea of other applications that might be causing issues
* The trace also tells you some basic information about the computer the trace was made on
* it also has tools that summarizes the data collected that can help you get a quick overview as to what went on while the application(s) were running
  * How often a specific file/registry-key/network-host was accessed (and in what manner)
  * How much CPU, I/O, RAM and network bandwidth was used **_(and the ability to click on the graphs to reach that specific point in the log!)_**
  * If any resources was accessed by multiple applications during the trace **(this is helpful to find out if for example a Antivirus is blocking a resource to scan it before our application of interest can use it)**
  * count the occurrence of different operations
  * do a **trace of a boot-up of the computer** so you can troubleshoot what it is taking such a long time or why some driver/application crashes during boot-up
* If you have the debugging-symbols for the application + source-code, you can tell Process Monitor to use these for the stack-traces and it can help you jump to the exact point in the sourcecode that triggered an event
  * it just makes the Stack Traces a lot more useful if you have the symbols so if you're working as support for a software company, try to **get access to the debugging symbols from the development team!** _(They'll probably be reluctant to give a copy of the source code, but for a support-technician, having that might be overkill anyway)_
* And since these traces can be saved to disk, you can easily share the information between multiple computers/people
  * By default, it will try to hold all captured data in RAM, but this can be switched to be stored on disk instead. The benefit of storing on the disk is that if Process Monitor itself crashes, you won't loose the data it capture up until that point

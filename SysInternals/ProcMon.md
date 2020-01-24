# Process Monitor (Windows Sysinternals)

This is the tool that you'll hate long before you begin to love it. 

* It can help you find bottlenecks in performance in more ways than one
  * I/O-performance in terms of response time per every single I/O-request (both local storage and network)
  * I/O-performance by showing amount of bytes per second
  * CPU-performance by measuring the amount of time spent between two operations
  * Total amount of CPU time spent in either **User Time** or **Kernel Time**
  * **Network response time/bandwidth** by looking at **duration time** for a operation of X amount of bytes
  * Stack traces for every captured event (so you can see which context an application has spent the majority of it's time with)
* It will let you see which parts of the registry the application(s) has touched in any way
* It will let you see which files the application(s) has touched in any way
* It will give you a crude understanding of what network access the application(s) made
  * Amount of data transferred in both directions
  * If a connection was successful or not
  * Which IP (and sometimes hostname) the application(s) communicated with
  * A crude identification of what type of network access the application(s) made
* It can show you which DLLs/images the application(s) loaded, from startup until termination
  * Every single captured event will show what DLLs/images are currently loaded into the applications memory
* It can show you when the application(s) create/exit threads and how much CPU time the thread used during it's lifetime _(as long as Process Monitor was running during the threads full execution)_
* Since Process Monitor stores a stack-trace for _every single collected row of data_, it can give you a very comprehensive overview of what the application(s) spent the majority of their time doing based on it's own inner contexts. **This is mostly relevant for developers, but that also means that if you've captured a trace when a application is behaving badly, that trace can be highly useful for the developers to fix the issue you're experiencing, errors and performance-bottlenecks alike**

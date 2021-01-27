# Chapter 1 Hello, world of concurrency in C++ 



*What is concurrency?*

Concurrency is about two or more separate activities happening at the same time.



- Task switching: one processor with a single processing core

- Hardware concurrency: multiple processors or multiple cores within a processor



Context switch:

- save the CPU state and instruction pointers for the currently running task
- work out which task to switch to
- reload the CPU state for the task being switched to
- load the memory for the instructions and data for the new task into the cache



Two basic approaches to concurrency:

- multiple single-threaded processes :

  `thread1 <-> interprocess communication <-> thread2`

  cons:

  - communication between processes is often either complicated to set up or slow, or both
  - there's an inherent overhead in running multiple processes (time and resources) 

  pros:

  - easier to write safe concurrent code
  - can run the separate processes on distinct machines connected over a network

- multiple threads in a single process:

   `thread1 <-> shared memory <-> thread2`

  cons:

  - must ensure that the view of data seen by each thread is consistent whenever its accessed

  pros:

  - the overhead associated with using multiple threads much smaller than that from using multiple processes



Concurrency vs. parallelism

Both terms are about running multiple tasks simultaneously, but parallelism is much more performance-oriented. 



*Why use concurrency?*

- separation of concerns
- performance




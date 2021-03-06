---
title: Multiprocessor Scheduling
date: "2021-10-11T23:46:37.121Z"
template: "post"
draft: false
slug: "multiprocessor-scheduling"
category: "Operating System Notes"
tags:
  - "Notes - "
  - "Operating System"
  
description: "Notes on the different types of scheduling systems for multiprocessors"
socialImage: "/media/image-2.jpg"
---
# Motivation
Most computers are multiprocessors. How do we use multiple cores to run sequential tasks?
How do we adapt scheduling algorithms for parallel applications?

# Sequential Scheduling - Naive Approach
Create a single multi-level feedback queue(MFQ) with a lock so that only one processor can read/write from it at once. Each idle processor takes the next task off the MLFQ and runs it.

**Potential Problems**:
  - **Contention for MFQ lock:** Many Processes are fighting over the MFQ lock
  - **Cache Coherence Overhead:** Processes need to fetch the current state of the MFQ from the cache of previous processors to hold the lock. The cache for the current processor is likely not going to be of much use since most of it will need to be updated. Also, you are fetching/updating the cache data while you are holding the MFQ lock, causes further delays.
  - **Limited Cache Reuse** Since threads run on the first available processor, that processor probably doesn't have the necessary data in its cache. 

# Commercial Operating System Scheduler
Use a separate copy of the MFQ for each processor to maximize cache reuse.

**Affinity Scheduling:** Once a thread has been scheduled on a processor, return it to the same processor when it is re-scheduled to maximize cache reuse. Each processor has its own copy of the queue to run threads from, only rebalance the queue length differences are large enough to compensate for the cache reload for the threads we move to different queues. Unfortunately locks are still necessary since per processor data structures must be protected by locks.

**Definition-Preempting a thread/process:** A thread is pre-empted when a higher priority thread is placed on the ready queue and the higher priority thread runs instead of the current thread.

# Oblivious Scheduling
Each thread is time sliced to available processors with no attempt to ensure threads from the same process run at the same time.

![Oblivious Scheduling Results](/media/7.2-Multiprocessor-Scheduling/ObliviousScheduling.JPG)

**Definition-Critical Path:** The minimum sequence of steps for the application to compute the result.  
**Definition-Spin-then-wait:** If a lock is busy, the waiting thread spin-waits briefly for it to be released, and if it still busy, it blocks and looks for other work.   
Pros: Great if the lock is held for short periods of time.

**Potential Problems:** 
  - **Critical Path Delay-** Preempting a thread on the critical path will slow down the end result.
  - **Bulk Synchronous Delay-** A common design pattern is to split work into equal sized chunks, do the work on the chunks, then communicate the work done to the next stage of computation(Think Google mapreduce). However, at each time step the work done is limited to the slowest processor, since all the chunks need to be finished before moving onto the next stage computation.
  - **Producer-consumer Delay-** Another common design pattern is the producer-consumer. The results of one thread are fed to the next thread, of which the results are fed to the next thread, etc. Preempting a thread in the middle of a producer-consumer chain will stall all of the processors in the chain.
  - **Preemption of lock holder-** In spin-then-wait strategies for locks, the lock holder can be preempted-other tasks will spin-then-wait until the lock holder is rescheduled.
  - **I/O-** If a read/write happens in the kernel, the thread blocks. To use the processor when the thread is waiting, the application needs to have more threads and processors so the extra threads can run while one is waiting. However, if the thread does not block(ie a read when the file is in memory), then we cannot do time slicing since we have more threads than processes.

# Gang Scheduling
Approach: Schedule all of the tasks of a program together. The application groups some work into a number of threads, and the threads run together or not at all. If the OS needs to schedule a different application or there aren't enough idle resources, it preempts all of the processors of an application to make room

![Gang Scheduling Results](/media/7.2-Multiprocessor-Scheduling/GangScheduling.JPG)

Implementation in Commercial Operating Systems: The application pins each thread to a specific processor and mark it to run with high priority. The system uses the rest of the processors to run other applications without interfering with the primary application.

**Problems**: As the number of processors increase, performance grows slower since some applications scale linearly with the number of processors, other achieve diminishing returns. 

Thus, it is usually more efficient to run two parallel programs with half the number of processors vs time slicing the two programs and gang scheduling.

**Definition-Space Sharing:** Allocating different processors to different tasks. Efficient since it minimizes processor context switches. The problem occurs when the not all tasks start and stop at the same time, how do we know how many processors to use if the number changes over time?

# Scheduler Activations
Make the assignment and re-assignment of processors to applications visible to applications. Each application has an execution context(aka scheduler activation). Each application is explicitly informed when a processor is added to the allocation or taken away via an upcall. 

This leaves the question of how many processors should each process be assigned? This is an open research problem, there is a fundamental tradeoff between average response time and fair allocation of resources among different applications. In multiprocessors, average response time can be improved by giving extra resources to parallel interactive tasks as long as it does not cause long-running compute intensive parallel tasks to starve for resources.

## Remaining Questions
1. What is the difference between MLFQ and MFQ? Are they the same thing just abbreviated differently?
2. Even if one of the waiting processors picks up the pre-empted task, a single preemption can delay the entire computation by a factor of two, and possibly even more with cache effects. Why is this the case with Bulk synchronous delay for oblivious scheduling?
3. Preempting a thread in the middle of a producer-consumer chain will stall all of the processors in the chain. Shouldn't this be stall all of the processors after this thread in the chain?
4. Performance as a function of the number of processors, for some typical parallel applications. Some applications scale linearly with the number of processors; others achieve diminishing returns. Why is this the case?
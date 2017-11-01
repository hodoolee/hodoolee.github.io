---
layout: post
title: Process Scheduling
comments: true
tags:
- Operating Systems
---

# Process Scheduling Queues

1. **Job queue:**
- also called as job pool.
- keeps all the processes in the system.

2. **Ready queue:**
- doubly linked list of processes which are ready to run.
- ordered by priority and highest priority process is at the front of the queue.
- header of the queue contains two pointers. 

3. **Device queue:**
- processes which are blocked due to unavailability of an I/O device cinstitute this queue.
- there is a separate queue for each I/O device.

# Schedulers
It selects the jobs to be submitted into the system and decides which process to run. These are Different Types of Schedulers:

1. Long-Term Scheduler
- also called as job scheduler.
- determines which programs are admitted to the system for processing.
- selects processes from the queue and loads them into memory for execution.
- controls the degree of multiprogramming.
- time-sharing OS does not have a long-term scheduler.

2. Short-Term Scheduler
- also called as CPU scheduler.
- also called as dispatcher.
- make the decision of which process to execute next.
- faster than long-term schedulers.
- allocates CPU resources to one of process which is ready to execute.

3. Medium-Term Scheduler
- part of swapping.
- removes processes from the memory.




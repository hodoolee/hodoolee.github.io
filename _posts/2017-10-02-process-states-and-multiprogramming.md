---
layout: post
title: Process States and Multiprogramming
comments: true
tags:
- Operating Systems
---

# Process States
1. **New:** contains the processes which are newly coming for execution.
2. **Ready:** contains the processes which are in main memory and available for execution. 
3. **Running:** contains the process which is running or executing.
4. **Exit:** contains the processes which are completely executed.
5. **Blocked:** contains the processes which are in main memory and awaiting an event occurrence.
6. **Blocked Suspend:** contains the processes which are in secondary memory and awaiting an event occurrence.
7. **Ready Suspend:** contains the processes which is in secondary memory but is available execution as soon as it is loaded into main memory.

# Multiprogramming
- There are one or more programs loaded in main memory which are ready to execute
- The main idea of multiprogramming is to maximize the use of CPU time.
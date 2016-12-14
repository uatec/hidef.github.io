---
title: Management as Multithreading - Part 2
date: 2016-11-03
tags: ["management", "agile", "multithreading"]
layout: post
---
[Previously](/2016/11/02/management-as-multithreading) I wrote about how management of a team and multiple work flows can be viewed as similar to the multithreading problem in software development. I also explored some key multithreading concepts and compared them to work management.

There are more concepts that are worth exploring.

<!--more-->

### Thread Pooling

When you have a numbe of long running tasks in software it makes sense to spin up a thread for a task, and when the task is complete, tear that task down. If you are managing an organisation you might do the same; hire a contractor for 6 months to do a task and when the task is complete, let the contractor go.

If you have many different tasks cropping up all the time, the effort of finding a contactor for each task, like the time involved in starting a new thread, can become prohibitively expensive. This is why organisations employ permanent staff which are kept on retainer for when they are needed, and why a thread pool can be used to have threads ready and waiting to perform tasks.

By maintaining a pool of workers or threads, which will will perform tasks on demand we can remove the overhead of begining and ending threads/employment if we are willing to take on the cost of maintaining these while there is no work available.

In practice an organisation is always able to find work for us to do.

### Task Scheduling

In computing the term 'scheduling' can mean more than one thing:

- Choosing 'when' a task runs. Fairly obvious.
- Choosing 'where' a task runs. If there are a number of environments with different capabilities, a memory set of tasks may be routinely scheduled to run in an environment with alot of memory, or highly parallel work on an environment with a GPU.

As humans we don't have RAM or SSDs or GPUs, but we do all have different skills and capabilities.

It is a managers role, or that of a self managing team, to act like the scheduler in a software system, to allocate tasks to the relevant individuals or teams. 

---

I hope that thinking about thinking about these familiar concepts in different ways may help us appreciate the nuances of both, and may be also gain a bit of understanding for responsibilities that might be outside our own realm.
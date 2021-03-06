---
title: Uniprocessor Scheduling
date: "2021-10-10T23:46:37.121Z"
template: "post"
draft: false
slug: "uniprocessor-scheduling"
category: "Operating System Notes"
tags:
  - "Notes"
  - "Operating System"

description: "Notes on the different types of scheduling systems for uniprocessors"
socialImage: "/media/image-2.jpg"
---

## Motivation

Given multiple tasks, how do we decide which one to do first?

## Uniprocessor

Everything discussed below is work conserving - ie never leaves processor idle

## Fifo/FCFS

Perform each task in the order it arrives. Run each task until it finishes

**Pros:** No overhead, fair since every task waits their turn, simple  
**Cons:** If a long task is scheduled before a short task, convoy effect dramatically raises average wait time. Average response time depends on task size distribution

## Shortest Job First (SJF) / Shortest Remaining Time First (SRTF)

Perform the shortest job first. Run each task till it finishes

**Pros:** Optimal strategy  
**Cons:** Impossible, Increases the variance of average response time, Starvation can occur due to too many short jobs coming in which blocks off the longer jobs on the queue

This shows that there is a tradeoff between reducing average response time and the variance of average response time

## Round Robin

Tasks take turns on the processor, if the task is not completed in the given time quanta, it goes to the end of the queue  
**Short time quanta:** Too much time spent context switching, no work will be done
**Long time quanta:** Tasks spend a long time waiting for a turn
**Infinite time quanta:** Same as FIFO
**Zero time quanta:** SJF

Simultaneous multithreading(SMT)/Hyperthreading: Switch between two processors on a cycle by cycle basis

**Pros:** No starvation  
**Cons:** Context switching takes time, need to evict cache in addition to registers of processor

## Max min fairness

Iteratively maximize the minimum allocation given to a particular process until all resources are assigned

If all processes are compute bound, this turns into round robin.

If some processes don't use entire share of processor becasue they are short running or IO bound, give those processes their entire request and redistribute the unused portion to remaining processes. Recursively distribute unused portions until nothing left. When no remaining requests can be fully satisfied, divide the remainder equally among all remaining processes.

**Pros:** Good for IO bound tasks, no starvation  
**Cons:** Computationally expensive

## Multi Level Feedback

Extension of Round Robin but uses multiple round robin queues, each with a different priority level and time quantum. Higher priority level queues go before lower priority queues, higher priority levels have shorter time quanta than lower levels.

Meant to favor short tasks over long ones. New tasks enter at top, drops to lower queue when it uses up its time quanta. If task yields the processor bc it is waiting for IO, it stays on the same level, and is removed when task completes.

Increases Priority when process receives less than its fair shares, reduces priority when process has more than fair share

Goals: Run short tasks quickly, Low Overhead, no starvation, fair, defers maintenance tasks

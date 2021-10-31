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

## Bankers Algorithm

- Motivation: Avoid deadlocks when working with threads
- Idea: When a thread requests a resource, the OS checks if it would deadlock
  - If so, wait for other threads to release resources
  - If not, grant the resource right away
- Implementation: State max resources that thread needs. Allow thread to proceed if `available resources - requested resources >= max remaining needed by any thread`
- Keeps system in a safe state

## Precise Exceptions

At the time that an exception handler begins execution, there is a well-defined instruction
address in the instruction stream for which all prior instructions have committed their results and no following instructions (including the excepting one) have modified processor state.

## Meltdown Security Flaw

Many processors allowed timing windows in which illegal accesses could be performed
speculatively and made to impact cache state – even though the speculatively loaded data
was later squashed in the pipeline and could not be directly used.

## Earliest Deadline First(EDF) scheduler

- The scheduler to use for realtime systems. Runs the task with the earliest deadline first.

## Beladys anomaly

- Increasing number of page frames increases number of page faults for FIFO, Random Page Replacement

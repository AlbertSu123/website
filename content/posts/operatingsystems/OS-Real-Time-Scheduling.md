---
title: Real Time Scheduling and Overload Management
date: "2021-10-12T23:46:37.121Z"
template: "post"
draft: false
slug: "real-time-scheduling"
category: "Operating System Notes"
tags:
  - "Notes"
  - "Operating System"
  
description: "Notes on the different types of real-time scheduling systems"
socialImage: "/media/image-2.jpg"
---

# Real Time Scheduling

## Motivation
The software that runs the brakes in your car or autopilot on an airplane must work when called for, you can't hit the brakes to stop after you have crashed your car. The change in value of a specific task over time is shown in the below graph.

![Oblivious Scheduling Results](/media/7.4-Real-Time-Scheduling/Motivation.JPG)

## Three techniques to increase likelihood that threads run before their deadline
  - **Over provisioning:** Make sure real-time tasks only use a fraction of the system's total processing power. This is like taking 2 units when you know you can handle 16 units.
  - **Earliest Deadline First:** Choose the task that has the earliest deadline first. However, this won't always work with the given tasks due to it requiring both I/O and computation. Thus, always break tasks down into shorter units and use the deadlines of the shorter units.
  - **Priority Donation:** The problem of *priority inversion* occurs when a higher priority thread must wait for a lower priority thread to complete its work. This can be solved via Priority Donation, where a higher priority thread that is waiting for a lower priority thread donates its priority to the lower priority thread, allowing it to run and finish using that resource.

# Questions to ask
1. For priority donation, do you actually change priority values by subtracting from the priority of the higher priority thread and adding to the priority of the lower priority thread or do you just set the lower priority thread's priority to the priority of the higher priority thread?
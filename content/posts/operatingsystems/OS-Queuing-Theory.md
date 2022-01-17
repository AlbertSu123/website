---
title: Queueing Theory
date: "2021-10-12T23:46:37.121Z"
template: "post"
draft: false
slug: "queueing-theory"
category: "Operating System Notes"
tags:
  - "Notes"
  - "Operating System"
  
description: "Notes on Queueing Theory"
socialImage: "/media/image-2.jpg"
---

# Queueing Theory
Response time depends non-linearly on the rate that tasks arrive at a system. For all intents and purposes, we assume that the system is work-conserving(all tasks that arrive are eventually serviced). Secondly, we are using FIFO scheduling.

# Definitions
- **Server:** A server is anything that performs tasks. ie web server, processor in a client machine, cashier in supermarket
- **Queueing delay(W):** The total time a task must wait to be scheduled. aka wait time
- **Number of tasks queued(Q):** number of tasks on the queue
- **Service time(S):** The time to complete a task assuming no waiting. aka execution time.
- **Response time(R):** Queueing delay(W) + Service time(S)
- **Arrival rate(lambda):** Average rate at which new tasks arrive
- **Arrival process:** When tasks arrive and whether arrivals are bursty or spread evenly over time.
- **Service Rate(u):** Number of tasks the server can complete per unit time when there is work to do.
- **Utilization(U):** Fraction of time the server is busy. `U = lambda / u` if lambda < u, else `U = 1` if lambda >= u
- **Throughput(X):** Number of tasks processed by the system per unit of time. `X = U * u` We can rewrite this as `X = lambda if U < 1, else X = u if U=1`
- **Number of tasks in the system(N):** The number queued + number receiving service. `N = Q + U`

# Little's Law
Conditions: Applies to any stable system where the arrival rate matches the departure rate. It relates the average throughput, response time, and number of tasks in the system.
`N = X * R`

# Response Time vs Utilization
- Higher Utilization normally implies higher queueing delay and higher response times.
- Higher Utilization also increases the risk of overload(where requests grow faster than they can be processed, thus queues and wait times grow without bound)
- Can predict average response time from the arrival process and service time but it is difficult
- Goal: make tradeoffs between higher utilization and better response time. In the fast, higher utilization was favored since computers were expensive, now, it is better to have a better response time since computers are cheaper than humans.
- **Best case(Evenly spaced arrivals):** Suppose we have a set of fixed sized tasks that arrive equally spaced from one another. As long as the rate at which tasks arrive is less than the rate at which the server completes the tasks, there is no queueing since the server finishes the previous request just in time for the next arrival.
- Suppose arrivals come in every 1 ms and the server can process each task in 1ms. If at some point a single extra request arrives, every other request will need to wait 1ms for that request.
- **Worst case(Bursty arrivals):** Suppose a group of tasks arrive exactly at the same time. The average wait time increases linearly as more tasks arrive together. 
- **Exponential arrivals:** A useful model for understanding queueing behavior is the exponential distribution. This can describe the time between tasks arriving and the time it takes to service each task.
An exponential distribution of a continuous random variable with a mean of 1 / lambda has the probability density function `f(x) = lambda * e ^ -(lambda * x)
The exponential distribution is *memoryless*, ie no matter how long we have already waited for the event, the likelihood of that event occurring remains the same.

# What if Questions

## What if we changed the scheduling policy from FIFO to something else?
- If the distribution of arrivals or service times is less bursty than an exponential(ie evenly spaced or Gaussian), FIFO will deliver nearly optimal response times. Round robin will be worse than FIFO
- If task service times are exponentially distributed but individual task times are unpredictable, the average response time is exactly the same for Round Robin as for FIFO.
- If task lengths can be predicted and there is variability in service times, Shortest Job first can improve average response times, particularly if arrivals are bursty
- Real world systems usually have more bursty arrival times than the exponential distribution, their distributions are known as *heavy tailed distributions* since it has more long tasks. SJF is better than Round Robin which is better than FIFO for *heavy tailed distributions*
- SJF can improve average response times but increases response times for long tasks since long tasks will only complete immediately before an idle period. As utilization increases, the idle periods become increasingly rare.

## What if workloads vary with the queueing delay?
- Arrival rates and service times are not always independent of queueing delay. For example, a online store becomes overloaded and slow during the holiday shopping season. Rather than continuing to browse, some customers may get fed up and leave, reducing the arrival rate of requests for individual web pages.

## What if we had multiple servers?
- Banks have multiple tellers, grocery checkouts have multiple cashiers. Clearly, there are often efficiency gains from having separate queues.
- On the other hand, a single queue is always better than separate queues when focusing on average response time because of variations in how long each task takes to service. One queue could be idle while another queue has multiple tasks

## If a processor is 90% busy serving web requests and we add another processor to reduce the load, how much will that improve average response time?
- Unfortunately, there is not enough info to say since each web request needs processing time and disk I/O and network bandwidth. 
- If arrival times and service times are exponentially distributed and independent of the system response time, the overall response time is the sum of the response times of the components `response time = sum for all arrival and service times(S / (1 - U))

## What are some conclusions we can draw from real world systems?
- Almost all real world systems exhibit some randomness in their arrival process or service times
- Response time increases with increased load
- System performance is predictable across a range of load factors if we can estimate the average service time per request.
- Burstiness increases average response time.

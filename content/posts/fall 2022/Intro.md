---
title: Intro to Networking
date: "2022-08-25T00:00:00"
template: "post"
draft: false
slug: "networking"
category: "Networking Notes Notes"
tags:
  - "Notes"
  - "Lecture"
  - "Networking"

description: "Notes on Networking"
socialImage: "/media/image-2.jpg"
---

# Lecture 1 - Intro to the internet, protocol, architecture

What is the internet?
- Goal: transfer data between end hosts
- Infra: TCP, IP, DNS, etc.
- Applications: facebook, google, twitter. <- don't give a shit about this in class
- End hosts: laptops, phones, pacemakers
- Switch: help end hosts communicate. ie router, phone on hotspot
- ISP: aws, at&t, UC Berkeley

## Protocol: specification of messages that communicating entities exchangeSpecifications
 - Useful for getting someone on board
 - If you design the protocol poorly you will spend lots of time debugging

 ## Architecture
- Network vs the Internet- How do you interconnecy
- What ifno should googogle share via amazon

### Protocol Design Factors
- Incentives for ISPs, topology, path selection, diagnostics, customers
- Enormous diversity and dynamic range with respect to parameters(packet loss, bandwidth, communication latency, types of endpoints, different users)
- How do you differentiate whe interoperability relies on a common protocol?
- Upgrading the internet not an option
- Speed of light => cannot go faster from berkeley to NY than 27.5 milliseconds
- Portions of the internet fail(software, switches, links, network interfaces)

### The protocol design paradigm for the internet
- Decentralized control: Software defined networking(SDN), dsdn
- Best effort service model
- Route around trouble
- Dumb infra with smart endhosts: NFV: Richer in network services
- e2e design principle: edge computing
- layering
- federation via narrow waist interface

How to prioritize goals?
What are the fundamental constraint?
How to decompose a problem?

# Lecture 2 - How to share networking resources
Best Effort: Packet Switching, just send packets and hope for the best
Reservations: Circuit switching, end hosts reserve bandwidth when needed

## Which is better? Circuit vs Packet Switching?
Factors to consider:
- Abstraction to applications
- Efficiency(at scale)
- Handling Failures(at scale)
- Complexity of implementation(at scale)

As an application developer, circuits offer better application performance due to reserved bandwidth and is more predictable and understandable. This is also very intuitive in support of business models(ie charge based on use). However, 

## Smooth vs Bursty Applications
- Definition: Difference between peak and avg transmission rate
- Small speak to avg ratio(3:1) - ie voice
- Bursty ratio(100:1) - Ie Data applications, web browsing
- Phone network uses reservations, internet does not
- Circuit switching spends some time to setup/teardown circuits, inefficient when you don't have much data to send

## Failures
### Circuit Switching
- Network must do all the things needed for packet switching
- Endhosts must detect failure, teardown old reservations, send new reservation request
- All endhosts must do this

### Packet Switching
- Link goes down, then what
- Network detects failures, recalculates routes
- Transient overload: When a point in time when you receive two packets at the same time

## Resource Reservation Protocol
- Packet switching is the default
- Used to setup state
- You can also buy a dedicated circuit: ie MPLS circuits, leased lines, 
  - Often used by enterprises from one branch location to another
  - very expensive(10x higher than normal connection)
  - statically set up, long lived, and per user(not per flow)

## Circuit vs Packet Switching a History
- 1970s-80s used packet switching, well suited to bursty file transfer applications
- 1980s-90s believed we'd need circuit switching. Envisioned that voice/live TV would be the Internet's true killer app

### Life of a Packet
- Source has data to send to destination
- Chun it into packets: each packet has payload and header
- Packet travels along a link
- Arrives at a switch, switch forwards to next hop(or buffer or drop packet)
- Repeat last step until we reach destination or packet is dropped

## Designing the Internet Top Down

### How to solve a problem?
1. Define the problem
2. Decompose the problem - Modularity(barbara liskov ted talk)
    - Decomposing system into smaller units, providing a separation of concerns
    - Motivation: Musk wants to send bezos a hand written letter. The letter goes from Musk -> aide -> fedex -> fedex -> aide -> Bezos. 
    - Each level understands the other people on their level
3. Assign tasks to entities

### Internet Modularization
Lowest Layer of Abstraction \
Physical transfer of bits \
Best effort local packet delivery \
Best effort global packet delivery \
Reliable/Unreliable data delivery <- Layer where we care about packets \
Applications \
Highest Layer of abstraction

### Local vs Global Delivery
Ethernet networks only speak to Ethernet networks
Optical Transport networks only speaks to optical networks.

# What is a Protocol?
- An agreement between parties on how to communicate
- Defines syntax and semantics
- Exist at multiple layers  
- Each layer of the stack has multiple protocols except the network layer, which only has IP
  - Each layer depends on the layer below, supports the layer above, and is independent of the same layer protocols

## Why Layering?
- Innovation can proceed in parallel #OpenInnovation
- Different communities can innovate, ie L7(app devs) vs L1(chip designers)
- The end host must implement all layers
- The network must implement L1-L3, L4+ is for reliability and reliable data transport
- Applications think about data, nic/driver cares about packets, network stack translates between these two
- Ethernet doesn't know how to talk to OTN, Ethernet goes to IP which sends it to OTN which sends it to other OTN which sends it to IP which sends it to Ethernet

### Layer Encapsulation
1. Take data, add app Header(ie HTTP)
2. Add transport header(ie port number)
3. Add Network header(ie host address, ip)
4. Add L2 header(ie ethernet address)
5. Now your packet should be on wire and have a header from all layers!

## Architectural Wisdom
- End to End arguments in system Design
- The Design Philosphy of the DARPA internet protocols

### End to End Principle
- Guides the debate about what functionality the network does or doesn't implement
- Should we implement reliability in the network?
- Solution 1: Implement reliability at each step(network must implement reliability)
  - Problem: Cannot be perfectly reliable. What happens if a component has a bug or fails between two steps.
- Solution 2: End to end check and retry(does not assume network is reliable)
- Sometimes you want reliability in the network to increase performance
# Questions?
1. Difference between packet switching and circuit switching?
2. How do we do reliable packet delivery?

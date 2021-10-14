---
title: Oracles
date: "2021-10-14T23:46:37.121Z"
template: "post"
draft: false
slug: "Oracles"
category: "DeFi Lecture Notes"
tags:
  - "Notes"
  - "DeFi"
  
description: "Notes on the implementations and real world examples of oracles"
socialImage: "/media/image-2.jpg"
---

# Motivation
From the previous stablecoins post, Ampleforth's AMPL is one example of a stablecoin that requires an oracle to properly function. Another example is flight insurance, how does the blockchain know when a flight has been delayed? Some other DeFi applications that need data include sports betting, wrapping other cryptocurrencies(bitcoin), synthetic assets(stocks), and undercollateralized lending.

The blockchain does not have a connection to the internet: How do we get external data to contracts?

# Building an Oracle

## Basic Oracle Design
Build an oracle into the consensus protocol. 
  - What if the miner lies or cheats?
      - Use multiple miners and use the data of a randomly selected miner.
  - What if the sending node lies or cheats?
      - Use digital signatures and take the majority answer.
  - What if the sending node goes down?
      - Allow for backup transmission.
  - What if the web site with data goes down?
      - Use multiple websites.

This results in a decentralized oracle with good liveness, what if there are malicious attackers in the system?
  - What if nodes disagree?
      - Take the majority value
  - What if nodes report different numerical values?
      - Use the median. Given a minority of bad values, a median is an honest value or bounded by honest values.

### Remaining Problems
These problems are usually solved at the protocol level, so solutions will differ based on the specific implementation.
- How do we ensure that a majority of nodes are honest?
  - How do we ensure nodes get paid for their service?
  - How do we ensure that oracle reports are mined by miners in a timely way?

# Implementations of Oracles

## Decentralized Exchanges (DEXes)
Decentralized Exchanges enable on chain trading of various asset pairs. As a result, the exchanges have the current valuation of the asset price, since mispricing the asset would result in an arbitrage opportunity. Thus, we can use the decentralized exchange price as the oracle price!

Pros: Instant response and composable with other contracts! Also, there is an economic assurance that the oracle is correct.
Cons: Only price information can be returned and the oracle can be manipulated.

### Decentralized Exchange Price Attack
Situation: bZx, a lending protocol, used the Kyber exchange as a price oracle. Their ETH loans were backed with sUSD collateral based on Kyber price. 
Attack:
1. The attacker sold ETH for sUSD on Kyber to drive down the ETH/sUSD price
2. The attacker borrowed ETH for sUSD cheaply on bZx since the oracle reported that ETH is worth a lot less sUSD than it once was.
3. The attacker ran away with the loaned eth, making 2381 ETH(673k USD)

Countermeasure: Use time weighted average oracles(TWAP) so attackers can't drive down the price for any significant periods of time.

# Verifiable Random Functions (VRFs)
Problem: Since everything is public on the blockchain and we can't introduce any external data without an oracle, there is no randomness since we could simulate what will happen on chain by reading the mempool and running the transactions on a local node.

Idea: The oracle uses a secret key to generate randomness. The randomness is verifiable and unpredictable.

# Oracle Privacy
Problem: Alice wants to buy flight insurance for her trip from San Francisco to New York, but how can she do that without everyone knowing that she's flying from San Francisco to New York?

Partial Solution: Use a trusted execution environment. This ensures integrity, as no one, not even the owner of the computer with the trusted execution environment can tamper with an application executing in it. This also ensures confidentiality, as no one, not even the owner of the computer with the trusted execution environment can learn about the data running in the environment. 

Problem: Hardware based trusted execution environments have serious vulnerabilities. ie Intel SGX
Solution: Use a DECO, a trusted execution environment alternative built using cryptographic techniques!

# Remaining Questions
1. Why do we first learn about the mean in school then the median? Or at least this is what I learned in elementary school.
2. How does a DECO work?
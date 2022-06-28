---
title: Mirror Protocol
date: "2021-12-30T23:46:37.121Z"
template: "post"
draft: false
slug: "borrow"
category: "DeFi"
tags:
  - "Primer"
  - "Crypto"
  - "Mirror"

description: "A quick primer on Mirror Protocol from Terra Classic"
socialImage: "/media/image-2.jpg"
---

## Mirror

Overcollateralized synthetic asset
Deposit bluna, luna, ust
Mint some index(ie price of oil, mTesla)

## Mirror Risks

- Governance Risk
  - Create formal governance proposal, hide sus code in it, hope it passes before people realize(Same think that rekt beanstalk finance)
  - Mirror v2 has a system of incentives to achieve quorum
- Oracle Risks
  - S&P 500 oracle gave incorrect price and a large position was liquidated(250-500k)
- Structural/Economic Risk

## Geometric Brownian Motion

- Problem: continuous in time but users don't act continuous in time but rather per block. The agent can continue to add more collateral as the price moves down to prevent liquidations.

## Jump Process

- Poisson point process: gives the arrival of the jumps
  - The model of the arrival in each independent event. The probability that a certain event arrives within a given interval is poisson distributed. Modeled by probability mass distribution, lambda tells you how often the jump happens on average.
- Jumps: the prices at each jump

## Model

1. Trader mints mS&P by supplying bLUNA as collateral, they are required to supply at least
   `collateral amount >= collateral factor* mint AMount * collateral price / synthetic price`
2. If no jumps occur in interval, no risk of liquidation because no price changes
3. If one jump occurs, check if the jump was less than threshold.
4. If two jumps occur, check if the first jump was less than threshold.
5. If n jumps occur, need to check if any jump was less than threshold

## Questions

1.How does risk harbor provide protection against Mirror Liquidations?

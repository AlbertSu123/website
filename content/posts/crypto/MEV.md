---
title: MEV
date: "2021-12-30T23:46:37.121Z"
template: "post"
draft: false
slug: "borrow"
category: "crypto"
tags:
  - "Primer"
  - "Crypto"
  - "MEV"

description: "Quick primer on MEV"
socialImage: "/media/image-2.jpg"
---

Broke: MEV is consensus mechanism that is the source of issues
Woke: Problem is from application layer, this is similar to the issues in the HFT space.

People try to randomize arrival of orders, make fees, etc.
There was a good paper that prompted people to use batch auctions instead of central limit order books
CLOB: orders come in serial time, processed serially in order of arrival. This is the same as the transactions ordering in ethereum, whoever comes in first gets executed first.
Batch Auction: Instead of processing everything in continuous time one by one, wait and batch transactions and process them together. - For all transaction that came in this block, store them and execute them next block

To submit an order on a dex, you submit an order and a slippage tolerance. If the slippage tolerance is too lenient, you can get front run for up to the slippage tolerance.

This leads to people looking elsewhere for execution markets, ie flashbots. Since flashbots only has a certain percent of mining power, your order will execute slower than if you put your transaction in the standard pool.

Example:

1. Bob wants to buy 100 ETH. Order size +100 ETH
2. Joe wants to sell 100 ETH. Order size 0 ETH
3. Smitty wants to buy 10 ETH. Order size +10 ETH
   The batch order of buy 10 ETH would then be submitted

## Overview of MEV

- Frontrunning
- arbitrage
- Liquidations
- Generalized Frontrunning: replacing profitable tx with

### Liquidations

- Whoever gets the liquidation first gets the money
- Liquidate position, market dump tokens

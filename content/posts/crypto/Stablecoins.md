---
title: Stablecoins
date: "2021-12-30T23:46:37.121Z"
template: "post"
draft: false
slug: "stablecoins"
category: "Crypto"
tags:
  - "Notes"
  - "DeFi"
  
description: "Notes on the implementations and real world examples of stablecoins"
socialImage: "/media/image-2.jpg"
---

## Variables to account for in DeFi/Stablecoins
1. Interest Rates in CeFi and DeFi
2. Cryptocurrency prices
3. Collateral Ratio
4. DeFi Protocol Fees
5. Blockchain Transaction fees
6. Number of users
7. Blockchain transaction throughput

**Exogenous:** A variable determined outside the model that is imposed on the model. In MakerDao's DAI, the asset price of the collateral(ETH/USDC) is independent of the MakerDAO system.

**Endogenous Variable:** A variable determined by the model. In Synthetix, arbitrary derivatives are backed by SNX(a project internal token)

**Fractional Reserve:** When a bank only has a fraction of its deposits backed by cash and available for withdrawal. A bank's reserve ratio = Money held by the bank / Money deposited by bank customers

## What is Stability?
  - Stability is a relative metric. USD is stable compared to bitcoin but the price still fluctuates with respect to other assets.
  - We can measure volatility, the standard deviation of returns, and the worst drop over a timeframe. Some fiat, for example euros or pounds, have a volatility of 6-12%. However, volatility alone does not capture the range of prices.

## Stable vs Pegged Coins
  - Stablecoins include USDC, USDT, DAI, UST, etc. These coins are simply USD derivatives and are usually reserve based, collateral based, or algorithmic.
  - Pegged coins include wBTC, renBTC, sETH, stETH. They are pegged to the price of an asset, usually via the same reserved based, collateral based, or algorithmic strategies used by stablecoins.

## How do Stablecoins work?

### MakerDAO's DAI
1. The minter deposits ETH to open a collateralized debt position (CDP) where 150% collateral mints 100% debt. The collateral in this case could be ETH, and the debt create is represented as the DAI Stablecoin.
2. The minter can withdraw DAI to use for anything they want.
3. To get their deposited ETH back, the minter needs to pay back the same amount of DAI they withdrew to unlock their ETH.

### Synthetix sUSD
1. The minter deposits SNX to mint a derivative. 600% collateral mints 100% derivative. The collateral in this case is always SNX, and the derivative could be something like sUSD or even something like sTSLA shares.
2. The minter can withdraw their derivative. In the case of sTSLA, price oracles report the external prices to the blockchain, which entitles the derivative to a portion of the collateral based on the current price of TSLA shares.
3. The minter can pay back their sTSLA to unlock SNX

## Ampleforth (AMPL)
Ampleforth has no collateral, it is an algorithmic stablecoin
The price oracle constantly updates the AMPL/USD price. There are three cases the oracle reports:
1. If each AMPL is worth more than 1 USD, wallet balances that hold ample increase. For example, if you had one AMPL in your wallet and each AMPL is now worth 2 USD, you now have two AMPL in your wallet. This is known as expansion.
2. If each AMPL is worth less than 1 USD, wallet balances that hold AMPL decrease. For example, if you had one AMPL in your wallet and each AMPL is now worth .5 USD, you now have .5 AMPL in your wallet. This is known as contraction.
3. If each AMPL is worth exactly 1 USD, no action is taken. This is known as equilibrium.

# Remaining Questions
1. Even though AMPL is 1:1 USD, why is it worth anything? It isn't backed by collateral like DAI/USDC so what can you do with AMPL? How is AMPL minted?
---
title: Osmosis
date: "2021-08-22T23:46:37.121Z"
template: "post"
draft: false
slug: "osmosis"
category: "Defi"
tags:
  - "Notes"
  - "DeFi"
  - "Cosmos"

description: "Intern Notes on Osmosis"
socialImage: "/media/image-2.jpg"
---

# Osmosis
- Proof of stake with decentralized exchange app to provide liquidity, swap, and stake the tokens of blockchains from across the cosmos ecosystem

- Automated market maker connects to other tendermint based blockchains

- OSMO holders can delegate their assets to coinbase cloud public validator to earn a share of rewards (self staking and earn fees through token delegated to their validators)

- Innovating in liquidity pools by using osmosis’ governance processes and interoperable builder toolkit. OSMO native token in osmosis used for governance and paying transaction fees.

## Superfluid staking 
- If you deposit two supported tokens into pool can stake liquidity share to validators on the token’s chains
- Deposited asset will not only earn a share of fees from LP swap transactions but also earn rewards for helping to secure the chain
- Can provide liquidity to the osmosis AMM and participate in the native ecosystem of the tokens they hold by staking
    - Example: OSMO<> AKT pool’s LP tokens can secure both Osmosis and Akash networks

## Participating in Osmosis
- Validator nodes participate in osmosis consensus, propose blocks, and verify blocks proposed by other validators.
- Over time validators will be able to incur more responsibility in the ecosystem, such as acting as price oracles or bridges
    - Full Nodes: Interact with the osmosis blockchain
    - Archival Nodes: Store history state of the chain
    - Seed Nodes: Used to help nodes find and connect to peers
    - Sentry Nodes: Used to stand between validators and rest of the network, protect validators from DDos
- Validator’s chance of being selected to perform work and earn rewards for doing so is determined by weight of OSMO staked to the validator
- OSMO holders may delegate their tokens to an active validator to  help secure the network and earn a portion of the rewards
- Validators set a commission fee between 5-100% of total rewards  earned by the delegators
14 day unbonding period for delegation, unbonded OSMO do not earn rewards but are eligible for losses due to slashing. Delegators also have full voting rights within Osmosis’ governance

## Rewards and economics on Osmosis
- Governance token to vote, pay transaction fees, and to provide rewards to participants for providing security
- Annual reward rate is 160% per annum for validators
- 25% of released tokens will be issued to validators and their delegators as a reward for their work daily
- 760,000 -> 500,000 -> 340,000 -> 250,000 per day

### Three types of fees paid out to users
- Transaction Fees: Any user who transact on the chain, distributed to OSMO stakers and validators
- Swap Fees: Any trader swapping assets on Osmosis
    - Determined by LP parameters and size of trade
    - Distributed to pool’s liquidity providers
- Exit Fees
    - Percentage of LP shares and charged to liquidity providers who pull their liquidity out of a pool. Shares are burned and distribute their value to the other liquidity providers in the pool.
- 45% of tokens released per epoch are designated as rewards for liquidity providers in pools
- Any designated rewards that are reduced from mining reward by governance will be directed to the Osmosis community pool instead.
- Inflation on Osmosis is structured around a thirdening model with token issuance cut by ⅓ each year
- 100m initial supply 300 in first year 200 in second until a maximum supply of 1bn OSMO is issued

### Risk of participation
- Slashing: Validators committing a slashable offense will result in loss of funds and reward for validators and any delegators who staked their OSMO to that validator

### Governance on Osmosis
- Will add new features that rely on active participation of the community
- Designed such that most efficient solution is reachable through the process of experimentation
5% of token released daily on osmosis are directed to the community pool.
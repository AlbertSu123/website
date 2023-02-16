---
title: Crypto
date: "2023-01-20T00:00:00"
template: "post"
draft: false
slug: "crypto"
category: "Reflections"
tags:
  - "Crypto"
  - "DeFi"
  - "Reflections"

description: "Actually Useful Study Tips"
socialImage: "/media/image-2.jpg"
---

# Everything I'm still bullish on
## Bullish on Team and Idea
**Eigenlayer**: You've probably heard of Eigenlayer by now. For me, Eigenlayer is one a project that has the potential to grow so big that it becomes an existential threat to Ethereum, since if the supply of re-staked ETH approaches the total supply of staked ETH, weird incentives around the Ethereum Consensus mechanism could happen - or worse yet, there is an exploit in one of Eigenlayers' contracts. The reason for Eigenlayer's near guarenteed success is simple: anything that can be innovated on in the Ethereum ecosystem will be tested with Eigenlayer first. The main risks to Eigenlayer are also close to solved. Eigenlayer needs to grow fast enough to generate an ecosystem of applications that use restaked ETH, otherwise they risk someone forking their ecosystem and giving a bit less of their tokens to investors. k

Additionally, the team is a powerhouse. Instead of being a collection of anons that met on the internet, Sreeram Kanaan brought his entire research lab from the University of Washington - Seattle, a top 10 CS research university. Having met a few of the team, here are some highlights: 
- Sreeram: Professor at UW, Postdoc at Stanford and UC Berkeley. He's one of those rare people who has massive raw brainpower. I learned more about consensus mechanisms in 1 hour talking to this man than in the previous six months.
- Gautam: A paradigm fellow, a child genius who only eats frozen vegetables and hard boiled eggs for entire weeks since "life is too comfortable"
- CryptoBanker: Has found multiple exploits in live contracts as a whitehat
- Bowen, Shoubik, and the rest of the lab: PhDs at UW, nuff said.

Hot take: One year after Eigenlayer launches, it will have the highest TVL in all of Crypto.

### Projects to be built on top of Eigenlayer
Hyperscale Data Availability Layer: We can build a DA layer providing high DA rate and low cost by utilizing EigenLayer re-staking and some of the cutting edge ideas in data availability developed in the Ethereum community, including Danksharding. 
Notes: The Eigenlayer team is already working on this as the first product, not much alpha to take from here unless you want to challenge them on their own turf.

Decentralized Sequencers: Many rollups require decentralized sequencers for managing MEV and censorship-resistance at L2. These can be easily built on EigenLayer exploiting ETH stakers. 
Notes: Not sure how this would work nor what a value a decentralized sequencer would bring

Light-node bridges: It is easy to build light-node bridges to Ethereum using EigenLayer. For example, the rainbow bridge between Near and Ethereum is based on an optimistic pattern but it suffers from high-latency due to the high gas-cost of verification. It is possible for re-stakers to verify off-chain whether such inputs are correct, and if a strong cryptoeconomic quorum signs off on the bridge input, then the bridge input is considered accepted. If someone challenges, then the bridge input can be verified and the restakers can be slashed in the slow mode. 
Notes: This is just an optimistic bridge with no privileged positions, it also speeds up existing bridges

Fast-mode bridges for Rollups For ZK rollups, since the proof verification expense on Ethereum is still high, the rollup sequencers write to Ethereum somewhat infrequently, thus affecting composability and delaying strong confirmation guarantees. It is possible for a large amount of restaked quorum to participate in ZK proof verification offchain and certifying that proofs are correct on chain. These fast mode bridge claim if they turn out to be wrong can then trigger a slower slashing path. For optimistic rollups, eigenlayer can enable a much larger collateral pool to participate in certifying stateroots under risk of slashing. 
Notes: This just speeds up bridging from rollup to eth mainnet. 

Oracles: There have been proposals for enshrining price feeds into Ethereum or using the UNI token quorum for providing price feeds. Such oracles can be potentially built via Eigenlayer if all we require is majority trust on Ethereum re-stakers, and it is an opt-in layer. 
Notes: This is just a majority trust oracle. Simple, and not sure if someone would use this over coinbase. There would need to be major advancements needed here for this to be interesting, but sreeram and team say they have something interesting here.

Opt-In Event-Driven Activation: Event-driven activations such as liquidations, collateral refilling are currently not available natively on Etheruem. While they can be built on a separate layer such as keepers, such keeper nodes which do not control blockspace cannot make strong guarantees on inclusion of event-driven actions. In EigenLayer, native ethereum re-stakers, who are also block-proposers, can provide strong guarantees on event-driven inclusion at the risk of getting slashed. 
Notes: This could also be a major step forward for account abstraction, allowing miners to effectively pay the gas for this transaction if they wanted to. Event driven activations stuff for liquidations, collateral refilling aren't as interesting but we could check the crazy stuff people do for MEV to see if something would be interesting there.

Opt-In MEV Management : We note that a manifold variety of opt-in MEV management methods become feasible under EigenLayer, including Proposal-Builder Separation, MEV smoothing, threshold encryption for transaction inclusion. As a simple example, MEV smoothing can be built on top of EigenLayer by a committee of ETH restakers who then equally share their MEV. Any restakers who deviate from the prescribed MEV smoothing behavior can be slashed. 

Sidechains with ultra-low latency: EigenLayer allows the creation of re-staked sidechains where ETH re-stakers can participate in new consensus protocols, some of which have very low latency and very high throughput. Of course, a precondition for opting in to such sidechains is a higher computational power.

Single-slot finality It is possible to imagine single-slot finality, where nodes sign off on the finality of block via an opt-in mechanism EigenLayer. The core idea would be that nodes who have restaked can now attest that they will not build on a chain that does not include the testified block, thus creating a potential finality pathway. Designing it so that it is truly opt-in and does not break the consensus protocol, is an important direction of research
Notes: useful for IBC, but requires much more to be practical

Risk Harbor

## Bullish on Idea
Morpho/Ajna/P2P lending
Waymont
Beanstalk
Eigenlayer
Primitive finance
UXD protocol
Nomad
On chain foreign exchange or interest rate swaps
Gammaswap
Lien protocol
Sudoswap
Nounsdao
Risk swap: Use bond curves to swap between underwriting capital and protection purchase. Triggers when something defaults
## Bullish on Team
Crocswap
## Other
Cosmos
Independent Security Auditors

# Everything I'm bearish on

## Bearish on Team and Idea
Nexus Mutual
## Bearish on Team
Canto - Just because of my work on Evmos
Saddle Finance - poor code quality

## Bearish on Idea
MEV
Waves(the blockchain)
## Other

# Things you might not have know

### TVL is fake
People fake TVL all the time. Whether through recursive lending/borrowing, liquidity mining rewards, or a waves play, what you see if often not what you get. However, some projects and firms take this one step further where they make deals to pledge some portion of project tokens in exchange for a certain TVL amount. Some examples of this include platypus finance, where alameda deposited hundreds of millions in stablecoins and dumped the tokens that they received from the team.

### Random list of protocols that probably have an exploit or two in them
- [Compound forks](https://defillama.com/forks/Compound) on weird EVM alt L1s. Compound is safe protocol. Compound forks are not safe protocols. There are a wide variety of possible exploits on a compound fork:
  - if you use bad oracles, you will get flash loaned and drained
  - even if you have flash loan checks, the prices of the least liquid token can probably be manipulated, as there usually isn't too much liquidity on an alt L1.
  - If you have a single ERC-777 or ERC-677 token, someone will hit you with a reentrancy attack
  - If you use an old version of the compound v2 codebase, you will get hit with the `.transfer()` hack, the same one that stole 80 million from the Fei pool in Rari

### Tiers of DeFi Teams
There's basically three tiers of DeFi Teams
Tier 1: 30+ people, basically the MakerDAOs and the Uniswaps of the world. They usually have strong PMF and are heavily recognized, they ship open sourced code in addition to working on their project, and generally try to advance the industry in addition to their own projects.

Tier 2: Top End: Frax Finance, Bottom End: Saddle Finance. This is a barebones team, where there is at least one person covering each of the major facets required to run a DeFi project. They usually have one of each of the following: Solidity Engineer, Frontend Engineer, Designer(usually contracted), business lead(basically takes all the calls), community lead(sometimes combined with business lead, basically someone to sit in discord all day), and founder. Fun fact, legal action will destroy them, as they have no bandwidth or money to deal with that.

Tier 3: This is either a solo dev or an anon "team" of devs. It usually takes them a while to get going since they are basically a one man show, telegram takes a while for a response, things don't ship very quickly, tweets usually happen at most once a day, etc. I know this because I did this in the past.

### List of Novel or Interesting Defi Protocols you probably haven't heard of
https://www.gamma.xyz/
https://end.gg/
https://www.arpeggi.io/
https://sloika.xyz/
https://aspect.co/status
https://beta.catalog.works/
https://koop.xyz/
https://www.defifranc.com/
https://fantasm.finance/
Timeless finance LIT token distribution
tom howard is building stablecoins in cosmos
gyroscope
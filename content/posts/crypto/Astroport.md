Astroport
Non-custodial liquidity/price discovery for any asset
Prioritize flexibility
Allows users to choose different pool types within Astroport
Trade tokens permissionless
Provides
Multiple pool types
Advanced charting/analytics for LP/traders
Dual token rewards for LP
Shared fee structure for LP and staker
Governance tiers xASTRO vxASTRO
Integrate liquidity mining programs for new Terra tokens/accept dual rewards
Generate key data/oracle price for projects like Mars
Backbone for index projects or baskets of tokens
Supplying liquidity to wallets and trading aggregators that access liquidity from multiple DEXes
Underlying liquidity layer for yield farming protocols
Can use Wormhole v2 to expand to other blockchains

Astro Pools
Different token markets have different properties and characteristics
Choosing appropriate pool type for given token market can increase efficiency for traders and LP
Factors
Pair correlation
Pair volatility
Market locality - whether there are similar token markets to the target token market and their sizes relative to the target token market
Market maturity - how well understood the value of the two target tokens are
Lifecycle stage - whether tokens are pre or post liquidity
Accommodates for the following pool types
Captures constant product pools
Stableswap invariant pools
Astroport has flexible architecture that allows builders to create new pool types that fit within Astroport with minimal changes to core protocol code
(wonder how this work, can u make your own formula)
Constant Product Pools
Good choice for tokens with high volatility
Where traders are more likely to speculate and act on external information despite high slippage
Rx * Ry = K
Rx and Ry are reserves of token X and Y
Selling quantity of delta x
(Rx + delta x)(Ry - delta y) = K
Stableswap invariant
When prices are similarly pegged, better to use stableswap invariant
4A(Rx+Ry) + D formula, resulting in a constant price ∆x / ∆y = 1.
Middle ground between constant product and constant price
Uses amplification parameter A that determines how close the stableswap curve should be to the constant product curve
Amplification value of 0 (A=0) results into constant product pool algorithm; higher values of A push the curve closer to the constant price curve, resulting in lower slippage for exchange rates close to 1:1
Works by combining terms to represent the constant product and constant sum formulas
Use constant D to represent the total number of tokens in the pool when token X and token Y have equal price
4A(Rx + Ry) + D = 4AD + (D^3)/(4(Rx*Ry)

Astro Generators
LP face decision on which platform to provide liquidity
Astroport addresses this with proxy based smart contracts that enable simultaneous dual farming of ASTRO tokens and the governance tokens of any other community seeking to distribute its governance token to LPs for a specific pool
Enable LP to claim governance awards of both ASTRO and tokens of extrinsic DeFI communities associated with the pool (MIR or ANC)
Dual distribution
Benefit both Astroport and third parties
Receive more tokens than they would have
Dual liquidity mining
Rather than depositing their Astroport-LP tokens in a third party staking contract, LPs can deposit their Astroport LP tokens into ASTRO generators
Astro generators forward the LP tokens into relevant third party staking contracts
LPs accrue both set of grants (both ASTRO and third party protocol tokens)
LP wishes to claim tokens
Astro generator transfers the appropriate amount of ASTRO to the LP
Proxy contract claims the third party token due to the LP and transfers them as well
Rewards are enabled with the simple proxy contract; they need not alter their own governance token staking contracts or frontends.

Astral Assembly
Governance, their take on DAO
xASTRO, vxASTRO have the power to propose and make binding votes on parameter changes, smart contract upgrades and treasury disbursements
Astroport will put governance in the hands of xASTRO and vxASTRO holders.

Lock Drop
Phase 0, Phase 1, Phase 2
0 -> airdrop to people who are active on Terra (25M)
1 -> Terraswap LPs who migrate LP tokens from Terraswap to Astroport (75M)
2 -> who supply ASTRO and/or UST to the ASTRO/UST pool (aka the “ASTRO-UST Bootstrapping Pool”) on Astroport (10M)
During phase 2 you will have the option of preclaiming your airdrop to lock ASTRO tokens in the ASTRO-UST liquidity bootstrapping pool to receive additional ASTRO tokens
Airdrop recipients will be able to claim their tokens for up to three months after the launch of Astroport
Able to migrate and lock Terraswap LP tokens to Astroport in exchange for a drop of ASTRO tokens (vampire attack)
LP tokens form the base liquidity for Astroport in the initial stages

The Migration (Phase 1 Vampire Attack)
Liquidity must be locked on Astroport for atleast two weeks, longer you lock your liquidity on Astroport, the more ASTRO tokens you will receive
First five days of Phase 1, you can decide how many LP tokens you want to lock
you can also leave the liquidity in the pool to continue accruing trading fees and dual rewards.

The Pools
Ten pools will be eligible to migrate and earn ASTRO (basically UST-other Terra tokens)
Split a total of 75 million ASTRO tokens
Phase 2, you can commit ASTRO and/or UST to the ASTRO-UST bootstrapping pool. Participants receive ASTRO tokens for locking liquidity.

The Discovery (Phase 2)
Any deposit will get converted into ASTRO-UST LP tokens
If you deposit just one asset, it will get converted to a 50/50 share of the pool when the pool closes. 
This means that when you redeem your LP tokens, you will receive back a combination of ASTRO and UST. Cannot withdraw your ASTRO tokens once they’ve been deposited during Phase 2 and until your lock expires..
Total of 10 million ASTRO tokens will be distributed to LPs who commit ASTRO or UST to the pool
Allocation will be determined by the total percentage of the pool you own when the snapshot is take at the end of phase 2
LP position will also receive ongoing ASTRO incentives and fees during the lock.
After launch, your ASTRO-UST deposits will unlock linearly over three months.

Separate Trading fees
Constant Product Pools -> 0.3%
0.2% to LP
0.1% to Astral Assembly
Purchase ASTRO from liquidity pools, goes into xASTRO and vxASTRO staking pools
Stableswap Invariant Pools -> 0.05%
0.025% to LP
0.025% to Astral Assembly
Purchase ASTRO from liquidity pools, goes into xASTRO and vxASTRO staking pools
Special Constant Product Pool -> 0.3%
0.28% to LPs
0.02% to Astra Assembly
No ASTRO emissions

ASTRO
Two uses
Staking ASTRO in the xASTRO pool: get governance power, xASTRO tokens, and share of trading fees (used to buy back ASTRO rebasing system)
Locking xASTRO in the vx ASTRO pool
To receive vxASTRO points, amplify governance power, receive an additional share of trading fees (funded with other half of the Astral Assembly’s share of trading fees) access other benefits such as boosted liquidity mining rewards
veCRV model
User locking 100 xASTRO into the Astral Assembly’s vxASTRO pool
Locked for 2 years user receives 200 vxASTRO
Locked for 1 year user receives 100 vxASTRO
Linear decrease
Can always extent their lockup for up to 2 years
Keep in mind that as a user’s vxASTRO allotment declines, they will still hold the underlying xASTRO so they can continue to collect fees and participate in governance
(reduced rate vs locking xASTRO for vxASTRO)
xASTRO holders to participate in governance
But empowers users who lock up their xASTRO tokens
Convicted ASTRO holders will be able to amplify their influence and commitment to the system

Astro Utility
Each of these tokens entitles their holders to different degrees of fee-sharing and governance power. While the xASTRO token represents one level of commitment within Astroport ecosystem, vxASTRO more commitment and higher governance power and fee-sharing

xASTRO Utility
Fee-share: xASTRO transferable token that automatically accrues additional ASTRO from Astral Assembly’s portion of trading fees, proportional on the number of xASTRO in existence.
While adjustable by ASTRO governance, initially half of the Astral Assembly’s portion of trading fees will accrue to xASTRO holders.
Governance: xASTRO holders will be able to vote on and submit proposals to Astroport governance, including incentives allocation voting (more on this in the following section).

vxASTRO Utility
vxASTRO not transferable
Can be thought of as points that allow users to leverage their xASTRO to access additional benefits within Astroport ecosystem
Increase governance power, additive
More protocol fees: The other half of the Astral Assembly’s portion of trading fees will be distributed pro rata to all vxASTRO holders based on the number of vxASTRO then in existence. 
Receive some of the Astral Assembly’s portion of trading fees that flow to the xASTRO pool(basically acting as xASTRO holders)
ASTRO generator boost
vxASTRO holders who are also Astroport pool LPs mining ASTRO through ASTRO Generators will receive a boost of 1x to 2.5x to their share of ASTRO emissions
LP can receive additional governance power if they lock xASTRO to become vxASTRO holders and vxASTRO holders receive additional governance power if they provide liquidity to pools
Final boost for LP/vxASTRO holder will depend on several factors, including: the percentage of LPs engaged, which liquidity pool, overall liquidity in the pool, how much vxASTRO the user holds, and the total amount of all vxASTRO holders in existence.
Earning WEight = Min(Liq. Provided, 0.4 * liq Provided + 0.6 * Pool Liq * vxASTRO/(Total vxASTRO))
Boost = (2.5*earning weight)/(liq provided)

Staking Dynamic and Benefit
ASTRO -> xASTRO -> vxASTRO
xASTRO
Liquid token can be transferred and used as collateral
Protocol revenue used to buy ASTRO and distribute to xASTRO holders
Governance
vxASTRO
Increased governance power
More protocol revenue used to buy ASTRO and distribute to xASTRO holders
Boosted rewards on generator boost

Astro Generator aims to distribute power to liquidity providers
Can adjust through voting process -> how much and where ASTRO gets distributed
xASTRO/vxASTRO holders vote in favor of weighting ASTRO Generator rewards towards the liquidity pools for their respective community tokens.
Voting period last 2 weeks, vote for multiple pools with different weights for each pool
xASTRO and vxASTRO votes in each two week voting period decide ASTRO generator emissions for the 2 week periods
Automatically assigns the same voted weight for each user so that you that no one is “inactive”


ASTRO Token Allocation
1 billion ASTRO max supply
49% to LP over 69 years
11% to Locked Drop (airdrop)
10% Astroport DAO Pool
30% Builders 3 year lock up period
Grace period of atleast 60 days between launch and activation of Astral Assembly, to allow for sufficient distribution of the token

APY calculation
Baseline Assumption
4,656,810 blocks/year ~= 12,758 blocks per day.
UST value of Astroport swap usually less than 1 min behind realtime
Swap Fee Returns
12,758 * LP fee (.2% for CP and 0.025% for stableswap) to get 24h fees, then multiply by 365 and divide by the pool liquidity to get APR value
APY = ((1+(daily fees)/(pool liquidity))^365) - 1
ASTRO and 3rd Party Rewards
3rd party rewards sent to corresponding generator proxy contract
Total Rewards (APY)  = ((1 + (sum of all 24 fees and token rewards) / (pool liquidity))^365) - 1.
APR calculated by extrapolating from 7 day period of fees + protocol rewards



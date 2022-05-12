# Asset Price Movements and Liquidations

Synthetic Asset protocols ie DAI, Mirror, Synthetix, Yeti
- Needs oracle that provides prices of collateral asset
- Can mint new DAI when `O_t * a * c >= b` where O_t is the oracle price, a is the amount of collateral, c is the collateral factor(usually less than 1), b is the amount of synthetic tokens
- If the oracle updates and cause the above invariant not to hold, a liquidation of the position is allowed(This is very bad for the person who took out their position!!)

For the multi-collateral case, O_t, a, c, b are all vectors and the following invariant is used 
`sum over collaterals(O_t * a * c) >= b`

If you have a mixture of two assets(ie 50% eth and 50% bitcoin), as long as the oracle prices are not perfectly correlated, there is a smaller chance that the collaterals move in a way to get you liquidated.

### Arguments for not doing this?
Might be hard to set the parameters for `O_t, a, c, b`, but gauntlet already does this for collateral ratios on compound and aave


### Why is everything correlated?
Things are empirically correlated and the why doesn't really matter.

People who invest in an asset class want to be diversified, if the bitcoin in their portfolio goes up, they want to rebalance to stay diversified so they sell part of their bitcoin and buy eth, causing the eth price to go up.

### Questions
What's the intuition behind multi-collateral synthetics being less likely to liquidate? What about non-stablecoins?

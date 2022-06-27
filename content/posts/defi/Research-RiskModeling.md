# Lending
Max Loan Size
Supply vector x collateral ratio * prices
Asset | Collateral factor
ETH   | .825
DAI   | .825
WBTC  | .7

# Mechanics of Liquidation
When liquidation is allowed, liquidators repay the borrowed amount and gets a portion of the collateral + some bonus. THe liquidator immediately market dumps the collateral + bonus.

There's a routing library in Julia that Bain cap wrote(doesn't support curve/univ3)

## Cascading Liquidation Feedback Loop
Liquidations -> Market Dump -> Price Drop -> More Liquidation

Algorithm:
1. Input current state of lending markets, exchanges, prices
While True:
    Check for liquidations
    If liquidations:
        Route them over exchange graph, update exchange states, update prices to post dump prices

Why?
Allows us to calculate structural component expected loss vector and variance covariance matrix based on actual state of the chain
Allows us to better understand price trajectories. Can sell to people who care
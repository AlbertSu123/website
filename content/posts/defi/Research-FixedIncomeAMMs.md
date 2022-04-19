# Fixed Income AMMs

How do we design an AMM for a fixed yield product?

- As the maturity approaches, the principal get closer and closer to a dollar since when it matures, you can redeem it for a dollar.
- Interest rates tend to be more stable than asset prices, thus the curve should be flatter than `x * y = k`

## Yield space AMM

`x^(1-t)+y^(1-t)=k`
Can chop off half of reserves since zero coupon bonds are always cheaper

- can shift the decay rate to non-linear decay rate
- if there are multiple competing zero coupon bonds(same asset but different)
- zero coupon bond on dai, zero coupon bond on usdc, zero coupon bond on usdt deposited into stableswap AMM, then take the LP token and put that into our `x^(1-t)+y^(1-t)=k` AMM
- For multiple maturities, you can create a nested AMM structure to trade between 3 month, 6 month, and 9 month maturities
  - create a 3 month, UST Pool, then take the {3 month, UST} LP token and pair it with the 6 month principal token {6 month, {3 month, UST}}, then take the {6 month, {3 month, UST}} LP token and pair it with the 9 month principal token to create a 3 layer nested AMM {9 month, {6 month, {3 month, UST}}}
    - Wouldn't this have less liquidity for later expiries?
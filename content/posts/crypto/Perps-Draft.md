
## Delta

What is the delta of holding 1 unit of the underlying asset?
1

How to generate high delta in DeFi
- Lending Markets
    1. Deposit 100 ETH in lending protocol -> borrow 75,000 USDC
    2. Swap 75,000 USDC for 75 ETH
    3. Deposit 75 ETH in lending protocol -> borrow 60,000 USDC
    4. Swap 60,000 USDC for 60 ETH
    5. Rinse and repeat
- Perps

How do you calculate the max leverage you get from lending markets?
- Use the geometric series `1/(1-C)` where C is the collateral ratio

How do you use lending markets to lever short?
1. Deposit USDC into lending protocol -> borrow ETH
2. Swap ETH for USDC
3. Deposit USDC into lending protocol -> borrow ETH
4. Swap ETH for USDC

Futures

## Cash settled futures
- Traditional futures are an agreement to deliver commodity at a fixed price on a future date
- Cash settled futures eliminate delivery but instead settle in cash

## Funding Payments
- The payoff of a future is the market price at maturity minus the price agreed upon in the futures contract
- If the price of the underlying asset increases, the contract buyer(long) makes money
- If the price of the underlying asset falls, the contract seller(short) makes money

## Perpetual futures
- A series of rolling futures contracts.
- Each funding period(around 1 hour), shorts pay longs based on `price today - price on contract`
- Instead of closing your position, your position is rolled over to the next period
- TWAP implementaton(binance, ftx)
    - shorts pay longs according to `24 hr twap today - 24 hr twap yesterday/24` every hour

## Nonlinear Perps

### Squeeth
Instead of funding `payments = index price - market price`, `funding payments = index price ** 2 - market price`
- What's the delta for squeeth? Take the derivate of the funding payment, ie `2x`
- How do we combine squeeth with a standard perp to make it delta neutral?
    - var(x) = E[X^2] - (E[X])^2
    - can construct the crab strategy, f(x)  = x^2 - k^2

Why do people buy leveraged products?
- Producers want to be short, Consumers want to be long
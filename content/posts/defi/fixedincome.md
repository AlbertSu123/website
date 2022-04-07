# Nauticus

Zero coupon bond: Worth 1 dollar at some point in the future. It is called a zero coupon bonds because there is no discount, you can see the fixed income rate by looking at how many zero coupon bonds you can buy. 
> If you can buy 102 zero coupon bonds that are redeemable in one year for 100 dollars, your risk free rate after 1 year is 2%
> Equivalent to zero coupon bond

## Why Fixed Income?
- Variable yields means uncertainty with to what your portfolio value will be in the future
- Fixed income is powerful when applied over multiple rates over a long time period. The randomness of the rates leads to a log normal distribution with a Fat tail, and nothing in this fat right tail can be used by institution.
    - ie pension fund only needs to pay back pensions, the long tail is not useful if they have already achieved the returns. Thus, I can sell this long tail and get some more guarenteed returns
    - Look into central limit theorem for how the distribution is generated

## End Users
- Institutions, people who want higher expected yields
1. Institutions observe the price of the zero coupon bond
2. Nauticus does financial engineering to get yield + principal tokens(see element finance) 
3. Institutions buy the zero coupon bond at a certain price. They wait 1 year or however long then redeem it

There must be some people who are willing to take on additional risk to get higher expected returns
Will likely be complete overlap between fixed income product and people that buy protection

## How does the product work?
Take yield bearing token aUST(anchor UST) and split it into paUST(Fixed Yield) and yaUST(Random yield).
The paUST is always worth 1 dollar at expiration. The yaUST's value depends on what the Anchor yields look like between creation and expiration. Higher anchor yields = higher yaUST value 

1. Users can deposit UST
2. Get aUST 
3. Deposit aUST into fixed income vault
4. Nauticus splits aUST into paUST(redeems to 1 UST at expiration) and yaUST(variable yield at expiration)

If you want to get fixed income, sell your yaUST and buy paUST in a DEX
If you want higher expected yields, sell your paUST and buy yaUST in a DEX

## Questions?
1. How do we solve the fungibility problem?
Treat them as different assets and add a market maker between them. As time to maturity approaches, the market maker should shift from a constant product market maker(x * y = k) to a stableswap (x^3 + y^3 = k)
Can also do nested AMMs
2. If I buy now and the expiration period is 6 months from now and my friend buys tomorrow and the expiration period is 6 months from tomorrow, how do we manage the UX?
All 6 month fixed yields expire on the same day
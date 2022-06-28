---
title: Options
date: "2021-12-30T23:46:37.121Z"
template: "post"
draft: false
slug: "borrow"
category: "DeFi"
tags:
  - "Notes"
  - "DeFi"
  - "options"

description: "A quick primer on options"
socialImage: "/media/image-2.jpg"
---

## Options

Why do we need a characteristic function and how does it relate to Options?
Options + probability are deeply related. If you know characteristic function of the option, you can compute the option price.

Benefit of characteristic functions: Allows for more flexibility in pricing risk, volatility. Increases flexibility with regards to pricing.
"Correlation structure is easier to work with in characteristic function" - Prof Resnick

## Characteristic function

Motivation: This stuff can be calculated on chain using Fast Fourier Transform if we have a characteristic function
Characteristic function: Alternative way to express info about random variable

Recall cumulative density function (Probability that random variable is less than a given value)
E[e^(itX)] - corresponds to the fourier transform, which is a good way to model things

Can express characteristic function as sum of the moments? E[X^n] term ~ price of a power future with power n.
The price of a power perpetual implies something? <- not sure what it implies

Sum of two random variables => product of characteristic functions
^ What's the use case here?

Delta is useful for market makers - If we can compute delta easily, we can check things on chain?
^Spoiler alert, we can using fast fourier transform

Two sides: uninformed flow + market makers on options(PhD physicist/math/stats)

- If you are little boi and make options flow and sell via black scholes, jump is gonna rek you with diffusive, lattice, monte carlo bullshit.
- When you price something wrong, they can arb you immediately and this is hard to fix since you wrote this in a smart contract
- This makes on chain options difficulty

## Questions?

- What will be computed on chain and what will be computed offchain?
  We should implement characteristic function as simpler function on chain using fft(divide and conquer algo- O(nlog(n))

- What is N in O(N log(N))?
  Can be tuned, higher it is better the approximation, similar to resolution.

- What is an FFT?
  https://www.youtube.com/watch?v=spUNpyF58BY
  Thanks matt :D

- What other protocols have power futures?
  Squeeth, Tracer, https://zeta.markets/. The later two aren't really power futures though, they are just multipliers right?

- Are these markets priced by spot? Do they reflect alpha in spot?
  Squeeth is underfunded, so people buy squeeth to hedge volatility future. Selling squeeth gives you positive EV, not enough liquidity in market for other side. No one wants to sell volatility. If there was a liquid options market, then you can hedge squeeth. In theory you can buy options and sell squeeth, and volatility, skewness hedged and set as options market maker.

## Quanto Derivatives

Derivative that is settled in a different reference currency.

- Anchor protection settled in usdc vs anchor protection settled in ust
  Why do the premiums cost a different amount.

Why isn't there another instrument that could close the gap in premiums between these products?
You are paying for more protection

When the euro collapsed, greece, italy, spain defaulted. Two types of cds, one denominated in euros and one denominated in usd. The usd premiums are much more expensive than the cds premiums denominated in euros because if greece defaulted on their loans, the euro is more likely to collapse as well

Effect of lower price of euros

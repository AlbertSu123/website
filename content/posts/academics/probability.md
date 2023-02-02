---
title: Probability in Electrical Engineering and Computer Science
date: "2023-01-10T23:46:37.121Z"
template: "post"
draft: false
slug: "probability"
category: "Misc"
tags:
  - "List"
  - "UC Berkeley"

description: "Real applications of probability"
socialImage: "/media/image-2.jpg"
---

Introduction to Probability, 2nd Edition
Follows the course, can borrow from engineering library

TODO:
Set calendar for homework self grades and resubmission time
Set midterm time on calendar
Choose discussion time
Countable vs uncountable

## Probability: Math + a way of thinking about uncertainty

Humans are naturally good at thinking about probability. For example, you can easily walk down a crowded street and estimate what path allows you to avoid bumping into people. However, humans are pretty awful at turning those innate estimates into quantifiable numbers. The goal is to take the natural probability estimation into something that a robot can understand.

**Quantify + Model**

Inference. Humans are also really good at taking in noisy observations and infer what is really happening. 

How do I generate some image from AI? Take a natural image and repeatedly add noise to it. You then end up with an image of pure noise. The idea behind stable diffusion and dalle-2 is to reverse this process. We generate a bunch of noise, then reduce noise until we end up with a image.

Modern theory of probability has an axiomatic starting point. Before the 1930s, probability was a heuristic field.
All of probability are logical deductions from the starting point.

*Probability space:* A probability space (Ω, F, P) is a triple. 
Ω is a set, the sample space. 
F is a family of subsets of Ω, called events
P is a probability measure

Rules: 
  - F is a σ-algebra, it contains Ω itself. F is closed under complements and countable unions. 
    - If A ∈ F, then any of A's complements belong to F 
    - countable unions: If A1, A2, ... ∈ F, then the union of Ai ∈ F
    - By DeMorgan's Laws, this implies that F is also closed under countable intersections
  - P is a function that maps events to a number between 0 and 1. `P: F -> [0,1]`
    - Probability measures must obey Komogorov Axioms
      1. `P(A) >= 0` Probability of any event must be non-negative
      2. `P(Ω) = 1` The probability of any outcome occuring must be equal to 1
      3. σ-additivity aka countable additivity. If A1, A2, ... is a countable sequence of events and disjoint, then `P(U) = ΣP(Ai)`
        - counterexample: `P = Unif(0,1)`, then P({x}) = 0 for x ∈ [0,1]. If `1 = P([0,1])`, then `Σ P({x}) = 0` <- Dive into this later

Examples of the Above Rules

1. Flip a coin with bias P(Heads with probability p, Tails with probability 1-p). 
    - Ω = {H, T} The sample space is just heads and tables
    - F = 2 ^ σ = {H, T, Ø, {H, T}} These are all subsets of σ
    - P(H) = p, P(T) = 1-p, P(Ø) = 0, P({H, T}) = 1 Why are Ø and {H, T} here?
    - We then want to create an experiment of n "independent" flips. Is our vocabulary rich enough for this? no
    - Ω = {H, T}^n, F = 2^Ω, P(f1, f2, ... , fn) = p^{number of flips fi = H} * (1-p)^{number of flips fi = T}
    - There can be multiple answers for a probability space. It just needs to be rich enough to capture all the probabilities

2. Ω now equals all possible configurations of atoms in the universe. How do we represent this?
    - Let A = {set of configurations that lead to flip heads}
    - Let B = {set of configurations that lead to flip tails}
    - F = {Ø, A, B, Ω = {A U B}}
    - P(A) = p, P(B) = 1-p, P(Ø) = 0, P(Ω) = 1

Note: The probability space is usually implicitly described, except for HW 1.
Lol

## All of the rules of probability you are likely familiar with are consequences of the three axioms.
Ex: If `A ∈ F`, `P(A^C) = 1 - P(A)`. The probability of A's complement is 1 - probability of A.
- Since F is σ algebra, then A^C ∈ F. 
    - A U A^C. A and A complement is a disjoint union equal to Ω
    - 1 = P(Ω) Using axiom 2
    - P(A U A^C) = P(A) + P(A^C) Using Axiom 3

Ex: If A and B are events and A is a subset of B, then `P(A) <= P(B)`
B = A U (B \ A). B is equal to the disjoint union of A and B minus the elements of A
P(B) = P(A U (B \ A)) => P(A) + P(B\A)  Using axiom 3
P(A) + P(B\A) >= P(A) Using axiom 1

Ex: If A and B are events, then P(A U B) = P(A) + P(B) - P(A ∩ B). Aka inclusion exclusion principle
Proof: `AUB, A∩B ∈ F`
A U B = B U (A \ B)
P(A U B) = P(B) + P(A\\(A∩B)) Using axiom 3
P(B) + P(A\\(A∩B)) = P(B) + P(A) - P(A ∩B) Using Axiom 3

Ex: Ω = countable set
F = 2 ^ Ω, each individual is an event {w} ∈ F, for all w ∈ Ω
P(A) = Σ for all A ⊆ Ω
This is a discrete sample space.

Ex: Law of total probability. If A1, A2, ..., partition Ω, then (Ω = U Ai). Mutually exclusive, collectively exhaustive.
Then, for any B ∈ F, then `P(B) = Σ P(B ∩ Ai)`
B = U(Ai ∩ B) then apply axiom 3

The goal of probability is to take complicated things and use that to approximate them into multiple simpler events.

Mathematicians: Given a probability space(model), what can I say about outcomes of experiments that derive from this model?
Statistician: Given outcomes, how do I choose a good model(probability space)?
Engineers: Given a real world problem, how do I choose a model that captures the essence of the model and then use the model to draw insight.



# Lecture 2: Conditional Probability, Independence, Random Variables

**Conditional Probability:** If `B ∈ F` and `P(B) > 0`, then conditional probability P(A|B) = P(A ∩ B) / P(B)

_Intuition_: Probability A occurs given we know that B occurred

Formal Definition: P(.|B) gives a restriction of our model (Ω, F, P) to those samples in B
If `(B, F|B, P(.|B))` is a probability space itself, where `F|B = \{A ∩ B: A ∈ F\}`

_Ex._ If A1, A2, ... ∈ F, P(Ai) > 0, A partion Ω
`P(B) = Σ P(B ∩ Ai) = Σ P(B|Ai) * P(Ai)`

## Bayes Rule

Sometimes, it's easy to express P(B|A), but we are really interested in P(A|B)
Let A be the state of the experiment, B be the observation data

P(A|B) - given the observation, we want the state. This is the task of inference.

Suppose we forgot Bayes Rule, let's try to recreate it from definition of conditional probability
`P(A|B) = P(B ∩ A) / P(B) = P(A) * P(B|A) / P(B)`

_Ex._ Suppose we have the following model: 85% of students got a pass, 15% of students got a no pass. 60% students that got passes went to lecture, 40% of students did not go to lecture. 10% of students that got no passes went to lecture, 90% didn't go to lecture.

We want P(NP | N) / P(NP | Y). How much more likely are you to NP when you don't attend lecture vs when you do attend lecture?
P(NP ∩ N) / P(NP ∩ Y) \* P(Y) / P(N)

_Ex._ Suppose we roll two dice and the sum is 10. What is the probability roll 1 was = 4?

```
B = \{Sum of rolls = 10\}
A = \{first roll is 4\}

P(A|B) = P(A ∩ B) / P(B)
       = P({First roll is 4, sum of rolls is 10}) / P({sum of rolls is 10})
       = P({first: 4, second: 6}) / P({(4,6), (5,5), (6,4)})
       = (1/36) / (3/36)
       = 1/3
```

### Conditioning also allows us to usefully decompose intersections of events.

_Ex._ Consider event A1 ... An
P(∩ A) = P(A<sub>1</sub> | ∩<sup>n</sup><sub>i = 2</sub> A<sub>i</sub>) P (∩ A<sub>i</sub>)

_Ex._ Given n people in a room, what is the probability that more than 2 people share a birthday?

A<sub>i</sub> = \{person i does not share a birthday with any of the people j = 1, ..., i-1\}

P(A<sub>i</sub> | ∩ <sup>i-1</sup><sub>j=1</sub> A<sub>j</sub>) = (365 - (i-1)) / 365

This is because the person needs to land in one of the days not in the 365.
P(no shared birthdays) = P(∩ A<sub>i</sub>)

Use 1-x <= e^-x.
Approximate using taylor series

Let n = 23 - there are 23 kids in a class.
P(shared birthday) >= 1 - e^((i-1)/365))

## Independence

Events A, B are independent if P(A ∩ B) = P(A) * P(B)
Disjoint events are not independent
*Note:\* in a special case where P(A) > 0: A,B: dependent <=> P(B|A) = P(B)

In general, collection A<sub>1</sub>, A<sub>2</sub>, ... ∈ F are independent events if P(∩<sub>i ∈ S</sub> A<sub>i</sub>) = ∏ <sub>i ∈ S</sub> P(A<sub>i</sub>) for all finite sets of indices S

If A<sub>1</sub>, A<sub>2</sub>, ... ∈ F are independent, then B<sub>1</sub>, B<sub>2</sub>, ... are independent, where each A<sub>i</sub> = B<sub>i</sub> or B<sub>i</sub><sup>C</sup>
Intuitively, we assume that knowing A means we know A's complement

∩<sub>i=2</sub><sup>n</sup> A<sub>i</sub>
∏P(Ai) = P(∩ Ai) = P(∩) + P(A1)
(1- P(Ai)) ∏ P(Ai) = P(A1^C ∩ ∏<sub>i=2</sub>Ai)
This shows A<sub>1</sub><sup>C</sup>, A<sub>2</sub>, ... , A<sub>n</sub> are independent
Therefore, B<sub>1</sub>, B<sub>2</sub>, ... , B<sub>n</sub> are independent

### Conditional Independence

Often times we have two events that we think of as independent but aren't. There might be a confounding variable C
If A,B,C are such that P(C) > 0 and P(A∩B|C) = P(A|C) P(B|C), then A,B are said to be conditionally independent given C

Consider two coins with bias p!=q
Pick a coin at random, and flip twice. H(i) = Event that flip i is heads
Are the two coinflips independent? No. If p is 1 and q is 0, then we know what the next coin flip is given our first coin flip.

P(H<sub>i</sub>) = p + q / 2
P(H<sub>1</sub> ∩ H<sub>2</sub>) = p<sup>2</sup> + q<sup>2</sup> / 2 != P(H<sub>1</sub>) P(H<sub>2</sub>) = (p+q)<sup>2</sup> / 2
C = \{pick coin p\} H<sub>1</sub>, H<sub>2</sub> conditionally independent given C

TODO:
Set calendar for homework self grades and resubmission time
Set midterm time on calendar
Countable vs uncountable

# Random Variables
Password: Teapot

*Def:* A random variable X is a function X: Ω -> R. It implies it is valid to write things like P(X <= 3). P(X <=3) is shorthand for P({ω: X(ω) <= 3})
Random conditions have a measurability condition

Often times we want to compute more complex probabilities, such as P(α < X < β).
{ω: X(ω) < β} = U<sub>n >=1</sub>{ω: X(ω) < β -1/n} ∈ F
{ω: X(ω) > α} = {ω: X(ω) <= α}<sup>C</sup> ∈ F
P(X ∈ B) for pretty much any B(subset of R) that you want
Here, the technical name for B is *Borel sets*

Another consequence of the definition of random variables: 
If X,Y are random variables on (Ω, F, P):
- X+Y is a random variable
- X*Y is a random variable
- |X|<sup>p</sup> is a random variable

## Probability Distributions
For any random variable X on probability space (Ω, F, P), we can define its distribution(aka its "law") L<sub>x</sub> via L<sub>x</sub>(B) := P(X ∈ B), B ∈ R
For example, L<sub>x</sub>({x}) = P(X=x)

The histogram of values that a random variable would take.

Similar to a probability measure.

In practice, we often describe our model for experimental outcomes in terms of distributions. Given this, I can always construct a probability space and random variable X that has this distribution.

Ex: Given distribution L, we can consider probability space (R, B, L) and random variable X(ω)=x, ω ∈ Ω = R
Distribution of X

Important Class of Random Variables;
*Discrete Random Variables*: A random variable that takes countably many values
Ex: X = flip of a p-biased coin. This is a bernoulli distribution with probability p

X = roll a dice. This is a uniform distribution over {1,2,3,4,5,6}
X = number of coin flips until I flip heads. Geometric Distribution
X = number of heads in n flips. Binomial Distribution (n, p)

In case of a discrete random variable X, the distribution of X can be summarized by its probability mass function.
P<sub>x</sub>(x) := P({X=x}) = P{ω: X(ω) = x}

By axioms, p<sub>x</sub>(x) >= 0 and Σp<sub>x</sub>(x) = 1

### Joint Distributions
For a pair of discrete random variables X,Y defined on the common probability space (Ω, F, P), their joint distribution is:
summarized by the join probability mass function defined via 
P<sub>xy</sub> = P(X=x, Y=y) = P({ω: X(ω) = x && Y(ω) = y}) = P({ω: X(ω)} ∩ {ω: Y(ω) = y})
We can obtain the "marginal" distributions by P<sub>x</sub>(x) = Σ P<sub>xy</sub>(x,y) using the law of total probability Σ P(x) = 1

## Independent Random Variables
Def: Discrete Random Variables X,Y are independent if their probability mass function factors into their marginals
P<sub>xy</sub>(x,y) = P<sub>x</sub>(x)P<sub>y</sub>(y)

This is equivalent to saying {ω: X(ω) = x}) and {ω: Y(ω) = y}) are independent events for all x,y

Examples of Joint Distributions:
X = {0 if patient tests negative with probability 0.9, 1 if patient tests positive with probability 0.1}
Y = {0 if patient is negative, 1 if patient is positive}

We know the false positive rate of the test is 5%, the false negative rate of test is 1%

Question: What is P<sub>XY</sub>?
I test my patient. The test is either negative(.9) or positive(.1).

## Expectation - Used for discrete random variables
Takes in a random variable, spits out a distribution
For a discrete random variable X on (Ω, F, P), we define its expectation E(X) = ΣxP{X=x} = Σxp<sub>x</sub>(x)

Ex: If X Ω = {0,1}<sup>n</sup>, P({ω}) = p<sup>{number of heads}</sup> (1-p)<sup>{number of tails}</sup>
ie a sequence of independent p biased coin flips

X(ω) = {ω = 9}. <= Number of heads
E[X] = Σ{i : ω<sub>i</sub>= 1}, p<sup>{i : ω<sub>i</sub>= 1}</sup> (1-p)<sup>{i : ω<sub>i</sub>= 1}</sup>

= Σ<sub>k=0</sub><sup>n</sup> (n k) p<sup>k</sup> (1-p)<sup>n-k</sup> = np

The most important property of expectation is that it is linear.
Expectation is an operator, it takes a function and spits out a number, just like an integral.
Just like how integrals are linear, expectation is also linear.

The most important and only property of expectation `E[X+Y] = E[X] + E[Y]`

### Linearity of Expectation
If X,Y random variables defined on a common probability space. `E[aX + bY] = aE[X] + bE[Y]` <= There is no need for expectation to be independent or discrete.

LemmaL E[g(z)] = Σ g(z) p<sub>z</sub>(z)

By the law of the unconscious statistician
E[aX + bY] = Σ(ax + by)P<sub>xy</sub>(xy) = aΣxP<sub>xy</sub>(x,y) + bΣxP<sub>xy</sub>(x,y)

# Sum of Independent Binomials, Variance, Covariance, Correlation Coefficient, Entropy

## General Strategy for Expectation
1. We introduce indicator random variables and express random variable of interest in terms of the indicators, then use linearity of expectation
2. In general, the indicator for event A is denoted by 1<sub>A</sub>(w) = {0 if ω is not A, 1 if ω is A}
   a. 1<sub>A</sub> = Bernuolli(P(A)), E[1<sub>a</sub>] = P(A)

Ex: n people put their hats in a bucket, and each draw 1 hat. Let X = number of people who get their own hat back.
X = Σ<sub>i=1</sub><sup>n</sup>1<sub>A</sub>
A<sub>i</sub> = {gets own hat back}
E[x] = Σ<sub>i=1</sub> E[1<sub>M/sub>]= n * 1/n = 1

Ex: Coupon collector problem
How many on boxes on average do I need to buy before I collect all N coupons
X<sub>i</sub> = number of boxes to buy to get ith coupon, after getting (i-1)<sup>th
R[].

## Tail sum formula: Another way to compute expectation without using linearity of expectation.
For non-negative integer values random value X<sub>i</sub>
E[X] = Σ<sub>k=1</sub><sup>inf</sup>P{x=k}

Proof: write P({x>= k}) = Σ<sub>x>=k</sub> P{x >= k} = Σ<sub>x>=k</sub> P(X=x)

X<sub>1</sub> = Geom(p), X<sub>2</sub> = Geom(p)
M = min{X<sub>1</sub>, X<sub>2</sub>}
P{M>= k} = P{x<sub>1</sub>}, P{x<sub>2</sub>}

IE[M] = Σ P{x<sub>1</sub> >= k} P{x<sub>2</sub>>=k} 
= 1 / (1- (1-p)*(1-q)))

## Variance
Quantitative Notion of variability of X around E[X]
Var(X) = E[(X - E[X])<sup>2</sup>] = E[X<sup>2</sup> - 2XE[X] + (E[X])<sup>2</sup>]= E[X<sup>2</sup>] - (E[X])<sup>2</sup>

If expectation is the price of a stock, variance is the volatility.
Variance is not the only way to measure the variability of a random variable. Another way to measure the variance of a random variable is entropy.
Entropy = Σ P<sub>x</sub>(X) log(1/P<sub>x</sub>(X)) = E[log(1/P<sub>x</sub>(X))]

Unlike expectation, variance is not linear with respect to its arguments. Var(X+X) = ?
Var(X+Y) != Var(X) + Var(Y)
Var(X+Y) 

By definition of variance
= E[X+Y - E[X] - E[Y]]<sup>2</sup>

Open up the square using foil
= Var(X) + Var(Y) + 2 E[(X-E[X])(Y-E[Y])]

## [X - E[X])(Y-E[Y]) = Covariance
Positive covariance means X and Y move in the same direction
If X and Y are independent, covariance is 0

If X,Y are uncorrelated(covariance of X,Y = 0), then Var(X+Y) = Var(X) + Var(Y)

Correlation coefficient: Covariance(X, Y) / sqrt(Var(X)*Var(Y)). It is always between -1 and + 1

Covariance(X,Y) <= sqrt(Var(X) * Var(Y))
Using cauchy schwartz inequality, x<sup>T</sup>y / (||X|| ||Y||) is between [-1, 1]

E[XY] <= E[X<sup>2</sup>]<sup>1/2</sup> E[Y<sup>2</sup>]<sup>1/2</sup>
E[XY] = Σ P<sub>xy</sub>(x,y)<sup>1/2</sup> x * P<sub>xy</sub>(x,y)<sup>1/2</sup> y <= (Σ<sub>xy</sub>P<sub>xy</sub>(x,y)x<sup>2</sup>)<sup>1/2</sup> (Σ<sub>xy</sub>P<sub>xy</sub>(x,y)y<sup>2</sup>)<sup>1/2</sup>

Let X ~ Binomial(n, p)
X = Σ X<sub>i</sub> X<sub>i</sub>Bernuolli(p)
Var(X) = Σ Var(X<sub>i</sub>) = n Var(X<sub>1</sub>)

## Poisson random variables
Poisson means fish in french. Think number of arrivals when you see poisson
If I dip a net into the water and pull it out, a poisson distribution is a good way to checking how much fish I pull out.

X ~ Poisson(λ) λ = rate
P<sub>λ</sub>)(k) = λ<sup>k</sup>/k! e<sup>-λ</sup>, k = 0,1,2,...

Imagine trying to take a bus in berkeley. They are supposed to be at each station every 30 minutes. However, there are times when no buses come for 45 minutes then two buses come in the 46th and 47th minute. You can model the arrival of the buses using a Poisson distribution

Where does Poisson come from? 
Each interval something will arrive with probability p<sub>n</sub> = λ/n. The probability that something arrives in an interval is X<sub>n</sub> ~ Binomial(n, p<sub>n</sub>)

P(X<sub>n</sub>=k) = Binomial PMF = (n k) * (λ/n) * (1-λ/n)<sup>n-k</sup>
= n(n-1)...(n-k+1) *(1/n-λ)<sup>k</sup>* λ<sup>k</sup>/k! (1-λ/n)<sup>k</sup>
as n goes to infinity, this distribution converges to a poisson distribution
λ<sup>k</sup>/k! * e<sup>-k</sup>

# Continuous Distributions, Continuous Random Variables
Password: Mirror

## Conditional Distributions
`P(A|C) := P(A ∩ C) / P(C)`

When X,Y are discrete: their joint pmf is P<sub>xy</sub>

P<sub>x|y</sub>(x|y) := P<sub>x|y</sub>(x,y) / P<sub>y</sub>(y) = P({X=x} ∩ {Y=y}) / P({Y=y})

Since P<sub>x|y</sub> is a pmf for each y with `P<sub>y</sub>(y) > 0`, we can take expectation of X with respect to it.

This is defined as conditional expectation
E[X|Y=y] := Σ<sub>x⊆X</sub> xP<sub>x|y</sub>
Once we have the value of Y, what is the value of x?
We usually just write E[X|Y] to denote Σ<sub>x⊆X</sub> xP<sub>x|y</sub> evaluated at random variable Y. Thus, the conditional expectation is itself a random variable since it is a function of the random variable Y.

## Tower Property
Most important property of Conditional expectation: 
- For all functions f, we can say `E[f(Y)X] = E[f(Y)*E[X|Y]]`
  - E[f(Y)X] = Σ<sub>y</sub> p<sub>y</sub>(y)f(y) * Σ<sub>x</sub>P<sub>x|y</sub>(x|y)
  - E[f(Y)X] = Σ<sub>x,y</sub>P<sub>xy</sub>(x,y)*f(y)x

Example: Iterated Expectation
E[X] = E[E[X|Y]]
We get this using the tower property by setting f(Y) = 1

Example: Let N>= 0 be an integer valued random variable. Let's flip a fair coin N times, and let X = the number of heads.
There are two sources of randomness, the number of flips and whether we get heads or tails.
E[X] = E[E[X|N]]
We can fix the number of coin tosses, and since half of coin tosses are heads
E[X|N] = N / 2
Substitute
E[N/2]
Use linearity of expectation
1/2 E[N]

Var(X) = E[Var(X|N)] + Var(E[X|N])
Using the same substitution as above of E[X|N] = 1/2
Use binomial theorem for E[Var(X|N)] = E[N/4]. Variance of a binomial = ?
Var(X) = E[N/4] + Var(N/2)
Var(X) = 1/4 * E[N} + 1/2 * Var(N)

Recall Var(X) = E[X-E[X]<sup>2</sup>]
Definition: Conditional Variance of X given {Y=y}
Var(X|Y=y) = Σp<sub>x|y</sub>(x|y)(X-E[X|Y=y])<sup>2</sup>

Just like conditional expectation, we write Var(X|Y) to denote the random variable evaluated at Y.

Theorem: Law of total variance

Var(X) = E[Var(X|Y)] + Var(E[X|Y])

Proof:

Var(X) = E[X<sup>2</sup>] - (E[X])<sup>2</sup>

= E[E[X<sup>2</sup>|Y]] - (E[E[X|Y]])<sup>2</sup>

= E[Var(X|Y)] + E[E[X|Y]<sup>2</sup>] - (E[E[X|Y]])<sup>2</sup>

= E[Var(X|Y)] + Var(E[X|Y])

Now let's start with Var(X|Y=y)

= E[X<sup>2</sup>|Y=y] - (E[X|Y=y])<sup>2</sup>

= E[Var(X|Y) + E(X|Y)<sup>2</sup>] - (E[E[X|Y]])<sup>2</sup>

## Continuous Random Variables and Continuous Distributions
Note: Random Variables need not be discrete or continuous or combinations thereof
For a random variable X, we can always define its cumulative distribution function abbreviated CDF via 

F<sub>x</sub>(X) := P{X<=x}

1. F<sub>x</sub> is nondecreasing 
2. F<sub>x</sub>(x) -> {0 if x -> -inf, 1 if x -> inf}
3. F<sub>x</sub> is continuous from the right

Definition: A random variable X has continuous distribution if there exists a function f<sub>x</sub> such that 

F<sub>x</sub>(x) = ∫<sub>-inf</sub><sup>x</sub>f<sub>x</sub>(u)du

f<sub>x</sub> is called the density of X aka a pdf(probability density function)

For f<sub>x</sub> to be a density, it just needs to be non-negative and the integral of the density to be equal to 1, since total probability = 1
f<sub>x</sub> >= 0
∫f<sub>x</sub>dx = 1

### Continous Random Variables
Good for modeling analog signals from the real world.
- time we wait until a bus arrives(Exponential Distribution)
- Voltage across resistor(Gaussian Distribution)
- Phase of a received wireless signal(Uniform Distribution)
  
Continuous distributions usually described by their density.
X ~ Uniform(a,b) = {1/(b-a) a <= x <=b, 0 otherwise}
X ~ Exp(λ) = {λe<sup>-λx</sup> if x>=0, 0 otherwise}
X ~ N(µ,σ<sup>2</sup>) = 1/sqrt(2pi*σ)exp(-(x-µ)<sup>2</sup> / 2σ<sup>2</sup>)

### Jointly Continuous Random Variables
We say X<sub>1</sub>, X<sub>2</sub>, ..., X<sub>n</sub> are jointly continuous if 
F<sub>X<sub>1</sub>X<sub>2</sub>...X<sub>n</sub></sub></sub>(x<sub>1</sub>x<sub>2</sub>...x<sub>n</sub>) = P{X<sub>1</sub><=x<sub>1</sub>, X<sub>2</sub><=x<sub>2</sub>, ... X<sub>n</sub><=x<sub>n</sub>}

= ∫∫ F<sub>X<sub>1</sub>X<sub>2</sub>...X<sub>n</sub></sub></sub>(x<sub>1</sub>x<sub>2</sub>...x<sub>n</sub>)du<sub>1</sub>du<sub>2</sub>...du<sub>n</sub>

Example:
Let a dart land uniformly at random on a 2d dartboard of radius r.
The joint density will be flat over the dartboard

{1/pi*r<sup>2</sup>, x<sup>2</sup> + y<sup>2</sup>}
Independence: Random Variables X,Y are independent if F<sub>xy</sub>(x,y) = F<sub>x</sub>(x) * F<sub>y</sub>(x)
If X,Y are continuous and independent, their joint density is the product of the marginals

### Expectation of continuous random variables
Same as discrete case, replace sums with integrals
E[X] = ∫x f<sub>x</sub>(x)dx

More generally: 
E[g(X<sub>1</sub>...X<sub>n</sub>)] = ∫...∫g(x<sub>1</sub>...x<sub>n</sub>) f<sub>x<sub>1</sub> ... x<sub>n</sub></sub>(x<sub>1</sub> ... x<sub>n</sub>)dx<sub>1</sub> ... dx<sub>n</sub>

Example:
Var(X) = ∫(X-E[X])<sup>2</sup> f<sub>x</sub>(x)dx

Example: X ~ Unif(a,b)
E[X] = ∫ uniform density function

= ∫ x * 1 / (b-a) dx

= 1/2 * (b<sup>2</sup> - a<sup>2</sup>) / (b-a)

= 1/2 * (b+a)

Example: Var(X) = E[X<sup>2</sup>] - (E[X])<sup>2</sup>
= 1/2 (b-a)<sup>2</sup>

Let r = sqrt(x<sup>2</sup> + y<sup>2</sup>)
Compute the probability that the dart is in the middle half of the dartboard.

P{R<= r/2} = P{x<sup>2</sup> + y<sup>2</sup> <= r<sup>2</sup> / 4}

= E[{x<sup>2</sup> + y<sup>2</sup> <= r<sup>2</sup> / 4}]

= 1/(pi * r<sup>2</sup>) ∫ {x<sup>2</sup> + y<sup>2</sup> <= r<sup>2</sup> / 4} dx dy

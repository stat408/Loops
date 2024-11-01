# Loop Lecture


``` r
library(tidyverse)
library(knitr)
```

## What is Simulation?

Statistical simulation uses computer to mimic scenarios and estimate
probabilities of outcomes. Consider the casino game Roulette.

![Roulette Wheel](roulette.jpg)

We can use simulation to answer probabilistic questions and evaluate
gambling strategies.

## Roulette Simulation in R

Consider 10 spins from a Roulette wheel function.

``` r
RouletteSpin <- function(num.spins){
  # function to simulate roulette spins
  # ARGS: number of spins
  # RETURNS: result of each spin
  outcomes <- tibble(number = c('00','0',
                as.character(1:36)),
                color=c('green','green','red','black','red','black',
                        'red','black','red','black','red','black',
                        'black','red','black','red','black',
                        'red','black','red','red','black',
                        'red','black','red','black','red',
                        'black','red','black','black','red',
                        'black','red','black','red','black','red'))
  return(outcomes[sample(38,num.spins,replace=T),])
}
kable(RouletteSpin(10))
```

| number | color |
|:-------|:------|
| 30     | red   |
| 29     | black |
| 24     | black |
| 36     | red   |
| 3      | red   |
| 1      | red   |
| 29     | black |
| 34     | red   |
| 16     | red   |
| 15     | black |

## Conditional Expressions in R

``` r
set.seed(10282024)
rand.uniform <- runif(n = 1, min = 0, max = 1); rand.uniform
```

    [1] 0.9718027

``` r
rand.uniform < .5
```

    [1] FALSE

## Conditional Expression: If and Else

Now what does this return?

``` r
if (rand.uniform < .5){
  print('value less than 1/2')
} else {
    print('value greater than or equal to 1/2')
}
```

## Conditional Expression: Vectorized?

Does this function accept a vector as an input?

``` r
rand.uniform2 <- runif(2)
print(rand.uniform2)
```

    [1] 0.5058507 0.2515175

``` r
if (rand.uniform2 < .5){
  print('value less than 1/2')
} else {
    print('value greater than or equal to 1/2')
}
```

    Error in if (rand.uniform2 < 0.5) {: the condition has length > 1

``` r
ifelse(rand.uniform2 < .5, 'less than 1/2', 'greater than 1/2')
```

    [1] "greater than 1/2" "less than 1/2"   

The `ifelse()` function is vectorized and generally preferred with a
single set of if/else statements.

## Exercise: Conditional Expression

Write a conditional statement that takes a playing card with two
arguments:

- (`card.number <- '4'` and `card.suit <- 'C'` = 4 of clubs or
  `card.number <-'K'` and `card.suit <- 'H'` = King of hearts) and
- Print `Yes` if the card is a red face card and `No` otherwise.

Verify this works using the following unit tests:

- `card.number <- 'J'` and `card.suit <- 'D'`
- `card.number <- 'Q'` and `card.suit <- 'S'`

## Solution: Conditional Expression

## Loops

Loops are a convenient way to repeat tasks. We can either cycle through
a sequence (`1:10`) or through items in a vector.

``` r
rand.uniform2
```

    [1] 0.5058507 0.2515175

``` r
for (i in 1:length(rand.uniform2)){
  print(i)
}
```

- what will each of these print?

``` r
rand.uniform2
```

    [1] 0.5058507 0.2515175

``` r
for (i in rand.uniform2){
  print(i)
}
```

## While loops

An alternative to for loops is to use the while statement.

``` r
set.seed(10282024)
total.snow <- 0
while (total.snow < 36){
  print(paste('need more snow, only have', 
              total.snow, 'inches'))
  total.snow <- total.snow + rpois(1,15)
}
```

    [1] "need more snow, only have 0 inches"
    [1] "need more snow, only have 22 inches"
    [1] "need more snow, only have 34 inches"

``` r
  print(paste('okay, we now have', total.snow, 'inches', ' hit the slopes'))
```

    [1] "okay, we now have 48 inches  hit the slopes"

## Repeat Loops

Repeat loops are similar to while loops, but with the requirement that
the expressions are evaluated at least once and require an explicit
`break` statement.

``` r
repeat{
  total.snow <- total.snow + rpois(1,15)
  print(paste('we have', total.snow, 'inches'))
  if (total.snow >= 36)
    break
}
```

    [1] "we have 63 inches"

## Exercise: Loops

Assume you plan to wager \$1 on red for ten roulette spins. If the
outcome is red you win a dollar and otherwise you lose a dollar. Write a
loop that simulates ten rolls and determines your net profit or loss.

``` r
#hint: to get color from a single spin use
RouletteSpin(1) |> select(color) |> pull()
```

    [1] "black"

## Solution: Loops

## Why not loops?

Some of you have seen or heard that loops in R should be avoided.

- **Why**: it has to do with how code is compiled in R. In simple terms,
  vectorized operations are much more efficient than loops.

- **How**: we have seen some solutions to this, explicitly using the
  apply class of functions. We can also write vectorized functions,
  consider the earlier roulette example where number of spins was an
  argument for the function.

## Monte Carlo Procedures

Some probabilities can easily be calculated either intuitively or using
pen and paper; however, as we have seen we can also simulate procedures
to get an approximate answer.

Consider poker, where players are dealt a hand of five cards from a deck
of 52 cards. What is the probability of being dealt a full house?

Could we analytically compute this probability? **Yes** Is it an easy
calculation? not necessarily. So consider a (Monte Carlo) simulation.

``` r
DealPoker <- function(){
  # Function returns a hand of 5 cards
  
  
  
  
}
```

Next write another function to check if the hand is a full house.

``` r
IsFullHouse <- function(hand){
  #determines whether a hand of 5 cards is a full house
  #ARGS: data frame of 5 cards
  #RETURNS: TRUE or FALSE
  cards <- hand[,2]
  num.unique <- length(unique(cards))
  ifelse(num.unique == 2, return(TRUE),return(FALSE)) 
}
```

Does this work? If not, what is the problem and how do we fix it?

## Closing Thoughts on Monte Carlo

A few facts about Monte Carlo procedures:

- They return a random result due to randomness in the sampling
  procedures.
- More iterations results in more accurate answers, but takes longer to
  run.
- The run-time (or number of iterations) is fixed and typically
  specified.
- Mathematically, Monte Carlo procedures are computing an integral or
  enumerating a sum.
- They take advantage of the law of large numbers.
- They were given the code name Monte Carlo in reference to the Monte
  Carlo Casino in Monaco.

## Exercise: Probability of Red, Green, and Black

1.  Calculate/Derive the probability of landing on green, red, and
    black.

2.  How can the `RouletteSpin()` function be used to compute or
    approximate these probabilities?

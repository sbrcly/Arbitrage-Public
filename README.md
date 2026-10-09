# Arbitrage detector

An internal tool I built for the Caesars Sportsbook trading floor in Las Vegas, in the evenings while I was still a trader. This repository is a write-up; the code belongs to the sportsbook. Case study: [scottbarclay.dev/work/arbitrage-detector](https://scottbarclay.dev/work/arbitrage-detector).

## What an arbitrage is

A gap between two books. If Caesars prices one side of a game high enough and a competitor prices the other side high enough, a bettor can back both outcomes and profit whichever way the game goes. The bettor's gain is the book's loss, and sharp bettors run software that finds these gaps within seconds. A trader who sees the gap first can move the line and close it.

## What the tool did

Every minute it compared every bet Caesars offered against the same bet at about fifty other sportsbooks and flagged any pair of prices a bettor could take both sides of. Each flag became a row on every trader's screen: the game, the market, our price, the competitor's price, and the margin. The trader moved the line; the row disappeared on the next cycle.

## How it worked

- The server pulled live odds from Caesars and from the other books through their APIs (Axios), paired each of our markets with the equivalent market elsewhere, and computed whether both sides together came out ahead.
- Results were pushed to every open screen over Socket.io on a one-minute cycle.
- The hard part was the pairing, not the arithmetic: fifty books spell team names, periods, and lines differently, and a false match is worse than a missed one because it sends a trader to move a line that was fine.

Stack: Node, Socket.io, MySQL, Google Cloud.

## Screenshots

![Flagged opportunities](https://user-images.githubusercontent.com/93163082/169880288-3cb09e61-2a11-4940-8607-8a3625321c0f.png)

![The table](https://user-images.githubusercontent.com/93163082/169875822-77eef13f-27ed-45f2-af33-0599492534ea.png)

![Checking a flag](https://user-images.githubusercontent.com/93163082/169905553-b0518a34-114a-4ee4-8841-b170cebd7338.gif)

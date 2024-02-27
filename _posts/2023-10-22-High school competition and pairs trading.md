---
title: High school competition and pairs trading - Stage 1
date: 2023-10-22 21:30 -0600
categories: [economics, quantitative-finance]
tags: [statistics, pairs-trading, portfolio-optimization, hypothesis-testing]

math: true
---

In September 2023 Warsaw Stock Exchange (WSE) hosted another edition of [a high school trading competition](https://sigg.gpw.pl/), with the first stage spanning three months. As of now (22/10/2023) I lead a 4 person team of IT-inclined students to implement a pairs trading strategy and research for other strategies, like portfolio optimization or others. Repository for this project will be available under [this](https://github.com/gournge/siggRL) link. 

This is a description of what happened throughout, along with most of the technical details. 

tldr; With neatly scraped data I implemented a Pairs Trading strategy, realized we aren't able to short sell stocks, backtested it's long-only version using an inverse ETF, got bad results, gambled all-in with Non-stationary Transformers and gambled all-in again with the most volatile stock on the market. Ultimately gained 2%, only to see all teams got admitted to the second stage. 

# SIGG game description 

Out of all 381 stocks that are available on WSE the game offers 60, along with 11 ETFs and 3 WSE indices (WIG, WIG20, MWIG40.) Our team is initialised with a wallet of 20k PLN. 

The game offers orders of types: Market order, Limit order, Stop order or Stop limit order. We made only market orders, buying/selling at the currently traded market price.

The second stage additionally offered the use of Futures, which will hopefully be the topic of the next post. 

## Legal disclaimer

The game rules fortunately prohibited only the use of automatic trading systems - "computer software that enables automatic transaction execution is not allowed" ([SIGG rules](https://www.gpw.pl/pub/GPW/files/PDF/Regulamin_SIGG18.pdf)) - it did not explicitly mention an external analytics program we would make to suggest trades. These market orders of course would be ultimately made by us.

# Data collection

With the incredible help of my friend Mikołaj WSE data was scraped and aggregated (for personal & educational purposes only!) from an API endpoint originally intended for graphical display of figures. 

To filter out only the 74 assets from 381 available, I copied the SIGG webpage HTML code and parsed it through ChatGPT to obtain relevant Ticker names.

![alt text](/gournge.github.io/assets/img/gpwtrader%20stocks.jpg)

# Strategies

The single most important thing we had in mind was that in order to gain any significant advantage over our competition, we shouldn't be following the general market trend, i.e. buy the stocks strongly correlated with the Polish market index WIG20, as this is what the majority of people would perhaps do. 

To get to any respectable position in the ranking, we should instead be trading the most volatile of stocks, following the market principle that the higher the risk, the higher reward.

Let's look at the daat in the [previous years](https://bank.pl/final-20-edycji-szkolnej-internetowej-gry-gieldowej-zwycieska-druzyna-osiagnela-stope-zwrotu-wynoszaca-1723/) around 30 teams gained any sort of prizes, out of 10k studens (so about 4000 teams). The top team got a return of $17\%$ in half a year, which is pretty extraordinary - the baseline S&P500 index should return $11.36\%$ annually on average (which we have access to through the ETFSP500) so every six months the average investor would tend to earn $\sqrt(11.36\%)=3.37\%$, not $17\%$. Following the parabolic shape of the [efficient frontier curve](https://en.wikipedia.org/wiki/Efficient_frontier), in order to match at least a $15\%$ return we would need to be 5 times more profitable than the market index, therefore the volatility we should be willing to tolerate is $5^2 = 25$ times larger. Over the course of a month, it would be $25 \times 12.84\% = 321\%$

Still, this is a strategy we were willing to take only if our main approach would not work. And it didn't. 

<!-- Assuming the market returns are normally distributed (which they aren't, but let's assume they are) we would have to be in the Top 30/4000=0.75% of all teams, which   -->


## Portfolio optimization 

Based on the dataset provided, [Wojciech Siostrzonek](https://github.com/wotorr3s) did research on the given stocks with [PyPortfolioOpt](https://github.com/robertmartin8/PyPortfolioOpt). 

From what we discussed with Wojciech, all considerable allocations contained the main market component. Therefore we speculated they would oscillate around the boring SP500 average, so we dropped this idea and proceeded further.

## Pairs trading 

In essence (and my understanding), the trading approach comes from the observation that some pairs of assets tend to be *cointegrated*; over time, their difference (or ratio) always comes back to some average value. 

There are many caveats of course, like the average value majorly changing: for instance, when in a pair of two similar companies one's Board of Directors would fire their founder CEO, a permanent divergence between their stock prices could appear.

When we find a cointegrated pair, we make profit by entering a long position on a stock that has diverged excessively downwards (we earn on it's growth) and entering a short position on a stock that has diverged excessively upwards (we earn on it's drop.) In case both stocks' price change in the same direction (they both fall or rise) the profits on one of them compensate the losses on the other.

For more comprehensible reference, see [this article](https://hudsonthames.org/definitive-guide-to-pairs-trading/) or [this Python notebook](https://github.com/KidQuant/Pairs-Trading-With-Python).

---

In our case, which spanned only 3 months, I considered only the prior 6 months to check for cointegration - additionally. If we were to consider much longer periods (like prior 2 years) we might have implicitly included currently events like COVID19, which might not have been so relevant in current relationships between companies.

### Finding the pairs 

We can deduce a pair of stocks is cointegrated if their difference is a stationary process - a time series that has a constant mean and variation. A statistical test to check if a process is stationary is already implemented in the SciPy library.

![Stationary vs Non-stationary process](https://miro.medium.com/v2/resize:fit:1400/0*u_PyV52-IQFtq1VP.jpeg)

After iterating through the ${74 \choose 2} = 2701$ pairs, which took only a couple of seconds, I considered only the 1000 best pairs (ones most likely to be be cointegrated.) Next, I excluded many pairs through ranking the newly obtained pairs by the number of times the difference crossed the x-axis (see image below.) 

![Pair of cointegrated stocks](/gournge.github.io/assets/img/good%20pair%20grupa%20pracuj%20-%20writualna.png)

In the end I manually inspected 172 pairs, excluding the ones which seemed to be cointegrated by an accident (like when a Software business was paired with a Metalurgy industry business.) 

Moreover, it looked like many pairs were following the general market trend (but their high volatility induced a large number of crossings in the spread graph.) Here are some examples:

![Bad pair 1](/gournge.github.io/assets/img/bad%20pair%20selvita%20vs%20gpw.png)

![Bad pair 2](/gournge.github.io/assets/img/bad%20pair%20jsw%20vs%20gpw.png)

### Trading with the pairs

### Approximating shorting
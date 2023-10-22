---
title: High school competition and pairs trading
date: 2023-10-22 21:30 -0600
categories: [economics, quantitative-finance]
tags: [statistics, pairs-trading, portfolio-optimization, hypothesis-testing]

math: true
---

**this post is currently is still in progress**

In September 2023 Warsaw Stock Exchange (WSE) hosted another edition of [a high school trading competition](https://sigg.gpw.pl/). As of now (22/10/2023) I am leading a 4 person team of IT-inclined students to apply a pairs trading strategy and a portfolio optimization library. Repository for this project is under [this](https://github.com/gournge/siggRL) link. Personally I am responsible for managing the pairs trading strategy. My friends ensured to: collect data from WSE, run the portfolio optimization algorithm and monitor outputs from my pairs trading algorithm.

# Data collection

With the amazing help of my friend Mikołaj WSE data was scraped and aggregated (for personal & educational purposes only!) from an API endpoint originally intended for graphical display of figures. 

# Portfolio optimization 

Based on the dataset provided, [Wojciech Siostrzonek](https://github.com/wotorr3s) has run the portoflio optimization algorithms from the open source Python library [PyPortfolioOpt](https://github.com/robertmartin8/PyPortfolioOpt).

# Pairs trading 


---
title: Simulating the Copernicus-Gresham's law in currency markets 
date: 2023-10-11 10:00 -0600
categories: [economics]
tags: [random, monte-carlo, multi-agent]

math: true
---

In September 2023 Polish Central Bank created [a high school competition](https://nbp.pl/xxi-edycja-konkursu-na-prace-pisemna-dla-szkol-podstawowych-i-ponadpodstawowych/) for an essay in economics exploring the persona of a XVI century astronomer [Nicolaus Copernicus](https://en.wikipedia.org/wiki/Nicolaus_Copernicus). 


![Copernicus](/assets/img/Nikolaus_Kopernikus.jpg)
*Nicolaus Copenicus (1473-1543)*

Having learned about it 10 days before the deadline, discarding my other schoolwork, I have managed to upload a rough version of my exploration. The main part of the essay, which I think is pretty interesting, is a simple model of a multi-currency market I will describe in this post. Project repository is under [this link](https://github.com/gournge/copernicus-transactions).

# Copernicus-Gresham's law

Copernicus law has been mistakingly attributed to a scottish economist [Thomas Gresham](https://en.wikipedia.org/wiki/Thomas_Gresham), due to a recurring error of works following the infamous [Henry Dunning Macleod](https://en.wikipedia.org/wiki/Henry_Dunning_Macleod)'s study *The Theory of Credit*. It was actually first consistently described in Copernicus' *Monete Cudende Ratio*. ([Source](https://kpbc.umk.pl/Content/48660/PDF/Copernicana_017_01.pdf))

The law is a monetary principle stating that "bad money drives out good". For example, if there are two forms of commodity money in circulation, which are accepted by law as having similar face value, the more valuable commodity will gradually disappear from circulation. (definition from [Wikipedia page](https://en.wikipedia.org/wiki/Gresham%27s_law))

In  *Monete Cudende Ratio* Copernicus also highlights the high possibility of loss of the country, as - based on historical facts he remarkably analyzed - foreign traders had demanded payments in the *better* version of a coin (the one containing more silver.)

# The model

In short, the model is about agents who perform transactions (transfers of funds) in a particular currency. 

## Simulation process

### Initialization 

An environment of agents is first initialized. Each country has an assigned home currency. Each agent has an assigned country id (chosen randomly from a uniform distribution) and a wallet of currencies, which is a list of numbers (same size for every agent) chosen uniformly from the interval $[0, 1]$. Agent's home currency value is then multiplied by a parameter `alpha` $>1$ as to imitate what realistically holds.
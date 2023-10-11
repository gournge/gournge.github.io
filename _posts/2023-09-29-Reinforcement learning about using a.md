---
title: Reinforcement learning about using a
date: 2023-09-29 14:00 -0600
categories: [machine-learning]
tags: [reinforcement-learning]

math: true
---

In April 2023 I have collaborated with [Wojciech Siostrzonek](https://github.com/wotorr3s) and a local university - [Politechnika Śląska](https://www.polsl.pl/idub/) - to conduct a project under the supervision of a PhD. The project finished with a presentation at a conference that happened in 22th of September. [Here](https://github.com/gournge/cleaning-optimization) is the link to our project repository and [here](https://github.com/gournge/cleaning-optimization/blob/master/posters/poster%20english.pdf) is the english version of our conference poster.

We wanted to explore how reinforcement learning algorithms work: we created a little environment in which you could move a broom inside of a room to drop dirt inside *mounds* (they act like holes.)

The DDPG model behind our agent is described in [this original paper](https://arxiv.org/abs/1509.02971).

# Results

Our agent failed to learn to clean. The only weird thing was this anomaly of sudden increase in variation of scores we couldn't reproduce in later experiments.  

![anomaly](/assets/img/anomalia.png)

# Discussion

I had some suspicions to whether the anomaly came from the cleaning agent appending his scores to some previous score history, but having analyzed the experiments and commit changes history, there is a really small chance for that. 

For future experiments we could try to change the Critic architecture: I thought about trying to connect the information about the previous action to be concatenated with the last layer instead of the one before it. 

![critic architecture](/assets/img/cleaning%20agent%20critic%20architecture.jpg)

From one perspective more layers (2 in the existing implementation) allows the agent to recognize more sophisticated patterns, but from the other, the information about the move the Actor model has produced might be really significant and it somehow gets lost when concatenated with additional 900 values. Sometime in the future I'd like to explore this payoff of simplyfing these *reasoning capabilities* in order to improve algorithm's convergence (or at least make it not be stuck in a local minimum, which in our experiments was e.g. constantly making moves about a diagonal where there is a low chance of hitting a wall, thus getting punished).
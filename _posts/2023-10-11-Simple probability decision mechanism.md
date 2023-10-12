---
title: Simple probability decision mechanism
date: 2023-10-11 10:00 -0600
categories: [probability]
tags: [random]

math: true
---

Here I describe a technical idea that I use multiple times in the implementation of [this model](https://gournge.github.io/posts/Simulating-the-Copernicus-Gresham's-law-in-currency-markets/) as well as in my IB Extended Essay on an application of genetic algorithms, when there is a need to select two parents to produce genes for the next generation.

How do we choose the parents so that there is not a large bias towards the genes with the best evaluation score? If the bias is too big, we might overlook combining other interesting characteristics of the rest of the population and ultimately get stuck in a local minimum.  

What is usually implemented is the following: if each gene $g_i$, $i = 1, 2, \ldots , n$ has a corresponding evaluation score $s_i$ we assign to it a probability of

 $$ p_i = \frac{s_i}{ \sum_{j=1}^{n} s_j}$$

Obviously the probabilities sum to one:

 $$ \sum_{i=1}^{n} p_i = \sum_{i=1}^{n} \frac{s_i}{ \sum_{j=1}^{n} s_j} = \frac{ \sum_{i=1}^{n} s_i}{ \sum_{j=1}^{n} s_j} = 1 $$

they also have the nice property that if a gene has e.g. twice as large evaluation score as some other gene it's twice as likely to be chosen compared to the other one. Based on $p_i$ we choose parents for crossover.

In my EE I wanted to compare the optimal results of the genetic algorithm to the worst possible ones. But how do we assign probabilites to genes for parent selection so that they are larger for genes with lower evaluation scores? 

If we have a gene with an evaluation score twice as large as some other gene, we would want the probability of choosing this gene to be twice as *small* compared to the gene with the smaller score. How do we calculate $p_i$? My idea was really simple:

$$ p_i = \frac{ \frac{1}{s_i} }{  \sum_{j=1}^{n} \frac{1}{s_j} } = \frac{ 1 }{ s_i \sum_{j=1}^{n} \frac{1}{s_j} } $$ 

Again, probabilities obviously sum to one. What about the proportional relationship? Let's evaluate

$$ \frac{p_x}{p_y} = \frac{ \frac{ 1 }{ s_x \sum_{i=1}^{n} \frac{1}{s_i} } }{ \frac{ 1 }{ s_y \sum_{j=1}^{n} \frac{1}{s_j} } } = \frac{s_y}{s_x}$$

so if the score of $g_x$ is $k = \frac{s_y}{s_x}$ times smaller than $g_y$'s the probability of choosing $g_x$ is $k$ times larger. Neat! 

To see how this mechanism is implemented in a currency market model, click [here](https://gournge.github.io/posts/Simulating-the-Copernicus-Gresham's-law-in-currency-markets/).

---
title: Circlestones game
date: 2023-02-28 10:36 -0600
categories: [optimisation]
tags: [random, monte-carlo]

math: true
---

# Introduction

This piece of work loosely researches properties of a certain type of a discrete time random process - a simple game that will be reffered to throughout as *Circlestones*. 

**Definition 1. Circlestones**
> $n$ distinct (numbered) stones are arranged in a circle. For every stone in order (after the $n$-th stone we turn back to the one with the smallest index) we move it right (with probability $p$), left (with probability $q$) or leave it in place (with probability $1-p-q$). If a spot is occupied the current stone takes the place of the stone that has been on the spot earlier. 

**Example 2.**

![example of a game with $n=4$](/gournge.github.io/assets/lib/example%201.jpg)
*Possible course of the game. Stone 1 jumps right with probability $p$ and takes the stone 2. Next smallest stone is the stone 3 - it jumps left with probability $q$ and takes the stone 1. Finally the next smallest stone - stone 4 - stays in place with probability $1-p-q$.*

**Definition 3.** 
> Stopping time is the amount of steps before there is only one stone left.

- - -

There are many questions we can ask about this system, one of which being: *what is the expected stopping time of a game*?

# Numerical simulations

## Explicitly approximating the expected value

## Monte Carlo simulation
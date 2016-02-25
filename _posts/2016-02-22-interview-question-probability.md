---
layout: post
title: Quant Interview Question (Probability Part)
category: Interview
tags: Interview Probability
keywords:
description:
---

**Question 1**:

A rope is divided randomly into three pieces. Find the average length of the shortest/longest segment.

**Answer**:

The shortest one is $$\frac{1}{9}$$, the longest one is $$\frac{11}{18}$$.

More generally, if the stick is broken into $$n$$ pieces, then the shortest one is $$\frac{1}{n^2}$$ and the longest one is $$\frac{1}{n}H_n$$, where $$H_n$$ is the [harmonic number](https://en.wikipedia.org/wiki/Harmonic_number).

Reference:

[1][http://math.stackexchange.com/questions/13959/if-a-1-meter-rope-is-cut-at-two-uniformly-randomly-chosen-points-what-is-the-av](http://math.stackexchange.com/questions/14190/average-length-of-the-longest-segment)

[2][http://math.stackexchange.com/questions/13959/if-a-1-meter-rope-is-cut-at-two-uniformly-randomly-chosen-points-what-is-the-av](http://math.stackexchange.com/questions/13959/if-a-1-meter-rope-is-cut-at-two-uniformly-randomly-chosen-points-what-is-the-av)

**Question 2**:

Expected number of uniform random variables needed such that their sum is greater than 1.

**Answer**:

$$e$$!

Hint: $$E[N] = 1 + \sum_{n=1}^{\infty}P(N > n)$$ and then think about the cube.

[1][http://math.stackexchange.com/questions/111314/choose-a-random-number-between-0-and-1-and-record-its-value-keep-doing-it-until](http://math.stackexchange.com/questions/111314/choose-a-random-number-between-0-and-1-and-record-its-value-keep-doing-it-until)

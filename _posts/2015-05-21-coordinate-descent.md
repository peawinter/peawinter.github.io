---
published: false
layout: post
title: Optimization - Coordinate Descent
category: Research
tags: optimization gradient coordinate descent blackbox
keywords:
description:
---

> Blackbox Oracle! OMG!

## Blackbox Model

What are we talking about when we are saying black-box?

![1](/public/img/blackbox.png)

We now describe our first model of ”input” for the objective function
and the set of constraints. In the black-box model we assume that
we have unlimited computational resources, the set of constraint X is
known, and the objective function f : X → R is unknown but can be
accessed through queries to oracles:
• A zeroth order oracle takes as input a point x ∈ X and
outputs the value of f at x.
• A first order oracle takes as input a point x ∈ X and outputs
a subgradient of f at x.
In this context we are interested in understanding the oracle complexity
of convex optimization, that is how many queries to the oracles are
necessary and sufficient to find an ε-approximate minima of a convex
function. To show an upper bound on the sample complexity we
need to propose an algorithm, while lower bounds are obtained by
information theoretic reasoning (we need to argue that if the number
of queries is ”too small” then we don’t have enough information about
the function to identify an ε-approximate solution).

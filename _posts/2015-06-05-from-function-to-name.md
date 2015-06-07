---
published: false
layout: post
title: R from function to its name
category: Tips
tags:
-  R
-  function
keywords: R
description:
---

In my research, I design an algorithm for various testing problems. In my main algorithm, I take the function name of testing problems as an argument.

```R
myTestFun1 <- function() {
  ...
}

myTestFun2 <- function() {
  ...
}

myMain <- function(testFun) {
  ...
}
```

R works greatly since it passes function as variable (so-called functional programming?)

In `myMain` function, I need to output

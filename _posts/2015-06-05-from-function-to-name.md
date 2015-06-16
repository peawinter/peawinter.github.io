---
published: true
layout: post
title: R from function to its name
category: Tips
tags:
-  R
-  function
keywords: R
description:
---

In my research, I design an algorithm for various testing problems. In the R implementation of the algorithm, I take the function name of testing problems as an argument.

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

In `myMain` function, I want to generate output looks like

```
test function: myTestFun1

        value: ....
```

Here I got a problem, since R passes the function to `testFun`, the names of those testing functions (myTestFun1 and myTestFun2) are not retrievable. To walk around it, instead of passing the function, I pass the name of test function and use `match.fun` to extract the desired function object.

```R
myMainNew <- function(testFunName) {
  testFun <- match.fun(testFunName)
}
```

### Reference

1. [http://stat.ethz.ch/R-manual/R-devel/library/base/html/match.fun.html](http://stat.ethz.ch/R-manual/R-devel/library/base/html/match.fun.html)

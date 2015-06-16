---
published: false
title: R download file from web
layout: post
author: Wenyu
category: tips
tags:
-  R
-  Download
-  Snippet
---

When I need to import data that are not on my computer, unless I will use the same data more than twice, I would rather not to save the file on my disk and use R function to download.

```R
> load(url("http://www.psychology.mcmaster.ca/bennett/psy710/datasets/mood_data.Rdata"))
> ls()
[1] "mood.data"
> head(mood.data)
     group mood
1 pleasant    6
2 pleasant    5
3 pleasant    4
4 pleasant    7
5 pleasant    7
6 pleasant    5
```

Another useful package is `RCurl`.

```R
> library(RCurl)
> myURL <- 'some_url'
> myData <- getURLContent(URLencode(myURL))
```

`getURL` may issue an error when the file is not text. `getURLContent` will determine the type of the content automatically.

`URLencode` is to encode the link. In this case, it is unnecessary, but for link contains quotes, we have to use 'URLencode' to process the link at first.

Reference

1. [http://stackoverflow.com/questions/26108575/loading-rdata-files-from-url](http://stackoverflow.com/questions/26108575/loading-rdata-files-from-url)
2.
[http://www.omegahat.org/RCurl/installed/RCurl/html/getURL.html](http://www.omegahat.org/RCurl/installed/RCurl/html/getURL.html)

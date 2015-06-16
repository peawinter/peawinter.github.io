---
published: false
title: Python punctuation
layout: post
author: Wenyu
category: tips
tags:
-  Python
-  re
-  Snippet
---

Here is a regex to match a string of characters that are not a letters or numbers:

```python
[^A-Za-z0-9]+
```

Here is the Python command to do a regex substitution:

```python
re.sub('[^A-Za-z0-9]+', '', mystring)
```

```python
s = "hello world! how are you? 0"

# Short version
print filter(lambda c: c.isalpha(), s)

# Faster version for long ASCII strings:
id_tab = "".join(map(chr, xrange(256)))
tostrip = "".join(c for c in id_tab if c.isalpha())
print s.translate(id_tab, tostrip)

# Using regular expressions
print re.sub("[^A-Za-z]", "", s)
```


Reference

1. [http://stackoverflow.com/questions/5843518/remove-all-special-characters-punctuation-and-spaces-from-string](http://stackoverflow.com/questions/5843518/remove-all-special-characters-punctuation-and-spaces-from-string)

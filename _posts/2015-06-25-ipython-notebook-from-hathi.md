---
published: true
title: Access remote ipython notebook
layout: post
author: Wenyu
category:
- tips
tags:
-  python
-  notebook
---

Ipython notebook provides a interactive and intuitive way to manipulate with data. As I am thinking to playing with larger size data, my hands are tied due to the limitation of my laptop. Then I realize I''ve had the access to the Purdue server `Hathi`, which is mainly for Hadoop job. More excitingly, it contains all packages from Python (including anaconda distribution), MPICH2, and R. (I found this when I am trying to install the `Rmpi` package.) The inspiration to use ipython hosted by Hathi hits me immediately.

The steps are straightforward,

1. Make sure your can login without password. This can be done by following [reference 2](http://www.linuxproblem.org/art_9.html).

2. Start ipython on Hathi.

```bash
% on server
module add anaconda
ipython notebook --no-browser --port=7777
```

3. Setup the ssh tunnel from the local machine to the remote machine.

```bash
% on local machine
ssh -L 7777:localhost:7777 user@hathi.rcac.purdue.edu
```

4. Access remotely hosted ipython notebook through your favorite brower at [localhost:7777](localhost:7777)

As Joe Hamman said,

> the beauty of this is that I interact only with the browser on my local machine, while having access to the computing power and file system of the remote machine.

## Reference

1. [http://www.hydro.washington.edu/~jhamman/hydro-logic/blog/2013/10/04/pybook-remote/](http://www.hydro.washington.edu/~jhamman/hydro-logic/blog/2013/10/04/pybook-remote/)
2. [http://www.linuxproblem.org/art_9.html](http://www.linuxproblem.org/art_9.html)

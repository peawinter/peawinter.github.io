---
title: Install Rmpi on Mac and Redhat server
layout: post
author: Wenyu
category: tips
tags:
-  R
-  parallel computing
---

To me, learning something new always starts from stucking in some troubles. This time is the compiling error issued when I installed `Rmpi` for the first time.

## Installation

It turns out `Rmpi` depends on `open MPI`, which should be installed by

```bash
$ brew install open-mpi
```

Then in `R`, install the package from source.

```r
install.packages("Rmpi", type="source")
```

As to install `Rmpi` on server, we need some specification. This line works on `radon`.

```bash
R CMD INSTALL Rmpi_0.6-5.tar.gz --configure-args=--with-mpi=/apps/rhel6/openmpi/1.6.3/intel-13.1.1.163
```

Although this line can do the job, but it is not straight forward to get here.

1. Download Rmpi_version.gz
2. Add module

```bash
module add r
module add openmpi
```

3. Check the directory of the openmpi via

```bash
ompi_info
```

4. Final step to install `Rmpi`

```bash
R CMD INSTALL Rmpi_0.6-5.tar.gz --configure-args=--with-mpi=/apps/rhel6/openmpi/1.6.3/intel-13.1.1.163
```

## Examples

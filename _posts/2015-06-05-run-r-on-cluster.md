---
published: true
layout: post
title: Running R on cluster
category: Tips
tags:
-  R
-  Linux
-  PBS
-  Terminal
-  cluster
keywords: R
description:
---

I've now started a new research on high dimension black box optimization. That means much larger computations and a need to program in a much more smart and layback manner. My old Mac just cannot take this job alone. Luckly, I have the 'Radon' and several other clusters to save my ass. Play R on my Mac is easy. But it is another thing on cluster.

I mainly follow the [instruction](http://www.stat.purdue.edu/~dgc/cluster.pdf) by Doug.

### Load R

Radon doesn't come up with R for granted. To load R, we need to type `model load r`. The `module` command is used to “load” software
packages for use by the current login session.

{% highlight Bash shell scripts linenos%}
-bash-4.1$ R --version
-bash: r: command not found
-bash-4.1$ module load r
-bash-4.1$ R --version
R version 3.1.0 (2014-04-10) -- "Spring Dance"
Copyright (C) 2014 The R Foundation for Statistical Computing
Platform: x86_64-unknown-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under the terms of the
GNU General Public License versions 2 or 3.
For more information about these matters see
http://www.gnu.org/licenses/.
{% endhighlight%}

Still, I want to run R script in background (leave it running even if logoff). One way is to run the R script in a screen session. Another way is to use the batch mode. The former one is no different from running the code on my Laptop. The later one needs some care.

### BATCH mode

R script vs R batch

> `R CMD BATCH` (see `help(BATCH)`) has default options `--restore --save --no-readline`, while `Rscript` (see `help(Rscript)`) has `--slave --no-restore`. Furthermore, the output of `R CMD BATCH` always goes to a file, taken from the command line if given, or else built from the input filename by appending "out", whereas the output of `Rscript` goes to stdout. Finally, `Rscript` accepts user level command-line arguments, after the script name "as usual" (`Rscript foo.R 1 2`), whereas CMD BATCH accepts them at the end of the options but before the script name, and prefaced by `--args` (`R CMD BATCH "--args 1 2" foo.R`). In either case, such arguments are available in the R program as the character vector commandArgs(trailingOnly = TRUE). [link](http://biostat.jhsph.edu/~hjaffee/R_tasks.html)

Like said, `Rscript` executes quietly in the background and generates output to 'stdout'.

{% highlight Bash shell scripts linenos%}
R CMD BATCH t.R > t.out
Rscript t.R > t.out & # run in background
nohup Rscript t.R > t.out & # run in background, even if logout
{% endhighlight%}

Although we can launch several jobs simultaneously (by concatenating with '&'), but invoking manually doesn't scale well. To run tasks seriesly, we can create a file like "run.sh" that contains several 'Rscript' invocations. It will run the jobs one at a time.

```ruby
# run.sh
Rscript t.R > tout.001
Rscript t.R > tout.002
Rscript t.R > tout.003
Rscript t.R > tout.004
Rscript t.R > tout.005
Rscript t.R > tout.006
Rscript t.R > tout.007
Rscript t.R > tout.008
```

A life-saving tips for creating the “run.sh” script using R

```r
> sprintf("Rscript t.R > tout.%03d", 1:8)
[1] "Rscript t.R > tout.001" "Rscript t.R > tout.002" "Rscript t.R > tout.003"
[4] "Rscript t.R > tout.004" "Rscript t.R > tout.005" "Rscript t.R > tout.006"
[7] "Rscript t.R > tout.007" "Rscript t.R > tout.008"
> write(sprintf("Rscript t.R > tout.%03d", 1:8), "run.sh")
```

Then we can run "run.sh" in command line.

{% highlight Bash shell scripts linenos%}
-bash-4.1$ sh run.sh # run every job in run.sh one at a time
-bash-4.1$ nohup sh run.sh & # run every job in run.sh one at a time, and keep running even if logout
-bash-4.1$ nohup xargs -d '\n' -n1 -P4 sh -c < run.sh & # run every job in run.sh keeping 4 jobs running simultaneously keep running even if logout
{% endhighlight%}

### Submit jobs through qsub

On a cluster, it is not recommended (allowed) to run big jobs on the front ends. A more proper way is to submit jobs through qsub. The punchline is to submit a job description through `qsub`. A typical job description will look like this one.

```bash
#!/bin/bash

## Give your job a name
#PBS -N ovs
## Needed if you want to use environmental variables
#PBS -V

## Specify running time and memory
#PBS -l walltime=00:15:00,vmem=4GB

## Specify number of nodes and cores
#PBS -l nodes=1:ppn=8

## Specify output and error (no need to include these two lines)
#PBS -o 'qsub.out'
#PBS -e 'qsub.err'

# set the temporary file location
export TMPDIR=$WORKDIR

# go to the job submission directory; you can change this to your project directory
cd $PBS_O_WORKDIR

# load the R module; change the version if needed
module load r


## Export the job number. This is unique to a job and can be used to set the seed
## and as an identifier for the output file
export job_number=`echo $PBS_JOBID | awk -F. '{print $1}'`

# run R
Rscript --vanilla foo.r > foo.Rout
```

Then the file is ready to submit.

{% highlight Bash shell scripts linenos%}
-bash-4.1$ qsub foo.pbs
{% endhighlight%}

Other useful command includes

{% highlight Bash shell scripts linenos%}
-bash-4.1$ qstat        # See list of jobs in the queue
-bash-4.1$ qstat –u usr # See list of jobs in the queue submitted by usr
-bash-4.1$ qdel JOBID   # delete a previously submitted job from the queue
{% endhighlight%}

Note that `#!/bin/bash` should be put in the first line.

Another trick from Doug is to force PBS to schedule a node exclusively to prevent misbehave users messing with the shared memory and node.

```bash
#!/bin/sh -l
#PBS -l nodes=1:ppn=8
#PBS -l walltime=00:30:00
cd $PBS_O_WORKDIR
module add r
Rscript t3.R >out1 &
Rscript t3.R >out2 &
Rscript t3.R >out3 &
Rscript t3.R >out4 &
Rscript t3.R >out5 &
Rscript t3.R >out6 &
Rscript t3.R >out7 &
Rscript t3.R >out8 &
wait
```

### Pass arguments

Like said, `Rscript` takes system arguments. In my numerical study, I need to test my algorithms under different parameter settings. A convenient way to create a script and execute it in terminal. (Learn this trick from [Dr. Anindya Bhadra](http://www.stat.purdue.edu/~bhadra/).)

```bash
## HOWTO: chmod ugo+rx cluster; ./cluster

## Make sure you are using the bash shell
#!/bin/bash

## Grid of starting values for parameters
STARTAs=(1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20)
STARTBs=(0 1 4)
STARTLs=(50 100 150)

## To be read in inside of R as environmental variables
LENA=${#STARTAs[*]}
LENB=${#STARTBs[*]}
LENL=${#STARTLs[*]}

## Submit jobs via qsub with each starting value of "a" exported to the environment.
## Sleep 1 second between submitting two jobs

for k in `seq 1 $LENL`
do
	for i in `seq 1 $LENA`
	do
	    for j in `seq 1 $LENB`
	    do
	        export STARTA=${STARTAs[`expr $i - 1`]}
	        export STARTB=${STARTBs[`expr $j - 1`]}
	        export STARTL=${STARTLs[`expr $k - 1`]}
	        qsub -V NumExp.pbs
	        sleep 1
	    done
	done
done
```

### Download packages

An annoying bug is that R cannot import and load packages properly.

{% highlight Bash shell scripts linenos%}
Calls: install.packages -> grep -> contrib.url
In addition: Warning message:
In library(package, lib.loc = lib.loc, character.only = TRUE, logical.return = TRUE,  :
  there is no package called ‘***’
Execution halted
{% endhighlight%}

A remedy is found on [stackoverflow](http://stackoverflow.com/questions/17705133/package-error-when-running-r-code-on-command-line).

> For example, if you want to use RStudio's package repository, set repos="http://cran.rstudio.com/" inside the install.packages call.

```r
if (!require("yaml")) {
  install.packages("yaml", repos="http://cran.rstudio.com/")
  library("yaml")
}
```

### Reference

1. [http://www.stat.purdue.edu/~dgc/cluster.pdf](http://www.stat.purdue.edu/~dgc/cluster.pdf)

2. [http://biostat.jhsph.edu/~hjaffee/R_tasks.html](http://biostat.jhsph.edu/~hjaffee/R_tasks.html)

3. [https://wiki.csiro.au/display/ASC/Run+R+script+as+a+batch+job+on+a+Linux+cluster](https://wiki.csiro.au/display/ASC/Run+R+script+as+a+batch+job+on+a+Linux+cluster)

4. [http://stackoverflow.com/questions/17705133/package-error-when-running-r-code-on-command-line](http://stackoverflow.com/questions/17705133/package-error-when-running-r-code-on-command-line)

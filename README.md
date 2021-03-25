# DADA2 Docker image

[![](https://img.shields.io/badge/dada2-1.18.0-blue.svg)](https://hub.docker.com/r/blekhmanlab/dada2/ "Current DADA2 version is 1.18.0")[![](https://img.shields.io/docker/pulls/blekhmanlab/dada2.svg)](https://hub.docker.com/r/blekhmanlab/dada2/ "DADA2 image on Docker Hub")

A simplified, updated repository based on the [dada2docker](https://github.com/joey711/dada2docker) repo from Paul J. McMurdie. This provides an isolated environment from which to run [DADA2](https://benjjneb.github.io/dada2/index.html), a tool for analyzing amplicon sequencing data. If you want to run a version of DADA2 that has system requirements that don't line up with your local environment (DADA2 v1.18 requires R version 3.6.1, for example), running the containerized version may simplify the process. **This image also includes the [decontam](https://benjjneb.github.io/decontam/) package,** and uses [Bioconductor](https://www.bioconductor.org/) v3.12.

## Use

By default, the container drops you into an R session. If this is how you intend to use the container, interacting with the R application directly, you'll want to launch the container in "interactive" mode, with the `i` and `t` flags:

```sh
docker run -it blekhmanlab/dada2
```

Or, using the Singularity runtime:

```sh
singularity run docker://blekhmanlab/dada2
```

Once the container starts, running `library(dada2)` will load DADA2, and `packageVersion('dada2')` should confirm that you have the correct version. **Exiting the R application will cause the container to shut down.**

### Running R scripts

There are a few more steps if you want to run an R script in your container, rather than working interactively in the R shell:

1. You will need to get your R script **into the container**. Docker [bind mounts](https://docs.docker.com/storage/bind-mounts/) enable you to do this; their documentation explains how.
1. Once you specify a location in the container's filesystem where your R script should live once it starts, you need to specify what command the container should run on startup, rather than just `R`, which it runs by default.

An example, that assumes you have an R script called `test.R` in the directory you are currently in on the host machine:

```sh
docker run -it -v "$pwd":/cool_demonstration blekhmanlab/dada2 Rscript /cool_demonstration/test.R
```

This will mount the current working directory on the *host* machine to a location called `/cool_demonstration` *within the container*. So your script will be in the container at `/cool_demonstration/test.R`, and on startup the container will run the `Rscript /cool_demonstration/test.R` command and then exit.


*Badges at top of README via [shields.io](https://shields.io/).*

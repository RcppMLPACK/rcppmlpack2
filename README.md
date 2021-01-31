## RcppMLPACK2: Rcpp bindings for mlpack 3.*

[![Build Status](https://travis-ci.org/rcppmlpack/rcppmlpack2.svg)](https://travis-ci.org/rcppmlpack/rcppmlpack2)
[![License](http://img.shields.io/badge/license-GPL%20%28%3E=%202%29-brightgreen.svg?style=flat)](http://www.gnu.org/licenses/gpl-2.0.html)



### Experimental -- And now outdated !!

**2020/2021 Update:** Thanks to most excellent and persistent work of [Yashwant Singh](https://github.com/Yashwants19) over the course of Google Summer of Code 2020 (and, actually, both _preceeding_ it and _continuing after_) along with a few others, R is now a first-class citizen in mlpack. See [this directory in the mlpack repo](https://github.com/mlpack/mlpack/tree/master/src/mlpack/bindings/R) for its source, and see [this page at CRAN](http://cloud.r-project.org/package=mlpack) for the resulting R package. The original README.md content below does not reflect this---but you can more or less ignore this repo now.

This is a more experimental version derived off
[Qiang Kou](https://www.linkedin.com/pub/qiang-kou/2a/986/6b7)'s excellent
[RcppMLPACK](https://github.com/thirdwing/RcppMLPACK) package which is also on CRAN as
[RcppMLPACK](https://cran.r-project.org/package=RcppMLPACK).  That version is based on
mlpack 1.0.6 and embeds it, ensuring builds on all platforms CRAN cares about.

Here, we are trying to build something lighter-weight and more current. We will use
_external_ (system) libraries for [mlpack](http://www.mlpack.org/) instead of bundling
them inside the R package. See below for more detailed installation notes.

Note that the external Boost linkage created issues with the RStudio IDE which also
uses Boost threading (and then some more Boost) itself. Loading the package will
crash an RStudio binary that is older than the daily build version 1.1.129.  The good
news is than more recent RStudio versions are not affected due to a change in how
the IDE is built off Boost.

### What is mlpack?

Quoting from the [main page](http://www.mlpack.org/index.html):

> [mlpack](http://www.mlpack.org/) is a scalable machine learning library, written in C++,
> that aims to provide fast, extensible implementations of cutting-edge machine learning algorithms.
> [mlpack](http://www.mlpack.org/) provides these algorithms as simple command-line programs and C++ classes
> which can then be integrated into larger-scale machine learning solutions.

and the [about page](http://www.mlpack.org/about.html):

> [mlpack](http://www.mlpack.org/) is a C++ machine learning library with emphasis on scalability, speed,
> and ease-of-use. Its aim is to make machine learning possible for novice users by means of a simple,
> consistent API, while simultaneously exploiting C++ language features to provide maximum performance
> and maximum flexibility for expert users. This is done by providing a set of command-line executables
> which can be used as black boxes, and a modular C++ API for expert users and researchers to easily
> make changes to the internals of the algorithms.
>
> As a result of this approach, mlpack outperforms competing machine learning libraries by large margins; see
> the [BigLearning workshop paper](http://www.mlpack.org/papers/mlpack2011.pdf) and the
> [benchmarks](http://www.mlpack.org/benchmark.html) for details.

An alphabetical list of currently implemented methods in [mlpack](http://www.mlpack.org/) is shown below,
see [here](http://www.mlpack.org/about.html) for links to papers, API documentation, tutorials and other
references:

* Collaborative filtering (with many decomposition techniques)
* Decision stumps (one-level decision trees)
* Density estimation trees
* Euclidean minimum spanning tree calculation
* Gaussian mixture models
* Hidden Markov models
* Kernel Principal Components Analysis (optionally with sampling)
* k-Means clustering (with several accelerated algorithms)
* Least-angle regression (LARS/LASSO)
* Linear regression (simple least-squares)
* Local coordinate coding
* Locality-sensitive hashing for approximate nearest neighbor search
* Logistic regression
* Max-kernel search
* Naive Bayes classifier
* Nearest neighbor search with dual-tree algorithms
* Neighborhood components analysis
* Non-negative matrix factorization
* Perceptrons
* Principal components analysis (PCA)
* RADICAL (independent components analysis)
* Range search with dual-tree algorithms
* Rank-approximate nearest neighbor search
* Sparse coding with dictionary learning


### Installation

As mentioned above, the official
[RcppMLPACK CRAN package](https://cran.r-project.org/package=RcppMLPACK) with its
[GitHub repo](https://github.com/thirdwing/RcppMLPACK) includes the source code from the
mlpack library (version 1.*). Thus users do not need to install mlpack itself in order to
use RcppMLPACK.

This package, however, uses the external [mlpack](http://www.mlpack.org/) library (version
2.* or later), so see below for detailed instructions.

We have successfully used version 2.1.0 (with a patch, see below), version 2.1.1,
version 2.2.0, and now version 3.0.1.

#### Using Docker

We use [Docker](https://www.docker.com) for the [Travis CI](https://www.travis-ci.org) tests, and also provide a container
for deployment. Do

```sh
docker pull rcppmlpack/run                 ## one time
docker run --rm -ti rcppmlpack/run bash    ## launch container
```

to start the container with `RcppMLPACK` installed.  See the
[Boettiger and Eddelbuettel RJournal paper](https://journal.r-project.org/archive/2017/RJ-2017-065/index.html)
for more on Docker for R, and the [Rocker Project](https://www.rocker-project.org) used here.

#### Debian

Thanks to Barak Pearlmutter, there is an excellent
[Debian package](https://packages.debian.org/source/sid/mlpack) and its versions 2.1.0 or
later will do nicely. Just do `sudo apt-get install libmlpack-dev` (provided you point to
Debian _sid_ (aka 'unstable') or _stretch_ (aka 'testing') as these packages are not yet
in a stable Debian release).

#### Ubuntu

Unofficial packages for LTS releases trusty (14.04), xenial (16.04),
and bionic (18.04)
[are available in Dirk's PPA](https://launchpad.net/~edd/+archive/ubuntu/misc/+packages),
and used by the
[Travis CI integration](https://github.com/eddelbuettel/rcppmlpack2/blob/master/.travis.yml). We
may also make versions for intermediate releases available as time
permits.  They are based on Barak's Debian packages.  Via the standard
Debian-to-Ubuntu package transition, these should also appear in
upcoming Ubuntu releases.

#### OS X / Brew

James Balamuta has kindly prepared a
[pull request which has been submitted and merged](https://github.com/Homebrew/homebrew-science/pull/4637)
to update the version of mlpack in brew.  However, I hear that there are issue between the
brew compiler stack and what is used for R.  So this may be in flux.


#### Others

You may have to install from source.  Let me know how it goes.

#### One-line patch

Under mlpack 2.1.0, the `R CMD check` command may complain about `std::cout`. Use
[this one-line change](https://github.com/eddelbuettel/mlpack/commit/6dd600825395e1bdb0455ad836daefc49b5ca66f)
which is also part of the mlpack 2.1.1 release. Release 2.1.1, 2.2.0 and later are fine.

### Authors

[Qiang Kou](https://www.linkedin.com/pub/qiang-kou/2a/986/6b7) for the main
[RcppMLPACK](https://github.com/thirdwing/RcppMLPACK) package.

[James Balamuta](http://thecoatlessprofessor.com/) for various builds
tricks and tests, in particular on OS X.

Dirk Eddelbuettel for most of the remainder of this experiment.

### License

GPL (>= 2)

---
layout:     post
title:      Improving sampling routines for correlation matrices and R interface
subtitle:   GSoC' 24
date:       2024-06-16
author:     Atrayee Samanta
header-img: img/gsocheader.jpg
catalog: true
tags:
    - gsoc
    - correlation matrices
    - spectrahedrons
    - cpp
    - linear algebra
---

# Introduction
### Overview

My work for GeomScale in the Google Summer of Code 2024 was focused on 2 of GeomScale's repositories- *volesti* and *Rvolesti*.

*volesti* is a generic open source C++ library, with R and Python interfaces. *volesti* implements various algorithms for high-dimensional sampling and volume approximation as well as functions for copula estimation in financial modelling and metabolic network analysis. *Rvolesti* provides an R interface for the C++ library volesti.

### Objectives and Goals

The goal of this project is to
1. improve sampling routines for correlation matrices and
2. further enhance the R interface with sampling correlation matrices of the volesti repository.

 The implementation will involve working with open-source C++ libraries to solve Generalized Eigenvalue problems as well as working with R and Rcpp. This implementation will create more efficient, well-tuned sampling routines for correlation matrices and increased accessibility and ease with which users can utilize these subroutines through volesti's R interface.

# Project Development

- **Improvising subroutines-** My task was to improve a subroutine that computes the smallest possible eigenvalue for a given pair of symmetric matrices. The application is crucial in the uniform sampling of correlation matrices. The pre-existing subroutine used only Eigen routines- I added an implementation of the same function, but using Spectra subroutines, to solve the GEP in each step of the random walk. This is primarily because while Eigen and Spectra can both be used for computation of Generalized Eigen problems, Spectra is lightweight and is specially optimized for this task- it may provide a better performance for large scale GEP computations.

[Merged pull request](https://github.com/GeomScale/volesti/pull/303)

- **Feature: Reusing a piece of code.** As pointed out by my mentor, a piece of code was already existing to check if a matrix was a positive definite one, using LDLT decomposition. I incorporated the required changes to reuse this code.

[Merged pull request](https://github.com/GeomScale/volesti/pull/306)

- **Exposing the sampler to the R interface.** I wrote an Rcpp function to expose the underlying C++ function to the user in the R interface. This C++ function uniformly samples correlation matrices. This was exported after compilation for use in R, I also added documentation and an example use in *Rvolesti*.

[Merged pull request](https://github.com/GeomScale/Rvolesti/pull/26)

- **Modification of the C++ sampler function.** We ran into a problem here: on running the sampler function from R, the printed matrix was asymmetric: the upper triangular half of the matrix consisted of all 0's. This was happening because in the C++ code, it had been assumed that he matrix was symmetric, and all the computations were being done using only the lower triangular half of the matrix. Here, I modified the C++ code so that correct values would be displayed on the upper triangular part of the matrix.

[Merged pull request](https://github.com/GeomScale/volesti/pull/321)

- **Tuning parameters.** I added a function that would tune the walk length parameter required for the uniform sampling of correlation matrices; the efficiency is judged based on 3 parameters- the run time, the univariate PSRF, and the effective sample size.

[Open pull request](https://github.com/GeomScale/volesti/pull/328)

# Future work

I would love to keep contributing to GeomScale! In regards to my project, I would like to especially focus on further tuning other parameters required for sampling of correlation matrices, as well as tune the actual values.

# Conclusion

I believe the project will contribute to the implementation of efficient Bayesian models to learn the covariance matric and to fit a copula on the given data. The project has also made the uniform sampling of correlation matrices accessible to the R interface.

# Acknowledgements

I am very grateful to GeomScale and Google Summer of Code for providing me with this opportunity to grow and learn. I would also like to extend my sincere appreciation to my mentors, Apostolos Chalkis and Vissarion Fisikopoulos, without whose support, careful reviews and guidance, none of this would have been possible.

# Code

Here is a list of all of my pull requests made during the Coding period-
1. [add the spectra routines](https://github.com/GeomScale/volesti/pull/303)
2. [New feature: spectra correlations](https://github.com/GeomScale/volesti/pull/306)
3. [New function to check for correlation matrices](https://github.com/GeomScale/volesti/pull/315)
4. [Fix: symmetric correlation matrix](https://github.com/GeomScale/volesti/pull/321)
5. [New function to tune the walk length parameter](https://github.com/GeomScale/volesti/pull/328)
6. [Implementation of the correlation matrices sampling function in the R interface](https://github.com/GeomScale/Rvolesti/pull/26)

# Appendix

I have also recorded my week-wise contributions in my blog-
1. [Week 1](https://atrayees.github.io/2024/06/02/gsoc24-week1/)
2. [Week 2](https://atrayees.github.io/2024/06/09/gsoc24-week2/)
3. [Week 3](https://atrayees.github.io/2024/06/16/gsoc24-week3/)
4. [Weeks 4 and 5](https://atrayees.github.io/2024/06/30/gsoc24-weeks4and5/)
5. [Weeks 6 and 7](https://atrayees.github.io/2024/07/17/gsoc24-weeks6and7/)
6. [Week 8](https://atrayees.github.io/2024/07/26/gsoc24-week8/)
---
layout: post
author: chayast
date: 2016-08-15
title: Fitting the Correlation Function
published: true
---

Single molecule fluorescence experiments provide a means to study subtle, time-dependent movement, such as structural
rearrangements, which cannot easily be studied in bulk. Such experiments result in many trajectories of the observed
intensity over time from which useful information needs to be extracted. The trajectories can be used to calculate the
correlation function that describes the correlation between points as a function of lag time. The correlation function
contains all of the essential information of the trajectories in compact form and can be used to extract relaxation
timescales in a model free way. Since the timescales for slow processes are invariant to the system, these timescales
can be compared to timescales extracted through other means as a way to invalidate proposed kinetic models.

The theoretical correlation function can be expressed as a sum of exponential $$ \mathbf C(t) = \sum_{i=1}^N A_i e^{-t/\tau_i } $$. The correlation function can also be computed from Markov State Models that were parameterized with many Molecular Dynamics trajectories as described in [this](http://www.sciencedirect.com/science/article/pii/S0301010411003892) paper, thus providing a way to directly connect simulations with experiments. 

The naïve approach to find $$\mathbf {A_i} $$ and $$\mathbf {τ_i} $$ from the experimental correlation function is to use Ordinary Least Squares (OLS) to fit the experimental CF to this sum. If the error is Gaussian distributed and the errors are uncorrelated, OLS reduces to the Maximum Likelihood Estimate
(MLE). However, in many time-series data, the assumption that the errors are uncorrelated is violated since the CF is
calculated from the same trajectory and the points are not independent. In [Lomakin’s paper “Fitting the Correlation
Function”] (http://web.mit.edu/physics/benedek/ArticlesMore/FitCorFunc2001.pdf) the problem is cast as a Gaussian
Process with a covariance function given by the sum of exponential. For
large N, Lomakin shows that the Gaussian Process approach reduces to the Generalized Least Squares (GLS) where the
covariance matrix accounts for the correlation between the points when fitting the correlation function.

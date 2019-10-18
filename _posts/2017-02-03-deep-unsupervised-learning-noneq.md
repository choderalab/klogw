---
layout: post
author: pgrinaway
title: Discussion of "Deep Unsupervised Learning using Nonequilibrium Thermodynamics"
date: 2017-02-03
published: false
tags:
  - generative models
  - nonequilibrium
  - markov chains
---

# Computational Statistics Club Paper discussion:
On 2/3/2017 we discussed the paper "Deep Unsupervised Learning using Nonequilibrium Thermodynamics", by Jascha Sohl-Dickstein, available [here](https://arxiv.org/abs/1503.03585). This paper presents an interesting twist on learning generative models that preserves two very important quantities: tractability (that is, the ability to evaluate and sample from the probability density you've learned) and flexibility (the ability to represent accurately the rich structure that is found in complex data). It accomplishes this by devising a forward and reverse diffusion process that transform the data into a simple tractable distribution and back.


## What interesting applications could be based on the work in this paper?

In addition to discussing the theory itself, the group also brainstormed several thoughts about where we could apply this work:

* The method could be used to generate molecular configurations in a tractable way
    * This would require the ability to condition the density that we learn.
    * This would also require transdimensional extensions
* Can we create alchemical intermediates this way?
* Perhaps this can be used to define a proposal distribution
    * Set in the framework of adaptive MCMC, this method might be able to assist sampling of intractable densities.


## What questions/comments are left after the discussion?

As is often the case when discussing significant papers, there are often a few questions remaining. During the discussion, the following questions and comments arose:

* Performance comparison on datasets besides images
    * Although we understand this is a critical application of ML, it would be fascinating to see how techniques such as this perform on, _e.g._, molecular configurational probability distributions.

* Is log likelihood the best way to assess the performance of generative models?
    * In probabilistic modeling, the log likelihood of the data is often used to assess model quality. However, is this the best approach, given that for certain classes of models it is very difficult to evaluate?

* If each step is very ill-conditioned, are there conditions on how stably you can compute these long chains of conditional probabilities?
    * Do we need to have a "good guess" of the protocol to begin stable training, much like we do with AIS?

* Is training numerically stable?

* How does the approach scale with dimensionality and complexity of the underlying distribution?

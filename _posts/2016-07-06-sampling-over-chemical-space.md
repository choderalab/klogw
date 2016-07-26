---
layout: post
author: jchodera
date: 2016-08-01
title: Sampling over chemical space
published: false
---

Man has long dreamed of sampling over chemical space in a single simulation.

-----

### The algorithm

Consider the following sampling algorithm:

* Presume we currently have a sample $(x,k) \sim \pi(x,k)$
* Draw a new chemical species $k'$ given the current species $k$
$$ k' \sim p(k' | k) $$
* Fragment the molecule $x$ into $(x_{core}, x_{old})$
* Use NCMC to propose $(x^{(1)}_{core}, x^{(1)}_{old}$ switching from potential $\pi$ to $\pi_{delete}$
$$ (x^{(1)}_{core}, x^{(1)}_{old}) \sim \rho_{\pi \rightarrow \pi_{delete}} $$
* Use RJMC to propose $x^{(1)}_{new}$ using potential $\pi_{RJMC}$
$$ x^{(1)}_{new} | x^{(1)}_{core}

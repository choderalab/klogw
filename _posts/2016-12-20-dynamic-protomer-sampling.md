---
layout: post
author: jchodera
title: Dynamic protomer sampling
date: 2016-12-20
published: true
tags:
  - algorithms
  - protonation state effects
  - protomer
  - tautomer
  - molecular dynamics
---
We recently submitted an [R01 grant proposal](https://grants.nih.gov/grants/funding/r01.htm) to the NIH focused on removing a major limitation to the accuracy of alchemical free energy calculations: Incorporating fully consistent treatments of protonation states and tautomers for both ligands and proteins, especially in key systems like kinase:inhibitor binding.
This post reviews some of the theory and describes the algorithm we've come up with to make this efficient.

<!--more-->

Here, we use the term *protomer* to denote either protonation states or tautomers of small molecules or amino acid residues.

This scheme builds on the pioneering work in Monte Carlo sampling of discrete protonation states by John Mongan, David Case, and Andy McCammon in [implicit solvent](https://dx.doi.org/10.1002/jcc.20139), as well as Harry Stern in [explicit solvent](http://dx.doi.org/10.1063/1.2731781).
More recently, Benoit Roux has made significant refinements to the [explicit solvent](http://dx.doi.org/10.1021/acs.jctc.5b00261) approach using theoretical advances from our [NCMC](http://dx.doi.org/10.1073/pnas.1106094108) paper.

<!-- See https://eduardoboucas.com/blog/2014/12/07/including-and-managing-images-in-jekyll.html for info on including images -->

{% include image.html name="constant-pH-method-overview.jpg" %}

For both **amino acids** and **small molecules**, we must first compute the aqueous relative free energies of each protomer species at the pH of interest, \\( \Delta G_{aq}^{\bf pH} \\).
For amino acids, we use reference p$K_as$ for amino acids in isolation, and compute \\( \Delta G_{aq}^{\bf pH} \\) at the pH of interest with the Henderson-Hasselbalch equation (or variants).
For small molecules, we use a protomer aqueous population prediction tool (such as [Epik](https://www.schrodinger.com/epik)) to estimate \\( \Delta G_{aq}^{\bf pH} \\), avoiding the uncoupling of titratable sites within the molecule that may be coupled by electronic effects, as in the quinazoline ring shown here.
For each titratable entity (small molecule or ionizable residue in a reference state matching the reference p\\(K_a\\)), an explicit solvent **SAMS calibration of protomer log weights** follows, which uses a [Gibbs sampling framework](http://dx.doi.org/10.1063/1.3660669) in which positions \\(\bf x\\) are updated by some form of (possibly Metropolized) molecular dynamics, protomer states \\(\bf s\\) are updated by [NCMC](http://dx.doi.org/10.1073/pnas.1106094108), and the optimal [SAMS (self-adjusted mixture sampling)](http://stat.rutgers.edu/home/ztan/Publication/SAMS_preprint.pdf) update scheme is used to update the log weights \\({\bf g}\\) (here using the function \\(f\\)).
SAMS is an adaptive single-replica [expanded ensemble method](http://aip.scitation.org/doi/abs/10.1063/1.462133) similar in spirit to [Wang-Landau](https://dx.doi.org/10.1103%2FPhysRevLett.86.2050) or [simulated scaling](https://dx.doi.org/10.1063/1.2982161), but has a provably asymptotically consistent and optimal convergence properties, a major advance over earlier methods.
This calibration step uses equal SAMS target probabilities for each protomer species to ensure the relative molecular mechanics free energies of each species are estimated to \\(O(k_B T)\\), so that these can be removed in the correction step.
To preserve charge neutrality, water molecules are reversibly turned into monovalent counterions.
This calibration step **must** be repeated whenever the forcefield, water model, electrostatics model (e.g. PME), temperature, or pressure are changed, or else the correct protomer populations will no longer be obtained if a constant-pH simulation is run for the amino acid or small molecule in isolation in solvent.
Next, we perform **pH correction of log weights**, in which we remove the residual bias from the relative molecular mechanics free energies in explicit solvent (using the SAMS-derived log weights \\({\bf g}_s^o\\) and \\(\pi_s^o\\), the empirical probability of observing protomer \\(s\\) in the SAMS simulation), to ensure the weight will generate the appropriate input protomer distribution in solvent constant-pH simulations.
This can be used to compute the pH-corrected log weight for any pH of interest without additional SAMS calibration.
Finally, in **MCMC protonation state sampling**, all titratable entities of interest (residues and small molecules) are included in the simulation, and we simulate use a modified version of the same Gibbs sampling updates we used in calibration, except that the pH-corrected log weights \\({\bf g}(\mathsf{pH})\\) are fixed throughout the simulations.
The overall distribution sampled is an expanded ensemble \\(\pi({\bf x}, {\bf s})\\) that includes the protomer states \\({\bf s}\\) of all titratable entities in the system.

{% include image.html name="ncmc-efficiency.jpg" %}

This scheme can be surprisingly effective:
*(A)* While explicit solvent acceptance rates for instantaneous Monte Carlo protomer change proposals are vanishingly small (\\(\sim 10^{-24} \\)), NCMC proposals with [GHMC (Metropolized Langevin) integration](https://arxiv.org/abs/1006.4914) to eliminate [shadow work](https://doi.org/10.1103/PhysRevX.3.011007) boost acceptance rates by a factor of \\(\sim 10^{23}\\) for switching times of 10 ps or more.
Error bars denote 95% confidence intervals.
*(B)* Calibration of imatinib uncorrected protomer log weights in explicit solvent using SAMS rapidly converge to \\(\sim 1 \: k_B T\\) in 4000 iterations of 1 ps of GHMC propagation followed by a 10 ps NCMC protomer switch proposal.
*(C)* Abl:imatinib complex showing ionizable residues observed to change protonation state in 2 ns of MD/MC simulation in explicit solvent at pH 7.4.
*(D)* Accepted protonation state changes for ionizable residues in Abl kinase at pH 7.4.

We are integrating the constant-pH scheme above into the existing MD/MC replica propagation scheme used within [YANK](http://getyank.org), our alchemical free energy code.
To complete our hybrid constant-pH alchemical methodology, we need two additional components:
(1) an updated alchemical replica-exchange scheme for the constant-pH ensemble,
and (2) an updated analysis method to extract pH-dependent free energy estimates.
For (1), we first define an appropriate *reduced potential* \\(u(x,{\bf s};\lambda,\mathsf{pH})\\), where the equilibrium distribution sampled by a given replica is \\(\pi(x,{\bf s};\beta, p, \lambda,\mathsf{pH}) \propto e^{-u(x,{\bf s};\beta, p, \lambda,\mathsf{pH})}\\), with \\(x\\) the current instantaneous configuration, \\({\bf s} \in \mathbb{Z}^N\\) an integer vector specifying the current protonation state of all \\(N\\) titratable sites (across small molecules and protein), \\(\beta = 1/(k_B T)\\) the inverse temperature, \\(p\\) pressure, \\(\lambda \in [0,1]\\) the alchemical parameter, and pH the current pH:

$$ u(x,{\bf s};\beta, p, \lambda,\mathsf{pH}) \equiv \beta \left[ U(x;{\bf s}, \lambda) + p V(x) + g_{\bf s}(\mathsf{pH}) \right] $$

where \\(U(x;{\bf s},\lambda)\\) is the potential energy function that depends on both protomers \\({\bf s}\\) and alchemical parameter \\(\lambda\\), \\(V(x)\\) the current volume, and \\(g_{\bf s}(pH)\\) is the pH-corrected log weights.
If velocities are regenerated from the Maxwell-Boltzmann distribution at the beginning of each Hamiltonian exchange iteration, the acceptance criteria is

$$ P_\mathrm{exchange}( ({\bf x}_i, {\bf s}_i),  ({\bf x}_j, {\bf s}_j)) = \min \left\{ 1, \frac{ e^{-u(x_j,{\bf s}_j;\beta_i, p_i, \lambda_i,\mathsf{pH}_i) - u(x_i,{\bf s}_i;\beta_j, p_j, \lambda_j,\mathsf{pH}_j)}  }{ e^{-u(x_i,{\bf s}_i;\beta_i, p_i, \lambda_i,\mathsf{pH}_i) - u(x_j,{\bf s}_j;\beta_j, p_j, \lambda_j,\mathsf{pH}_j)} } \right\} $$

For (2), the free energy contribution for each alchemical leg is optimally estimated using the [multistate Bennett acceptance ratio (MBAR)](http://dx.doi.org/10.1063/1.2978177) method we developed to analyze equilibrium data from arbitrary equilibrium thermodynamic states, by solving the self-consistent equations for the dimensionless free energies \\(\hat{f}_i\\), \\(i = 1,\ldots,K\\),

$$ \hat{f}_i \equiv - \ln \sum_{k=1}^K \sum_{n=1}^{N_k} \left[ \sum_{l=1}^K \exp\left\{\hat{f}_l - [u(x_{kn}, {\bf s}_{kn}; \beta_l, p_l, \lambda_l, \mathsf{pH}_l) - u(x_{kn}, {\bf s}_{kn}; \beta_i, p_i, \lambda_i, \mathsf{pH}_i)]\right\}  \right]^{-1} $$

where there are \\(N_k\\) samples \\(x_{kn}\\) from each of \\(K\\) alchemical states.
This self-consistent set of equations can be efficiently solved in several ways; we make use of [pymbar](http://pymbar.org) to do this.
The free energy difference between the fully interacting state (\\(\lambda_1=1, \beta_1=\beta, p_1=p, pH_1=pH\\)) and noninteracting state (\\(\lambda_K=0, \beta_K=\beta, p_K=p, pH_K=pH\\)) at the inverse temperature, pressure, and pH of interest is simply given by \\(\Delta \hat{f}_{1K} \equiv f_K - \hat{f}_1\\).
Note that the Gibbs sampling replica exchange and MBAR framework is sufficiently general to permit the use of *multiple* pH values in a single simulation, allowing the estimation of binding affinities over a pH range of interest or pH sensitivities (\\(\partial \Delta f / \partial \mathsf{pH}\\)).
Indeed, including multiple pH values in a hybrid approximate implicit/explicit replica exchange method has been found to [improve mixing](http://dx.doi.org/10.1021/ct401042b).

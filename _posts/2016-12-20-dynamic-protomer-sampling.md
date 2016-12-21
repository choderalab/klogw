---
layout: post
author: jchodera
title: Protomer/tautomer sampling for small molecules and proteins in molecular simulations
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

{% include image name="constant-pH-method-overview.jpg" caption="Overview of the dynamic protomer/tautomer sampling scheme." %}

From left to right:
For both **amino acids** and **small molecules**, we must first compute the aqueous relative free energies of each protomer species at the pH of interest, \Delta G_{aq}^{\bf pH}$.
For amino acids, we use reference p$K_as$ for amino acids in isolation, and compute $\Delta G_{aq}^{\bf pH}$ at the pH of interest with the Henderson-Hasselbalch equation (or variants).
For small molecules, we use a protomer aqueous population prediction tool (such as [Epik](https://www.schrodinger.com/epik)) to estimate $\Delta G_{aq}^{\bf pH}$, avoiding the uncoupling of titratable sites within the molecule that may be coupled by electronic effects, as in the quinazoline ring shown here.
For each titratable entity (small molecule or ionizable residue in a reference state matching the reference p$K_a$), an explicit solvent **SAMS calibration of protomer log weights** follows, which uses a [Gibbs sampling framework](http://dx.doi.org/10.1063/1.3660669) in which positions ${\bf x}$ are updated by some form of (possibly Metropolized) molecular dynamics, protomer states ${\bf s}$ are updated by [NCMC](http://dx.doi.org/10.1073/pnas.1106094108), and the optimal [SAMS (self-adjusted mixture sampling)](http://stat.rutgers.edu/home/ztan/Publication/SAMS_preprint.pdf) update scheme is used to update the log weights ${\bf g}$ (here using the function $f$).
SAMS is an adaptive single-replica [expanded ensemble method](http://aip.scitation.org/doi/abs/10.1063/1.462133) similar in spirit to [Wang-Landau](https://dx.doi.org/10.1103%2FPhysRevLett.86.2050) or [simulated scaling](https://dx.doi.org/10.1063/1.2982161), but has a provably asymptotically consistent and optimal convergence properties, a major advance over earlier methods.
This calibration step uses equal SAMS target probabilities for each protomer species to ensure the relative molecular mechanics free energies of each species are estimated to $O(k_B T)$, so that these can be removed in the correction step.
To preserve charge neutrality, water molecules are reversibly turned into monovalent counterions.
This calibration step **must** be repeated whenever the forcefield, water model, electrostatics model (e.g. PME), temperature, or pressure are changed, or else the correct protomer populations will no longer be obtained if a constant-pH simulation is run for the amino acid or small molecule in isolation in solvent.
Next, we perform **pH correction of log weights**, in which we remove the residual bias from the relative molecular mechanics free energies in explicit solvent (using the SAMS-derived log weights ${\bf g}_s^o$ and $\pi_s^o$, the empirical probability of observing protomer $s$ in the SAMS simulation), to ensure the weight will generate the appropriate input protomer distribution in solvent constant-pH simulations.
This can be used to compute the pH-corrected log weight for any pH of interest without additional SAMS calibration.
Finally, in **MCMC protonation state sampling**, all titratable entities of interest (residues and small molecules) are included in the simulation, and we simulate  use a modified version of the same Gibbs sampling updates we used in calibration, except that the pH-corrected log weights ${\bf g}(\mathsf{pH})$ are fixed throughout the simulations.
The overall distribution sampled is an expanded ensemble $\pi({\bf x}, {\bf s})$ that includes the protomer states ${\bf s}$ of all titratable entities in the system.

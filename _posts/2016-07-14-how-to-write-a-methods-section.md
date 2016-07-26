---
layout: post
title: How to write a Methods section
date: 2016-07-25
published: true
tags:
  - writing
  - papers
  - methods
author: jchodera
---

Our relatively young group has nine graduate students, and a number of them are starting to write up their first manuscripts for submission.
To help prepare them to be better writers and communicators in the primary medium of our trade---scientific publications---I've started to present some useful pointers on how to actually go about organizing and writing a manuscript to be submitted as a scientific journal article as part of our Hot Topics series.
I'm sharing this material in case others find it useful!
Please feel free to provide feedback or suggestions as to how we can improve these resources and suggestions!

## Resources for writing papers

- Always read journal guidelines for authors first: They contain useful information
  - [JACS Information for Authors](http://pubs.acs.org/page/jacsat/submission/authors.html)
  - [PNAS Procedures for Submitting Manuscripts](http://www.pnas.org/site/authors/procedures.xhtml)
  - [Nature Communications Guide to Authors](http://www.nature.com/ncomms/authors/index.html)
- The [ACS Style Guide](http://pubs.acs.org/isbn/9780841239999) contains a wealth of good information on how to plan and write good scientific papers

----

Traditionally, papers had rigid structure: Intro, Methods, Results, Conclusions. Not necessary with modern formats: tell story without getting too many irrelevant details in he way, but include complete details.

## Examples of Methods sections:

### Computational
* Our [Ensembler paper](http://www.choderalab.org/s/Ensembler-enabling-high-throughput-molecular-simulations-at-the-superfamily-scale.pdf) clearly describes the reasoning behind each step of the protocol.
* Our [automatic equilibration detection paper](http://www.choderalab.org/s/A-simple-method-for-automated-equilibration-detection-in-molecular-simulations.pdf) first explains the results, keeping experimental details minimal, and relegates the detailed computational methodology to a later section.
* Our [automatic state decomposition paper](https://choderalab.squarespace.com/s/automatic-discovery-of-metastable-states-for-the-construction-of-markov-models-of-macromolecular-con.pdf) first lays out the reasoning for the overall experimental design before describing the specific protocol in detail.

### Experimental
* Our [ITC Worksheet paper](http://www.choderalab.org/s/itc-worksheet.pdf) clearly describes the reason behind each step of the protocol, as well as why alternative approaches were not chosen. This paper is intended to be more didactic than most, but the idea is still reasonable.

----

## Be empathic!

* Think about the poor grad student that will have to reproduce or build on your work.
Help them out by making it easy for them to understand, reproduce, and extend your work!
* Be empathic toward reviewers: don't make your readers hunt through a dozen papers for the key details, but do cite earlier work.

----

## Provide sufficient detail

You will need to provide sufficient detail for a competent researcher in the field to reproduce it.
* Document everything you did in detail
* Send to someone in another lab to read through to see if you have provided enough information for them to reproduce your work
* Post to a preprint server and see if people have questions about information you may have left out

----

## Keep the flow clean

Don't clog up the main flow of the story with irrelevant details, but be complete by including what is needed somewhere (e.g., separate Detailed Methods section, Appendix, etc.)

----

## Writing a computational methods section
* What's the point?
  - ensure someone understands what you did and WHY you did it; make sure you explain WHY! This is often neglected.
  - ensure a competent practitioner can reproduce
* Describing a simulation:
  - how was the system prepared? Where did starting coordinates come from?
  - how were parameters assigned?
  - how was minimization and equilibration done?
  - document software (vendor if applicable) and versions
  - only need to document non default options, but best to make a statement that default options were used unless otherwise specified
  - give credit; cite papers that go along with tools, forcefields, structures (if possible)
  - explain how error estimates were computed and what they represent (eg standard error of the mean)

* What's the best way to do capture the info you need to write the methods section?
  - automate it: use a script to perform the whole process
    - exact record of what you did that you can turn into text later
    - you can fix it and repeat it without a lot of manual work if there was an issue
    - you can share the script and others can reproduce it if there is ever any question

----

## Writing an experimental methods section
* Describe all materials used and steps performed, along with why!
* Include Supplier, Product, and lot number for materials used
* standards for characterization and documentation often published by journals (e.g., [JACS Guidelines for Characterization of Organic Compounds](http://pubs.acs.org/page/jacsat/submission/org_character.html))
* make primary data (eg .itc files) available online and/or supplementary data (or both: supplementary data likely to be resilient, but online can be updated if we find some problems or a way to make the data more useful)
* sharing huge tables of numbers ONLY as a PDF is not acceptable!
* Some great lessons in [this article from Bruce Gibb](http://www.nature.com/nchem/journal/v6/n8/full/nchem.2017.html)

----

## Is there a "reproducibility crisis" in science?

Recently, journals (like the Nature Publishing Group) have devoted a lot of thought and discussion to the "reproducibility crisis" in science.
- [Nature Publishing Group papers](http://www.nature.com/news/reproducibility-1.17552)
- An example on removing center-of-mass velocities

## Can we go further to make work more reproducible?
* [conda](http://conda.pydata.org/) allows easy reproducibility: An example from our [automatic equilibration detection paper](https://github.com/choderalab/automatic-equilibration-detection/blob/master/examples/liquid-argon/reproduce.sh)
* [Docker images](https://www.docker.com/) can be deployed via [Docker hub](https://hub.docker.com/r/jchodera/docker-fah-client/) and can guard against upstream package changes or OS differences

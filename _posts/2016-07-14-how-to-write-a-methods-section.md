---
layout: post
author: jchodera
title: How to write a Methods section
date: 2016-07-28
published: true
tags:
  - writing
  - papers
  - methods
---

Our relatively young group has nine graduate students, and a number of them are starting to write up their first manuscripts for submission.
To help prepare them to be better writers and communicators in the primary medium of our trade---scientific publications---I've started to present some useful pointers on how to actually go about organizing and writing a manuscript to be submitted as a scientific journal article as part of our Hot Topics series.
I'm sharing this material in case others find it useful!
Please feel free to provide feedback or suggestions as to how we can improve these resources and suggestions!

<!--more-->

## Where should I start?

There are some great resources available for writing scientific papers:

- Always read journal guidelines for authors first! They contain useful information about what kinds of manuscripts are accepted, as well as formatting requirements, required content, and guidelines for presenting or organizing certain kinds of information:
  - [JACS Information for Authors](http://pubs.acs.org/page/jacsat/submission/authors.html)
  - [PNAS Procedures for Submitting Manuscripts](http://www.pnas.org/site/authors/procedures.xhtml)
  - [Nature Communications Guide to Authors](http://www.nature.com/ncomms/authors/index.html)
- The [ACS Style Guide](http://pubs.acs.org/isbn/9780841239999) contains a wealth of good information on how to plan and write effective scientific research articles.

----

## Guidelines for writing a Methods section

### Choose a structure that works for the material you want to present.

Traditionally, papers in chemistry or biophysics have a relatively rigid structure:
* Introduction
* Methods
* Results
* Conclusions

Conforming to such a rigid structure often makes it difficult to tell the scientific story.
Rigidly conforming to this structure is not always necessary with modern journal formats.
You can often find a structure that allows you to tell the story without having too many irrelevant details get in the way, making sure to include complete details in an appropriate location so that the work is fully reproducible by other scientists and evaluable by refrees.

### Be empathic!

* Think about the poor grad student that will have to reproduce or build on your work.
Help them out by making it easy for them to understand, reproduce, and extend your work!
* Be empathic toward reviewers: don't make your readers hunt through a dozen papers for the key details, but do cite earlier work.

### Provide sufficient detail for someone to reproduce your work.

You will need to provide sufficient detail for a competent researcher in the field to reproduce it.

* Document everything you did in detail
* Send to someone in another lab to read through to see if you have provided enough information for them to reproduce your work
* Post to a preprint server and see if people have questions about information you may have left out

### Keep the flow clean.

Don't clog up the main flow of the story with irrelevant details, but be complete by including what is needed somewhere (e.g., separate Detailed Methods section, Appendix, etc.).

----

## Examples of good Methods sections

The best way to get a feel for what makes a *good* Methods section is to read a lot of them, and judge for yourself which structures work well in effectively communicating what was done and why.
Here are a few papers we have written that we feel are good examples of different strategies for doing this:

### Computational papers
* Our [Ensembler paper](http://www.choderalab.org/s/Ensembler-enabling-high-throughput-molecular-simulations-at-the-superfamily-scale.pdf) clearly describes the reasoning behind each step of the protocol.
* Our [automatic equilibration detection paper](http://www.choderalab.org/s/A-simple-method-for-automated-equilibration-detection-in-molecular-simulations.pdf) first explains the results, keeping experimental details minimal, and relegates the detailed computational methodology to a later section.
* Our [automatic state decomposition paper](https://choderalab.squarespace.com/s/automatic-discovery-of-metastable-states-for-the-construction-of-markov-models-of-macromolecular-con.pdf) first lays out the reasoning for the overall experimental design before describing the specific protocol in detail.

### Experimental papers
* Our [ITC Worksheet paper](http://www.choderalab.org/s/itc-worksheet.pdf) clearly describes the reason behind each step of the protocol, as well as why alternative approaches were not chosen. This paper is intended to be more didactic than most, but the idea is still reasonable.

----

## Writing a computational Methods section

* Make sure you understand the *point* of the Methods section first:
  - A Methods section should communicate both *what* you did and *why* you did it.
    Make sure you explain *why*, rather than leave the reader guessing!
    Surprisingly, the *why* is often neglected, which makes it harder for you to determine which steps are critical.
  - A Methods section should allow a competent practitioner in your field to reproduce the work and obtain the same results (to within the quoted statistical error).
* When describing a simulation, ensure someone else can reproduce what you did.
  - How was the system prepared? Where did starting coordinates come from?
  - How were parameters assigned?
  - How was the system minimized and equilibrated?
  - Document software (vendor if applicable) and versions
  - Only need to document non default options, but best to make a statement that default options were used unless otherwise specified
  - Give credit; cite papers that go along with tools, forcefields, structures (if possible)
  - Explain how error estimates were computed and what they represent (e.g., standard error of the mean)
* What's the best way to do capture the info you need to write the methods section?
  - Automate it: use a script to perform the whole process
    - Exact record of what you did that you can turn into text later
    - You can fix it and repeat it without a lot of manual work if there was an issue
    - You can share the script and others can reproduce it if there is ever any question

----

## Writing an experimental Methods section

* Describe all materials used and steps performed, along with why!
* Include Supplier, Product, and lot number for materials used
* standards for characterization and documentation often published by journals (e.g., [JACS Guidelines for Characterization of Organic Compounds](http://pubs.acs.org/page/jacsat/submission/org_character.html))
* make primary data (eg .itc files) available online and/or supplementary data (or both: supplementary data likely to be resilient, but online can be updated if we find some problems or a way to make the data more useful)
* sharing huge tables of numbers ONLY as a PDF is not acceptable!
* Some great lessons in [this article from Bruce Gibb](http://www.nature.com/nchem/journal/v6/n8/full/nchem.2017.html)

----

## Is there a "reproducibility crisis" in science?

Recently, a number of journals have devoted a lot of thought and discussion to the "reproducibility crisis" in science.
Some of these problems have been blamed on poor practices for presenting research methodologies.
The Nature Publishing Group has collected a number of perspectives and examinations of the subject here:

* [Nature Publishing Group: Reproducibility](http://www.nature.com/news/reproducibility-1.17552)

## Can we go further to make work more reproducible?

New technology makes it easier to share the *exact* version of the code to perform the study and analyze the data, and potentially even isolate against upstream software issues by emulating the platform on which the work was performed:

* [conda](http://conda.pydata.org/) allows easy reproducibility because packages can be pinned to specific versions. Our recent [automatic equilibration detection paper](https://github.com/choderalab/automatic-equilibration-detection/blob/master/examples/liquid-argon/reproduce.sh) that utilizes the [Omnia suite](http://omnia.md)
* [Docker images](https://www.docker.com/) can be deployed via [Docker hub](https://hub.docker.com/r/jchodera/docker-fah-client/) and can guard against upstream package changes or OS differences

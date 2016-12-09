---
layout: post
author: jchodera
title: Response to NIH RFI on Interim Research Products
date: 2016-12-09
published: true
tags:
  - NIH
  - preprints
  - RFI
---
The NIH has posted an [RFI on including "preprints and interim research products" in NIH applications and reports](http://grants.nih.gov/grants/guide/notice-files/NOT-OD-17-006.html).
Many others have provided responses to this RFI that they have shared publicly, including thoughtful responses from [David Mobley](http://mobleylab.org/2016/11/17/where-do-we-hope-publishing-is-headed-preprints-and-interim-research-products/), [Steven Floor](https://twitter.com/stephenfloor/status/806986294127538176), and others that have been collected by [ASAPBio](http://asapbio.org/nih-rfi).
Here is my response, written very quickly on a train ride back from the NIH.

<!--more-->

### Types of interim research products your or your organization create/and or host

My laboratory in a mixed wet/dry laboratory. Interim research projects my laboratory both creates and uses are:

* Preprints and other prepublication sharing mechanisms
    * We regularly post manuscripts to [bioRxiv](http://biorxiv.org) or [arXiv](http://arxiv.org) ahead of submission in order to solicit community feedback prior to submission. We find this process has universally improved the quality of our submitted manuscripts and allowed others to make immediate use of our work.
* Open source software:
    * Engineering a new piece of software needed for a publication or research project often generates several interim software modules that are useful on their own but may are not used to generate publictions in isolation. Because our lab has an "open first" policy (developing unpublished code publicly on GitHub) and engages in good software development practices (using revision control, continuous integration, unit testing, and frequent point and bugfix releases), these software modules are useful to others, and may even find use by others and assist in generating data reported in publications by others before the primary publication is complete.
    * To facilitate citability of these interim research projects, we now use [Zenodo's GitHub integration feature](https://guides.github.com/activities/citable-code/) to assign citable DOIs to software point releases.
* Plasmids
    * We make plasmids we generate availeble ahead of publication through [AddGene](https://www.addgene.org). We recently made a [library of 68 plasmids available through AddGene](https://www.addgene.org/kits/chodera-kinase-domains/) in conjunction with a [bioRxiv preprint](http://dx.doi.org/10.1101/038711) while finishing the collection of additional data to be included in the manuscript that will eventually be submitted for publication in a journal. This allows other laboratories to make use of our work in advance of the final publication.
* Simulation datasets
    * Our laboratory generates very large biomolecular simulation datasets using [Folding@home](http://foldingathome.stanford.edu), frequently generating several milliseconds of aggregate simulation data up to several TB in size. We often make simulation datasets available ahead of publication since these are often highly valuable to multiple laboratories interested in performing alternative analyses.
* Experimental datasets
    * We often make experimental datasets available ahead of publication as well, since these can also be useful on their own. For example, for the plasmid set above, we have made the bacterial expression dataset [available and browsable even in advance of publication](http://choderalab.github.io/kinome-data/kinase_constructs-addgene_hip_sgc.html)
* Online tutorials, educational material, and input files
    * We contribute to community and educational websites, such as [alchemistry.org](http://alchemistry.org)
* 3D printed parts
    * We have shared a number of 3D printed parts designed by members of our laboratory both on [the laboratory website](http://www.choderalab.org/3dparts/) and on the [NIH 3D Print Exchange](http://3dprint.nih.gov/users/choderalab).

2. Feedback on what are considered to be interim research products, and how they are used in your field

* Preprints and other prepublication sharing mechanisms
    * We regularly read and share bioRxiv and arXiv preprints. In some fields (such as machine learning), the rapid rate at which the field advances means that arXiv is a primary venue for reporting new work.
* Open source software:
    * We may use software components from other laboratories in our own research projects before the component is reported in a publication.
* Plasmids
    * We consume plasmids from other laboratories that may not yet yet have reported these in a publication. These plasmids or plasmid sets can be enormously valuable.
* Simulation datasets
    * While simulation datasets are not typically made available in our field due to a lack of facile data-sharing mechanisms (suggesting there is an opportunity for the NIH to facilitate this), many labs do make use of the datasets we share
* Experimental datasets
    * Experimental datasets---and especially primary data---is of great value for analyzing the data using new techniques to extract new information or examine the data in light of an alternative hypothesis. Data sharing in machine-readable standards (and not tables in PDF format) should be encouraged.
* Online tutorials, educational material, and input files
    * Many people find these to be of great value
* 3D printed parts
    * As 3D printers become ubiquitous, the value of these downloadable parts will increase.
* Protein structures in the RCSB
    * The utility of the RCSB as a resource for biology is unquestionable.

### Insight on how particular types of interim research products might impact the advancement of science

There is great value in many interim research products. Peer-reviewed publications are not error-free and endowed with value simply by virtue of the fact that they have passed through the peer review process---to think otherwise is nothing short of "magical thinking" that does nothing but damage our field and hinder progress. The current publication peer review system is highly flawed, and may do little to ultimately enhance ultimate research quality or accuracy. Instead, the main factors governing the utility of research products (interim and final) are:

* Relevance: Is the research product relevant to my project, and provide important information that informs my own work?
* Timeliness: Is the research product available when I'm looking for it?
* Quality: Does the research product meet the standards of quality I expect from the field, especially in terms of the data I need to address my question? I am a much better judge of this than a reviewer without knowledge of my specific research question.
* Trustworthiness: Is the research from a trustworthy scientist or laboratory?

The timeliness factor is not to be underestimated. In particular, it is critical to consider the overall cost of delaying release of a publication and its data by a lengthy multi-month review process.

### Feedback on potential citation standards

* It is now relatively straightforward to assign DOIs to interim research projects such as software (e.g. [Zenodo](http://zenodo.org)), datasets or figures (e.g. FigShare), many other forms of interim research products. DOIs should become a standard requirement.
* Authors or major contributors should continue to be listed so that they may get credit for their work.

Should the NIH consider trying to define their own standard, as they have with PMIDs or PMCIDs, I would encourage them to first read this XKCD comic:

http://xkcd.com/927/

### Insight on the possible need and potential impact of citing interim products on peer review of NIH applications

For early-career scientists (where time to receiving first NIH funding is a major factor in career progression) or any time-sensitive research, it is critical that the NIH allow interim products to be cited in NIH applications as evidence of productivity leading up to grant submissions.

### Advice on how NIH reviewers might evaluate citations of interim research products in applications

NIH reviewers should evaluate interim research products the same way they should evaluate any research project: By the quality, utility, and significance of the scientific results presented in it.
How else are would they evaluate research products? By looking to see if they are published in Nature or Science? Because if that is what is currently happening, we have a major problem not with how reviewers should evaluate interim research projects, but how reviewers evaluate all research products.

### Any other relevant information

Rather than delicately approaching the issue of pre-publication products (instead of wholeheartedly embracing them), the NIH should develop a strategy to cope with the impending demise of the journal enterprise, and whether the NIH can do more to accelerate this process. [Elsevier alone has an annual publishing revenue of $25.2B](https://medium.com/@jasonschmitt/can-t-disrupt-this-elsevier-and-the-25-2-billion-dollar-a-year-academic-publishing-business-aa3b9618d40a#.rskqys5kh), which is nearly the size of the NIH extramural budget. Elimination of the flawed peer-review journal system and establishment of an NIH-supported post-publication review or rating system that rapidly disseminated NIH-funded scientific results would save an enormous amount of money and drastically increase scientific productivity.

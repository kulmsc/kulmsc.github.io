---
layout: page
title: rare_variants
permalink: /rare_variants/
---


# Discovery and Association of Pathogenic Cardiac Rare Variants

## Introduction

The primary tool for analyzing genetic data is a linear regression.  While the regression may be in many different forms, fundamentally what is happening is a comparison of a genetic-type vector against some outcome or phenotype.  This process is flexible and powerful, however begins to break down when the genetic-type vector is skewed to one class.  Or in other words, the vast majority of individuals have one type of genetic identity (at that variant) while a small group of individuals have another type of genetic identity. While there is a good mathematical explanation to back this up, the underlying reason we have difficulty analyzing this skewed dataset is that it represents a small sample size.  The few people with the rare variant could have a disease because of the variant, or perhaps their disease status is just random chance - with a small sample size it is difficult to determine which is true.

A single rare variant can impart a relatively huge impact on a relativey tiny number of individuals.  Therefore, we cannot give up on identifying whether a rare variant does impact or lead to disease just because regression does not work.  Rather we need to develop other tools.  In this research pursuit two general types of identification methods have been developed.  The first simply looks at case reports from clinical settings.  This slightly old fashioned approach can be though of as a collection of anecdotes for a given variant which, when viewed together, can be used to suggest that a variant does have some true, causal relationship to a disease.  For example, if ten doctors and ten different hospitals all stumble upon ten patients who have an odd cardiac condition and the same super rare genetic variant then we can conclude that super rare genetic variant is likely doing something important.  These reports or anecdotes are collected within a resource called [ClinVar](https://www.ncbi.nlm.nih.gov/clinvar/).  The second approach employs sophisticated algorithms which try to predict how the rare variant affects downstream biology.  Namely, does the rare variant cause a proten to be misfolded and disfunctional.  Many different algorithms exist, each of which report a different severity or pathogenicity score.

With many methods available to determine whether a rare variant is pathogenic, it may seem impossible to come up with a single, clear methedology.  Fortunately, many very bright individuals have developed just such a methedology.  The American College of Medical Genetics (ACMG) provide a [flowchart with detailed accompanying descriptions](https://www.acmg.net/docs/Standards_Guidelines_for_the_Interpretation_of_Sequence_Variants.pdf) that help one classify a variant as being either pathogenic or non-pathogenic.  The only problem 

![ACMG definition pathogenic](/assets/img/acmg_flow.png)

Unfortunately for my purposes, this flow chart requires a great deal of labor (multiple hours) just to classify a single variant as pathogenic.  While resources are being developed that alleviate this load, for example [ClinGen](https://clinicalgenome.org/), they are not yet at the point to help me out.  Therefore, for this project I defined a variant as being pathogenic if it met all three conditions (then I went on to see if these pathogenic variants associate to adverse cardiac conditions).

## Methods

To begin, I defined pathogenicity from the three following conditions.

1. High Confidence Pathogenic According to LOFTEE - Ensembl has released a piece of software entitled [Variant Effect Predictor](https://useast.ensembl.org/info/docs/tools/vep/index.html) (VEP), which does exactly what it sounds like.  Although, VEP does not report a simple binary rating of patogenicity, which is likely why a recent add-on was created called [LOFTEE](https://github.com/konradjk/loftee) which attempts to report whether a variant can be classified with high confidence a loss of function variant.  
2. Listed as Pathogenic or Likely Pathogenic Within ClinVar - 
3. Located on a Gene That Invitae Uses in Their Cardiac Panel - Invitae is a private, well-respected company that creates clinical-grade reports on a patient's genetics.  We figured that they have a good sense of what genes potentially influence adverse cardiac conditions.  The genes we specifically used came from the [Invitae Arrhythmia and Cardiomyopathy Comprehensive Panel](https://www.invitae.com/en/providers/test-catalog/test-02101).









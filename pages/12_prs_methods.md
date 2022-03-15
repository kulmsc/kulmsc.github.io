---
layout: page
title: prs_methods
permalink: /prs_methods/
---


# Performance of Polygenic Risk Score Adjustment Methods

## Introduction

A polygenic risk score is computed by multiplying the number of effect alleles by an externally determined effect size to form a single genetic variant prediction, then repeating the process and summing over all single predictions.  This may sound complicated, but in essence it's just the sum over many linear predictions, in which the prediction is calculated the same way any plain linear prediction is calculated.  However, there are some very important considerations that are specific to this genomic situation.  

First, the specific genetic variants that are included in the polygenic risk score.  In theory, every single genetic variant (over ten million) could be included in the polygenic risk score sum.  However, most of these millions of variants have no relation to the risk condition.  Therefore, we want to select genetic variants that have some reasonably valid effect on the risk condition.  

Second, the weights of the genetic variants that are included.  The most likely true weight can be estimated through external linear regressions, or in other words an external genome wide association study.

However, a neat trick that many people like to pull is to include a genetic variant that has a questionable association to a disease but artifically decrease its effect size such that it has a very small impact on the overall polygenic risk score.  Of course this neat trick is actually informed by very complex Bayesian algorithms.

When it comes to polygenic risk scores There are many other considerations or problems that many have worked on and generated solutions.  These solutions can be though of as methods that take in a table of variants and effect sizes (often times the full summary statistics from a GWAS) and outputs a similar table of variants and effect sizes that can be used to construct the best performing polygenic risk score.  Again, the ultimate objective of the methods is to generate a set of variants and effect sizes that can be used to calculate the maximally accurate polygenic risk score.



## Available Methods

All of the methods that I will be discussing are listed in the following table.

![prs methods](/assets/img/prs_methods.png)



### Clumping

The clumping methods takes a very empirical, straightfoward approach to the problem of selecting variants that go into a polygenic risk score.  After all, one of the more obvious and common approaches people take to selecting variants that relate to a phenotype is the application of a significance threshold, namely that the p-values is less than 5e-8.  However, due to linkage disequilibrium several variants will all clump together, in one location or locus, each of which have very low, significant p-values.  One of these variants is (likely) causing some biological event that leads to the phenotype whereas all of the other variants are just reflecting the allele identities of that likely causal variant.  How do we identify the causal variant.  Well that is a whole other area of study, but one of the fastest and simplest strategies is guessing that it is the variant with the lowest p-value amongst those in the locus.  This is exactly what the clumping algorithm does, it goes to each locus selects the variant with the lowest p-value and pulls those into a table of variants that become the polygenic risk score.

Implementation of the clumping method was applied through the PLINK software.  The “-—clump” flag was selected with a series of “-—clump-p1” p-value and “-—clump-p2” R2 thresholds, each of wich define exactly what an independent locus is.  The variants in the output file were used to subset the primary summary statistics. 

[Official Documentation](https://www.cog-genomics.org/plink/1.9/postproc)


### WC-2D

The clumping algorithm is simple, logical and effective - but there is room for improvement.  For example, not all genetic variants are created equally.  A genetic variant on a gene body is far more likely to cause some biological chage than a genetic variant in an intergenic region.  Therefore, we might want to lower the signficance threshold of those variants in the gene body regions, as we have some other motivation for thinking the genetic variant is worthy to be included in a polygenic risk score.  This idea of having two dimensions of threshold is implemented in the WC-2D method.

Implementation of the WC-2d (winners curse two dimensions) method roughly followed the steps outlined in the clumping method, except for specification of which regions clumping applies to.  Specifically, variants identified as being conserved (listed within \url{http://compbio.mit.edu/human-constraint/data/gff/}) and pleiotropic (listed within supplementary table 2 of the respective publication) were both clumped with a higher p-value threshold.  Only one of these varieties of variants were allowed to have a different threshold at a time.  

[Originating Publication](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1006493)

### WC-lasso

There is a concept called winner's curse, which is best explained in terms of an auction.  The person who has the highest bid wins the item for sale, but is cursed because they spent more money than anyone else wanted to.  Similarly, a variant that is barely included or excluded in the final set of variants that forms a polygenic risk score is cursed.  One way to avoid this curse is to minimize the effects of variants that are on the cusp of polygenic risk score inclusion.  A popular way to determine which variants should be minimized, and exactly what that minimization should be is LASSO - a form of penalized regression. While LASSO is typically implemented in the context of a typical regression, a group of researchers have developed a mathematical workaround such that we can post-hoc complete LASSO.  This workaround is called WC-lasso.

To implement WC-lasso (winners curse lasso), the original summary statistic file was first clumped with a p-value cut-off of 0.01 and R2 cut-off of 0.1.  The vector of remaining effect sizes was then modified according to an equation that was extracted from the original publication. 

The lambda value ranged from 0.001 to 0.1, and the output effect sizes were directly extracted without additional modification.

[Originating Publication](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1006493)


### WC-likelihood

There is more than one way to try and eliminate winner's curse.  Another methedology looks to maximize the overall likelihood of the effects and p-values collected in variant set that is used to calculate the polygenic risk score.  Specifically, as I understand it, this method assumes some underlying distribution of effect sizes then maximizes the selected effect sizes such that they fall under the high tail of this distribution.  For lack of a better term this method is called WC-likelihood.

The WC-likelihood (winners curse likelihood) method was implemented similar to WC-lasso, with the same initial clumping step. The following effect adjustment step however required minimizing a likelihood function.  The full function is available in the originating publication.  Computationally, the minimization was accomplished by the Python function “minimize” and the “nelder-mead” method.  

[Originating Publication](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1006493)


### Double Weight

The Double Weight method, was implemented very similarly to the WC-likelihood and WC-lasso methods, even though it is not formally a member of the WC family.  Specifically, the double weight method attempts to estimate the likelihood each variant belongs to a set of k top variants.  The exact method it goes about calculating this probability of top variant inclusion is slightly complex, but it can be described as an inherently empirical process that looks at the similar variants to the one under analysis and makes an estimation.  The surrounding process also follows the other WC methods.

  After the initial clumping, the effects and standard errors were read into a custom written R script that simulated many samples of effects for each variant.  Then a variant was selected to be in the final adjusted summary statistics if it was in the top range of SNPs with a specified probability.  The value for the range of SNPs varied.

[Originating Publication](https://pubmed.ncbi.nlm.nih.gov/27513194/) - Specifically, check the supplemental.


### Tweedie

What I have called Tweedie's method is highly similar to the WC-likelihood method.  In fact, its motivation is identical.  The only real difference is the form of the likelihood equation employed to estimate the true effect size of the genetic variants.

Implementation of the Tweedie method began by an application of the clumping method, following the steps as described in the original publication.  For the initial clumping step the p-value threshold was set at 0.05 and R2 at 0.25.  The modified summary statistic file was then used within the main tweedie R script that minimized a likelihood function similar to WC-likelihood.  In order to extract the beta values the published script was modified slightly, and is located at https://github.com/kulmsc/PRS-Ithaca/blob/master/tweedy.R.  Three variations of the betas were created, one for each of the FDR, Tweedie, and FDR x Tweedie sub-methods.  

[Originating Publication](https://www.nature.com/articles/srep41262?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+srep%2Frss%2Fcurrent+(Scientific+Reports))
[Official Documentation](https://sites.google.com/site/honcheongso/software/empirical-bayes-risk-prediction)

### LDpred

The LDpred method, in my reading of the literature, kick started the transformation from relatively simple, empirical clumping type methods to more advanced, modeling methods.  The key model that LDpred is attempting to replicate is specifically a whole genome regression model.  In other words, a typical GWAS will report summary statistics in which the effect of each variant is calculated independently.  However, there are many other regression frameworks in which the effects of the variants are calculated all at the same time, which provides critical adjustments that limit the confounding caused by linkage disequilibrium.  What LDpred does is convert the effect sizes from the independent linear regressions to effect sizes from the simultaneous, whole genome regression.  Or at least, it tries to do this by taking advantage of an external linkage disequilibrium matrix.  Within this process LDpred adds the further complexity of trying to estimate whether a variant has a true, causal effect on a phenotype. It completes this second task by considering some sub-distribution of variant effects, from the total distribution, that represent truly causal variants.  The math gets a little complicated, and if you are interested you can check out another [project](https://kulmsc.github.io/ldpred/) I did which fully derives this LDpred method.  But the key idea to understand, because it comes up again and again, is to combine LD patterns with simple, independent GWAS summary statistics to approximate complex, whole genome GWAS summary statistics through a bayesian framework.

To implement the LDpred method the starting summary statistic file was first converted into the STANDARD format required by LDpred (columns of chromosome, position, reference allele, alternative allele, reference allele frequency, info, rsID, p-value, and effect of the alterative allele).  Generating this file simply required reorganizing the starting summary statistic file.  The LD range was calculated as the number of SNPs divided by 4500.  The “ldpred coord” step was first run, followed by a series of “ldpred gibbs” applications with various “f” values (the proportion of true causal variants).  

[Originating Publication](https://pubmed.ncbi.nlm.nih.gov/26430803/)
[Official Documentation](https://github.com/bvilhjal/ldpred)


### LDpred2

LDpred2 is theoretically identical to the original LDpred.  The changes primarily involve computational re-workings of the bayesian algorithm that make it faster and easier to implement.

The implementation of LDpred2 directly followed the vignette that accompanied the original publication.  While the theory was nearly identical to LDpred, all of the coding was done in R.  Additional hyperparameters were fit to investigate a greater number of possible fractions of causal SNPs and heritabilities.  

[Originating Publication](https://academic.oup.com/bioinformatics/article/36/22-23/5424/6039173)
[Official Documentation](https://privefl.github.io/bigsnpr/articles/LDpred2.html)


### lassosum

The lassosum method can be though of as a theoretical combination of both the WC-lasso and LDpred methods.  LDpred because it understands that linkage disequilibrium is a problem that must be handled and that variants that likely do not have any true link to the disease should not be considered in a polygenic risk score calculation.  WC-lasso because it decides that the best way to determine whether a variant does have a true link to the disease is not with multiple theoretical distributions of effect sizes, but rather the LASSO algorithm which shrinks the effects of variants that are deemed to likely not have any effect on the phenotype.  This combination ends up with an algorithm that largely mirrors LDpred, except that the whole genome regression model that we are trying to replicate is now specifically a LASSO model.

Implementation of lassosum essentially required a single function call with UK Biobank used as reference data.  The lassosum.pipeline function generated the adjusted effects with minimal intervention.  A grid of "s" and "lambda" parameters were tried directly within this pipeline.

[Originating Publication](https://pubmed.ncbi.nlm.nih.gov/28480976/)
[Official Documentation](https://github.com/tshmak/lassosum)


### PRScs

The PRScs methods is very, very similar to LDpred.  The only difference is the way variants are judged to be truly causal or not.  Specifically, LDpred uses what is called a point-slab prior in the bayesian process that approximates true causality.  As the name states, PRScs instead uses a series of more flexible continous priors.  This small mathematical change allows can potentially lead to a completely different selection of variants in the final set that are used to calculate a polygenic risk score.

Implementation of the PRScs first required converting the format of the summary statistics.  The primary computation was encapsulated in the single python prscs call.  The reference data employed was the European LD data listed within the PRScs documentation.  The phi parameter was changed over three different function calls, whereas the a and b parameter were held steady.  

[Originating Publication](https://www.nature.com/articles/s41467-019-09718-5)
[Official Documentation](https://github.com/getian107/PRScs)


### sBLUP

Implementation of the sBLUP method began by down sizing the original summary statistic to only the variants included within the HapMap (\url{https://www.broadinstitute.org/medical-and-population-genetics/hapmap-3}), and the columns were re-arranged to the MA format used throughout the GCTA tool kit.  The actual sblup option was then run within gcta, with the wind option (the LD distance parameter) set to 100.  

Official Documentation: \url{https://cnsgenomics.com/software/gcta/#SBLUP}

### SBayesR

Implementation of the SBayesR method started similarly to sBLUP, with conversion of the summary statistics to the necessary MA format.  The primary SBayesR computations were then run within the gctb toolkit.  The ldm files were generated from the UK Biobank and down sized to HapMap variants, and could be downloaded directly from the documentation.  Ldm files with more variants were not used due to size limitations.  

Official Documentation: \url{https://cnsgenomics.com/software/gctb/#SummaryBayesianAlphabet}

### DBSLMM

Additional information was first derived, such as allele frequencies from a PLINK call applied to the UK Biobank data.  Implementation of the DBSLMM algorithm was completed easily within a single R function call.  Once generated p-value and R-squared hyperparameters were iterated over, and the adjusted effect sizes directly computed.

Official Documentation: \url{https://github.com/biostat0903/DBSLMM}

### SMTpred

Implementation of SMTpred is unique in that it required not just the primary summary statistics being adjusted, but also summary statistics of similar diseases.  The exact set of similar summary statistics are determined a priori through genetic correlation calculations.  After proper formatting of the primary and similar summary statistics, they are all entered into a single python function call that adjusts the effect sizes.  The number of similar sets of summary statistics varied.  In addition, SBLUP adjusted effects are also utilized in this process.

Official Documentation: \url{https://github.com/uqrmaie1/smtpred}

### LDAK
Most of the methodology behind the LDAK adjustment is similar to that of PRScs, however the actual application has been custom-written and therefore requires far different inputs and operations.  First, the summary statistics are converted into the proper format and a subset of the UKBB is converted such that the SNP identifiers are by position and allele, matching the style of externally downloaded annotation files.  Next, per-SNP heritabilities are computed using the full BLD-LDAK heritability model, which includes 64 different types of annotations.  The summary statistics are split three ways.  In one split pseudo-summary statistics are computed, in another SNP-SNP correlations are computed.  Next, the actual SNP adjustments are made assuming priors of lasso, lasso-sparse, ridge, bolt, bayes-r, and bayesr-shrink.  For each model multiple parameters are tried, the best of which is determined via scoring with the third split of the summary statistics.  High LD regions are removed if present, and the final adjusted effect sizes are substituted to form the final adjusted summary statistics.

Official Documentation: \url{http://dougspeed.com/megaprs/}

### JAMPred

Implementation of the JAMPred algorithm is analogous to LDpred2 as both chiefly complete their computations within R.  However, JAMPred is far less readily able to handle the large genotypic matrices necessary.  Therefore, LD pruning in PLINK was first carried out, to reduce the size of the genotypic files.  Next, the genotypic data is read into R using the bignsnpr package, and is then carefully converted into the necessary matrix specifications while still being in a memory-efficient format.  The primary JAMPred function is then called over several iterations of lambda values, generating the adjusted effect sizes.

Official Documentation: \url{https://github.com/pjnewcombe/R2BGLiMS}




---
layout: page
title: ldpred
permalink: /ldpred/
---


# LDPred Derivation

## Introduction

In this section I will attempt to derive the LDPred method for shrinking GWAS effect sizes so they may be used in polygenic risk scoring.  In this process I will mention other papers that use Bayes Law to estimate the posterior GWAS effect size.  The [paper](https://www.cell.com/action/showPdf?pii=S0002-9297%2815%2900365-1) has its own derivation, however I found that many steps were missed or even wrong.  Please contact me if you find any errors within this derivation.

## No LD - Infintesimal

### Calculate Estimated Beta

As within nearly all polygenic risk score methods, LDPred begins with the regression equation that has been assumed to be used within the GWAS.

$$ Y = X\beta + \epsilon $$

$$Y$$ is a $$N \times 1$$ vector of phenotypes, $$X$$ is a $$N \times M$$ genotype matrix, $$\beta$$ is a $$M \times 1$$ vector of effects, and $$\epsilon$$ is a $$N \times 1$$ vector of error terms.  An assumption is made that the $$Y$$ vector and each column/variant within $$X$$ has been mean centered and standardized to have variance of 1.

This equation is never explicitly stated (unfortunately), however it can be implied from the application of ordinary least squares (OLS) to obtain the estimated betas - which is the next step.  A typical application of OLS looks like:

$$ \widetilde{\beta} = (X'X)^{-1}X'Y $$

We use the over character tilde to signify estimated values.  The LDPred method was written for summary statistics, which are assumed to be generated using univariate regression, or one variant at a time.  Single variant OLS for variant i looks like:

$$ \widetilde{\beta_i} = (X_i'X_i)^{-1}X_i'Y $$

While I cannot find a great proof, it is easy to verify through simulation that if $$X_i$$ has a mean of 0 and sd of 1:

$$ X_i'X_i = \sum_{j=1}^N X_{i,j}^2 = N $$

Therefore our marginal regression turns into:

$$ \widetilde{\beta_i} = \frac{X_i'Y}{N} $$

Before moving forward we need to make a few more assumptions, or rather give our variables some prior distributions.  Specifically we let $$ \beta \sim N(0,h^2/M). The variance is logical, as the greater the heritability of the trait the larger the possibility of getting large impact variants.  Similarly, the greater number of variants the better our estimates can get (similar to the error of the mean).  Since $$\beta$$ is normally distributed, so is $$\widetilde{\beta}$$.  We can find the parameters of this normal distribution as follows:

$$ \widetilde{\beta_i} = (X'X)^{-1}X'Y $$
$$ E[\widetilde{\beta_i}] = (X'X)^{-1}X'E[Y] $$
$$ E[\widetilde{\beta_i}] = (X'X)^{-1}X'X\beta_i $$
$$ E[\widetilde{\beta_i}] = \beta_i $$

$$ Var(y) = Var(X\beta + \epsilon) $$
$$ Var(y) = Var(\epsilon) = 1 - \frac{h^2}{M}
$$ Var(\widetilde{\beta_i}) = Var((X'X)^{-1}X'Y) $$
$$ Var(\widetilde{\beta_i}) = (X'X)^{-1}X'Var(Y)X(X'X)^{-1} $$
$$ Var(\widetilde{\beta_i}) = (N)^{-2}X_i'Var(Y)X_i $$
$$ Var(\widetilde{\beta_i}) = (N)^{-2}X_i'(1 - \frac{h^2}{M})X_i $$
$$ Var(\widetilde{\beta_i}) = (N)^{-2}X_i'X_i(1 - \frac{h^2}{M}) $$
$$ Var(\widetilde{\beta_i}) = \frac{1-h^2}{M}N $$

The mor confusing process is the variance, in which we first need to calculate out the variance of y.  For some reason in this case we treat beta as a fixed effect leaving us with the variance of epsilon.  Epsilon is the error term, which accounts for the sum of total variance in the phenotype not accounted by the genotypes.  Therefore it is simply one minus the per SNP heritabiliy (per SNP because this is the variance per variant).  

Putting both pieces together our final distribution is:

$$ \widetilde{\beta_i} \sim N(\beta_i,\frac{1-\frac{h^2}{M}}{N}) $$

### Calculate Posterior Beta

Now that we have fully defined the estimated beta, we can use this value to work back and get the posterior distribution of 

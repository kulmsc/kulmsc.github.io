---
layout: page
title: ldpred
permalink: /ldpred/
---


## LDPred Derivation

In this section I will attempt to derive the LDPred method for shrinking GWAS effect sizes so they may be used in polygenic risk scoring.  In this process I will mention other papers that use Bayes Law to estimate the posterior GWAS effect size.  The [paper](https://www.cell.com/action/showPdf?pii=S0002-9297%2815%2900365-1) has its own derivation, however I found that many steps were missed or even wrong.  Please contact me if you find any errors within this derivation.

As within nearly all polygenic risk score methods, LDPred begins with the regression equation that has been assumed to be used within the GWAS.

$$ Y = X\beta + \epsilon $$

$$Y$$ is a $$Nx1$$ vector of phenotypes, $$X$$ is a $$NxM$$ genotype matrix, $$\beta$$ is a $$Mx1$$ vector of effects, and $$\epsilon$$ is a $$Nx1$$ vector of error terms.  An assumption is made that the $$Y$$ vector and each column/variant within $$X$$ has been mean centered and standardized to have variance of 1.

This equation is never explicitly stated (unfortunately), however it can be implied from the application of ordinary least squares (OLS) to obtain the estimated betas - which is the next step.  A typical application of OLS looks like:

$$ \beta = (X'X)^{-1}X'Y $$

The LDPred method was written for summary statistics, which are assumed to be generated using univariate regression, or one variant at a time.  Single variant OLS for variant i looks like:

$$ \beta_i = (X_i'X_i)^{-1}X_i'Y $$

While I cannot find a great proof, it is easy to verify through simulation that if $$X_i$$ has a mean of 0 and sd of 1:

$$ X_i'X_i = \sum_{j=1}^N X_{i,j}^2 = N

Therefore our marginal regression turns into:

$$ \beta_i = \frac{X_i'Y}{N} $$

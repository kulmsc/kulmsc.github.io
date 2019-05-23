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

As within nearly all polygenic risk score methods, LDPred begins with a simple linear model connecting phenotypes and genotypes.  Note that in this equation we are regressing all of the variants simultaneously.  This is not how GWASs are carried out, and therefore summary statistics are generated.  However it is the most accurate yet simple model.

$$ Y = X\beta + \epsilon $$

$$Y$$ is a $$N \times 1$$ vector of phenotypes, $$X$$ is a $$N \times M$$ genotype matrix, $$\beta$$ is a $$M \times 1$$ vector of effects, and $$\epsilon$$ is a $$N \times 1$$ vector of error terms.  An assumption is made that the $$Y$$ vector and each column/variant within $$X$$ has been mean centered and standardized to have variance of 1.

This equation is never explicitly stated (unfortunately), however it can be implied from the application of ordinary least squares (OLS) to obtain the estimated betas - which is the next step.  A typical application of OLS looks like:

$$ \tilde{\beta} = (X'X)^{-1}X'Y $$

We use the over character tilde to signify estimated values.  The LDPred method was written for summary statistics, which are assumed to be generated using univariate regression, or one variant at a time.  Single variant OLS for variant i looks like:

$$ \tilde{\beta_i} = (X_i'X_i)^{-1}X_i'Y $$

While I cannot find a great proof, it is easy to verify through simulation that if $$X_i$$ has a mean of 0 and sd of 1:

$$ X_i'X_i = \sum_{j=1}^N X_{i,j}^2 = N $$

Therefore our marginal regression turns into:

$$ \tilde{\beta_i} = \frac{X_i'Y}{N} $$

Before moving forward we need to make a few more assumptions, or rather give our variables some prior distributions.  Specifically we let $$ \beta \sim N(0,h^2/M) $$. The variance is logical, as the greater the heritability of the trait the larger the possibility of getting large impact variants.  Similarly, the greater number of variants the better our estimates can get (similar to the error of the mean).  Since $$\beta$$ is normally distributed, so is $$\tilde{\beta}$$.  We can find the parameters of this normal distribution as follows:

$$ \tilde{\beta_i} = (X'X)^{-1}X'Y $$

$$ E[\tilde{\beta_i}] = (X'X)^{-1}X'E[Y] $$

$$ E[\tilde{\beta_i}] = (X'X)^{-1}X'X\beta_i $$

$$ E[\tilde{\beta_i}] = \beta_i $$


$$ Var(y) = Var(X\beta + \epsilon) $$

$$ Var(y) = Var(\epsilon) = 1 - \frac{h^2}{M} $$

$$ Var(\tilde{\beta_i}) = Var((X'X)^{-1}X'Y) $$

$$ Var(\tilde{\beta_i}) = (X'X)^{-1}X'Var(Y)X(X'X)^{-1} $$

$$ Var(\tilde{\beta_i}) = (N)^{-2}X_i'Var(Y)X_i $$

$$ Var(\tilde{\beta_i}) = (N)^{-2}X_i'(1 - \frac{h^2}{M})X_i $$

$$ Var(\tilde{\beta_i}) = (N)^{-2}X_i'X_i(1 - \frac{h^2}{M}) $$

$$ Var(\tilde{\beta_i}) = \frac{1-\frac{h^2}{M}}{N} $$

The more confusing process is the variance, in which we first need to calculate out the variance of y.  For some reason in this case we treat beta as a fixed effect leaving us with the variance of epsilon.  Epsilon is the error term, which accounts for the sum of total variance in the phenotype not accounted by the genotypes.  Therefore it is simply one minus the per SNP heritabiliy (per SNP because this is the variance per variant). Note that I got much of this process from [Ruppert](https://www.cambridge.org/core/books/semiparametric-regression/02FC9A9435232CA67532B4D31874412C), specifically on page 30 in Chapter 2.

Putting both pieces together our final distribution is:

$$ \tilde{\beta_i} \sim N(\beta_i,\frac{1-\frac{h^2}{M}}{N}) $$

### Calculate Posterior Beta

Now that we have fully defined the estimated beta, we can use this value to work back and get the posterior distribution of beta itself.  While the paper points to [Dudbridge](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1003348), which points to [Ruppert](https://www.cambridge.org/core/books/semiparametric-regression/02FC9A9435232CA67532B4D31874412C), there is no satisfactory explanation for the seemingly nice result that is achieved.  The work by Dudbrdige would indicate that some BLUP style calculations are needed, but rather this is simply a direct application of Bayes Law:

$$ (\beta | \tilde{\beta}) = \frac{(\tilde{\beta} | \beta)(\beta)}{\tilde{\beta}} $$

$$ (\beta | \tilde{\beta}) = \frac{(\tilde{\beta} | \beta)(\beta)}{\int_{-\infty}^{\infty} (\tilde{\beta} | \beta)(\beta) d\beta}  $$

There is no easy way to do this integral but to do it.  I will now substitute in the normal distributions and solve.  Note that the distribution of the estimated beta is simplified to $$ N(\beta,\frac{1}{N}) $$.  I will show this is correct, or at least produces to right result, although it is stated incorrectly within the paper as $$Var(\beta_i)=1$$.  

$$\int_{-\infty}^{\infty} N(0,\frac{h^2}{M})N(\beta,\frac{1}{N}) d\beta $$

$$\int_{-\infty}^{\infty}    \frac{1}{\sqrt{2\pi \frac{h^2}{M}}}exp[-\frac{(\beta-0)^2}{2\frac{h^2}{M}}]    \frac{1}{\sqrt{2\pi \frac{1}{N}}}exp[-\frac{(\tilde{\beta}-\beta)^2}{2\frac{1}{N}}]   d\beta $$

$$\frac{1}{2\pi} \sqrt{\frac{NM}{h^2}}       \int_{-\infty}^{\infty}    exp[-\frac{(\beta-0)^2}{2\frac{h^2}{M}} -\frac{(\tilde{\beta}-\beta)^2}{2\frac{1}{N}}]   d\beta $$

$$\frac{1}{2\pi} \sqrt{\frac{NM}{h^2}}  \int_{-\infty}^{\infty}  exp[-\frac{1}{2}(\frac{(M\beta)^2}{h^2} + \frac{N (\tilde{\beta}^2- 2\tilde{\beta}\beta + \beta^2)}{1})]   d\beta $$

$$\frac{1}{2\pi} \sqrt{\frac{NM}{h^2}}  \int_{-\infty}^{\infty}  exp[-\frac{N\tilde{\beta}^2}{2}  + N \beta \tilde{\beta} - \frac{1}{2}(N+\frac{M}{h^2})\beta^2 ]   d\beta $$

This integral is clearly very difficult to do, so I will refer to a book of integral or in my case Wolfram alpha, which stats:

$$ \int_{-\infty}^{\infty} exp[-a + bx -cx^2]dx = \sqrt{\frac{\pi}{c}}exp[\frac{b^2}{4c}-a] $$

$$a = \frac{N\tilde{\beta}^2}{2} $$

$$b = N\tilde{\beta}$$

$$c = \frac{1}{2}(N+\frac{M}{h^2}) = \frac{Nh^2 + M}{2h^2} $$

Substituting becomes:

$$ \sqrt{\frac{\pi}{\frac{Nh^2 + M}{2h^2}}} exp[\frac{N^2\tilde{\beta}^2}{4(\frac{1}{2}(N+\frac{M}{h^2}))} - \frac{N\tilde{\beta}^2}] $$

$$ \sqrt{\frac{2h^2\pi}{Nh^2 + M}}exp[-\frac{1}{2}\frac{\tilde{\beta}^2}{\frac{h^2}{M} + \frac{1}{N}}] $$

Now bringing back the coefficients we left out previously:

$$\frac{1}{2\pi} \sqrt{\frac{NM}{h^2}} \sqrt{\frac{2h^2\pi}{Nh^2 + M}}exp[-\frac{1}{2}\frac{\tilde{\beta}^2}{\frac{h^2}{M} + \frac{1}{N}}] $$

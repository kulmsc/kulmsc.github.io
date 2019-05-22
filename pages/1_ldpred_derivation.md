---
layout: page
title: ldpred
permalink: /ldpred/
---


## LDPred Derivation

In this section I will attempt to derive the LDPred method for shrinking GWAS effect sizes so they may be used in polygenic risk scoring.  The paper has its own derivation, however I found that many steps were missed or even wrong.  Please contact me if you find any errors within this derivation.

As within nearly all polygenic risk score methods, LDPred begins with the regression equation that has been assumed to be used within the GWAS.

$$ Y = X\beta + \epsilon $$

This equation is never explicitly stated (unfortunately), however it can be implied from the application of marginal least squares to obtain the estimated betas.  

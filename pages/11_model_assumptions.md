---
layout: page
title: model_assumptions
permalink: /model_assumptions/
---


# Assumptions of Disease Prediction Models

## Introduction

Polygenic risk scores are a completely unknown, and therefore useless, quantity until they are properly evaluated.  These evaluations are completed in the context of a model, for example a comparison of a polygenic risk score against a disease label can be considered an univariate linear regression.  However, there are many factors that may confound a polygenic risk score, which is why most evaluations are multivariate models that contain common risk factors such as age and sex.  Moreover, since disease is often considered an all or nothing affair we can use a logistic regression model, which better handles a binary covariate than a linear regression model.  Now, most investigations of polygenic risk scores stop here and simply use a logistic regression model with a few basic covariates.  However, just as logistic regression is better than linear regression, there are further model modifications we can make to produce a better model.

Although, it is important to note in this process that the descriptions of better, improved or enhanced are all difficult to objectively evaluate.  Which is why we have to both consider the qualitative operation of the model modification and the quantitative effect on the prediction.  While many, many model modifications are possible, we considered modifications that fall into five main catagories of assumptions.  We will analyze these assumptions in turn:


## Results

<div class="ticTacToe-header">
<div style="text-align: left"> <h3> Sections: </h3> </div>
</div>

<div class="ticTacToe-links">
  <a class="link" href="#age" data-scroll>Representation of Age</a>
  <a class="link" href="#censor" data-scroll>Manner of Censorship</a>
</div>


<div class="vertical-space"></div>
<section id="age">
</section>
### Representation of Age



<div class="vertical-space"></div>
<section id="censor">
</section>
### Manner of Censorship



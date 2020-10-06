---
title: "Introduction to Bayesian Inference"
excerpt: "A brief overview and visualization of Bayesian inference<br/><img src='/images/thumb_bayes.png'>"
collection: portfolio
---


Say we are given a coin with an unknown success probability $\hat{p}$. Instead of viewing $\hat{p}$ as a fixed, unknown value, we instead treat it as a random variable, with its own distribution. As we flip the coin, we can update our previous belief of the distribution with the new information from the subsequent coin flips to obtain a new proposed distribution.

In the example below, we show the updating process of the distribution of the success probablity of coin. The parameters on the right represent the actual success probability, and the parameters $\alpha$ and $\beta$ of our initial _prior_ or initial guess on the distribution, which is a $Beta(\alpha,\beta)$.

<div id="observablehq-b6737eed">
  <div class="observablehq-Q"></div>
</div>
<script type="module">
  import {Runtime, Inspector} from "https://cdn.jsdelivr.net/npm/@observablehq/runtime@4/dist/runtime.js";
  import define from "https://api.observablehq.com/@sean-ohagan/coin-flip.js?v=3";
  (new Runtime).module(define, name => {
    if (name === "Q") return Inspector.into("#observablehq-b6737eed .observablehq-Q")();
  });
</script>

[View the Observable Notebook Here](https://observablehq.com/@sean-ohagan/coin-flip)

## Frequentist vs Bayesian Probability

Statistical inference is the process of drawing conclusions about the distribution of data from a sample. Commonly, the goal is to estimate the values of _parameters_ of the distribution— for example, the probability that a coin yields a result of heads.

In the frequentist sense, parameters are thought of to have unknown, fixed values which we are trying estimate. The tools to accomplish this task are primarily point estimates and confidence intervals.

In Bayesian statistics, parameters are instead viewed as random variables, having probability distributions on their values. Instead of estimating the value of the parameter, we estimate instead the probability distribution on them. We can perform similar inference to the frequentist setting by considering the _maximum a posteriori_ estimate or by constructing *credible intervals*, but by having a distribution on the parameters we retain more information about the confidence in our estimates.

## Bayes' Theorem

Bayesian statistics relies on Bayes theorem, a fundamental result regarding conditional probability. For events $A$, $B$, we have
$$P(A|B)=\frac{P(B|A)P(A)}{P(B)}$$

In inference problems, we often let $\theta$ denote the parameter(s) in question, and $y$ denote the data that we observed. We may then apply Bayes' theorem:

$$P(\theta|y) = \frac{P(y|\theta)P(\theta)}{P(y)}$$

- $P(\theta|y)$ is called the *posterior distribution*-- this is what we are trying to calculate, as it will give us a distribution on $\theta$.
- $P(\theta)$ is our *prior distribution*-- this quantifies our previous beliefs on $\theta$. A uniform prior would represent no prior knowledge.
- $P(y|\theta)$ is our *likelihood* function-- this represents how likely we are to obtain the observed data given a value of $\theta$
- $P(y)$ represents a normalization factor

## Inference

The process of inference in a Bayesian setting is the process of calculating a posterior distribution on $\theta$ given the observed data $y$ and a given prior (often uniform in practice, or an educated guess). DeFinetti's theorem guarantees that the same posterior distribution will be reached independent of how many times the updating procedure is done and the order by which it is done ~ADD more here later~

The maximum a posteriori, or MAP estimate, is a point estimate for a parameter obtained by taking the mode of the posterior distribution. This can be viewed as the Bayesian equivalent of the maximum likelihood estimate (MLE), and is actually equivalent in the case when a uniform prior is used.

Credible intervals are the Bayesian counterpart to confidence intervals. The two sided credible interval at the $\alpha$ significance level contains $1-\alpha$ of the probability mass, and is weighted such that $\frac{\alpha}{2}$ is left on each tail.

## Computing the Posterior

Computing the normalizing factor $P(y)$ turns out to be often difficult in practice-- it is typically calculated as
$$P(y)=\int P(y|\theta)P(\theta)\,\mathrm{d}\theta.$$
However, in real application such an integral often becomes nearly impossible to solve analytically, and difficult to approximate using canonical numerical techniques. Before discussing the solution to this issue, we first discuss a special case that avoids it entirely.

#### Conjugate Priors

Note above that computing the normalization factor comes down to integrating the product of the likelihood and the prior. Because of this, knowing that the data which we are modeling follows a certain likelihood function, we may choose our prior in a crafty way to make this integral easy to compute. Additionally, we may choose it such that the resulting posterior distribution is in the same parametric family as the prior.

The classic example of a conjugate prior is using a beta prior with a Bernoulli (or binomial) likelihood.



### Sampling
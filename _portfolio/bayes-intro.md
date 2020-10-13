---
title: "Introduction to Bayesian Inference"
excerpt: "A brief overview and visualization of Bayesian inference<br/><img src='/images/thumb_bayes.png'>"
collection: portfolio
---


Say we are given a coin with an unknown success probability $\hat{p}$. Instead of viewing $\hat{p}$ as a fixed, unknown value, we instead treat it as a random variable, with its own distribution. As we flip the coin, we can update our previous belief of the distribution with the new information from the subsequent coin flips to obtain a new proposed distribution.

In the example below, we show the updating process of the distribution of the success probablity of the coin. The parameters on the right represent the actual success probability, and the parameters $\alpha$ and $\beta$ of our initial _prior_ or initial guess on the distribution, which is a $Beta(\alpha,\beta)$.

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



View the full Observable notebook, including the source, [here](https://observablehq.com/@sean-ohagan/coin-flip)

## Frequentist vs Bayesian Probability

Statistical inference is the process of drawing conclusions about the distribution of data from a sample. Commonly, the goal is to estimate the values of _parameters_ of the distribution— for example, the probability that a coin yields a result of heads.

In the frequentist sense, parameters are thought of to have unknown, fixed values which we are trying to estimate. The tools to accomplish this task are primarily point estimates and confidence intervals.

In Bayesian statistics, parameters are instead viewed as random variables, having probability distributions on their values. Instead of estimating the value of the parameter, the goal is instead to determine its probability distribution. We can perform similar inference to the frequentist setting by considering the _maximum a posteriori_ estimate or by constructing *credible intervals*, but by having a distribution on the parameters we retain more information about the confidence in our estimates.

## Bayes' Theorem

Bayesian statistics relies on Bayes theorem, a fundamental result regarding conditional probability. For events $A$, $B$, we have
$$P(A|B)=\frac{P(B|A)P(A)}{P(B)}$$.

In inference problems, we often let $\theta$ denote the parameter(s) in question, and $y$ denote the data that we observed. We may then apply Bayes' theorem:

$$P(\theta|y) = \frac{P(y|\theta)P(\theta)}{P(y)}$$

- $P(\theta\|y)$ is called the *posterior distribution*-- the resulting probability distribution on the parameter $\theta$ given that we have observed that data $y$
- $P(\theta)$ is our *prior distribution*-- this quantifies our previous beliefs on $\theta$. A uniform prior would represent no prior knowledge.
- $P(y\|\theta)$ is our *likelihood* function-- this represents how likely we are to obtain the observed data given a value of $\theta$
- $P(y)$ represents a normalization factor-- the essence of Bayes' theorem is really that the numerator is the posterior _is proportional to_ the product of the prior and the likelihood. Dividing by $P(y)$ is equivalent to normalizing the posterior.

## Inference

The process of inference in a Bayesian setting is the process of calculating (or approximating) a posterior distribution on $\theta$ given the observed data $y$ and a given prior (often uniform in practice, or an educated guess). Given an exchangable set of data, DeFinetti's theorem guarantees that the same posterior distribution will be reached independently how many times we 'update' the posterior, and the order relative to the data by which we 'update' it. As an example, this means if we flip a coin repeatedly and update the posterior each time, we will obtain the same result as if we updated the posterior once after many flips (provided the set of flips is the same).

The maximum a posteriori, or MAP estimate, is a point estimate for a parameter obtained by taking the mode of the posterior distribution, sometimes written as $\text{arg max} P(\theta\lvert y)$. This can be viewed as the Bayesian equivalent of the maximum likelihood estimate (MLE), and is equivalent in the case when the initial prior used is uniform.

Credible intervals are the Bayesian counterpart to confidence intervals. The two sided credible interval at the $\alpha$ significance level contains $1-\alpha$ of the probability mass of the posterior distribution, and is weighted such that $\frac{\alpha}{2}$ is left in the tail on each side.

## Computing the Posterior

Computing the normalizing factor $P(y)$ turns out to be often difficult in practice-- it is typically calculated as
$$P(y)=\int P(y|\theta)P(\theta)\,\mathrm{d}\theta.$$
However, in real applications, particularly high-dimensional ones, such an integral often becomes nearly impossible to solve analytically, and difficult to approximate using canonical numerical techniques. Before discussing the solution to this issue, we first discuss a special case that avoids it entirely.

### Conjugate Priors

The posterior distribution is proportional to the product of the prior and the likelihood, and computing the normalization factor is often the difficult part. However, for certain pairs of prior distributions and likelihood functions, called _conjugates_, the posterior will conveniently be of the same family of distributions as the prior.

For example, consider a beta prior and a binomial likelihood function. Observe

$$
\begin{align*}
P(\theta\lvert y) &= \frac{\binom{n}{y}\theta^y(1-\theta)^{n-y} \frac{\Gamma(\alpha + \beta)}{\Gamma(\alpha)\Gamma(\beta)}\theta^{\alpha-1}(1-\theta)^{\beta-1}}{\int_0^1\binom{n}{y}\theta^y(1-\theta)^{n-y}\frac{\Gamma(\alpha+\beta)}{\Gamma(\alpha)\Gamma(\beta)}\theta^{\alpha-1}(1-\theta)^{\beta-1}\,d\theta} \\\\
&= \frac{\theta^y+\alpha-1 (1-\theta)^{n+\beta-y-y}}{\int_0^1\theta^y+\alpha-1 (1-\theta)^{n+\beta-y-y} } \\\\
&= \frac{\Gamma(\alpha+\beta)}{\Gamma(\alpha)\Gamma(\beta)}\Big(\theta^{\alpha+y-1}(1-\theta)^{\beta+n-y-1}\Big) \\\\
&= \text{Beta}(\alpha+y,\beta+n-y)
\end{align*}
$$



### Sampling

When we do not have the luxury of using conjugate priors, it is often necessary to use numerical methods to approximate the posterior distribution. A frequently used example of these are known as Markov Chain Monte Carlo (MCMC) methods. 

In essence, the goal is to create a Markov chain with an equilibrium distribution equal to the posterior distribution. Then, after we let the chain get sufficiently close to the equilibrium distribution (burn-in period), sampling states from the chain approximates sampling points from the distribution. We can use these points to approximate the distribution with a histogram.

A natural question at this point remains: how do we construct such a Markov chain?

While there are many methods of doing this, the most classical one is the Metropolis-Hastings algorithm
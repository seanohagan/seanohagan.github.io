---
title: "Polya's Urn Processes"
excerpt: "Description and visualizations of some facts regarding Polya's urn processes<br/><img src='/images/thumb_polya.png'>"
collection: portfolio
---

## The Dirichlet Distribution

First, we describe the __Dirichlet distribution__. This distribution is essentially a multivariate extension of the Beta distribution. Parametrized by a vector $\alpha\in\mathbb{R}^k$, $\alpha_i>0$, $i=1,2,\ldots,k$, $\text{Dirichlet }(\alpha)$ is probability measure supported on the $k-1$ _simplex_ $S_{k-1}$, defined by 

$$S_{k-1}=\Bigg\{(x_1,\ldots,x_k)\in [0,1]^k: \sum_{i=1}^k x_i = 1\Bigg\}\,.$$

For some geometric intuition, we can think of a $1$-simplex as a line segment, a $2$-simplex as an equilateral triangle, and a $3$-simplex as a regular tetrahedron.

Similarly to how the Beta distribution works nicely with a binomial data generating process, the Dirichlet distribution works the same way with a multinomial data generating process. We can visualize the interaction between these distributions below: given a multinomial data generating process with the given probabilities $\alpha$, use the buttons to draw samples and update the distribution of the estimator $\hat{\alpha}$.

<div id="observablehq-c98f84da">
  <div class="observablehq-display"></div>
</div>
<script type="module">
  import {Runtime, Inspector} from "https://cdn.jsdelivr.net/npm/@observablehq/runtime@4/dist/runtime.js";
  import define from "https://api.observablehq.com/@sean-ohagan/multinomial-dirichlet-inference.js?v=3";
  (new Runtime).module(define, name => {
    if (name === "display") return Inspector.into("#observablehq-c98f84da .observablehq-display")();
  });
</script>

## Polya's Urn

__Polya's urn__ is a statistical model that describes repeated draws of balls from an urn. Let's say we start with $k$ different colors of balls. Given some initial conditions $\alpha\in\mathbb{N}^k$, we consider the urn to start with $\alpha_i$ balls of the $i$th color. For every draw from the urn, we replace the chosen ball, but additionally _add another ball of the same color_. In this way, "the rich get richer" is an apt way to describe the process.

With given initials conditions, see how the urn sampling happens below. To reset, simply change one of the initial conditions.

<div id="observablehq-eb912ae0">
  <div class="observablehq-display"></div>
</div>
<script type="module">
  import {Runtime, Inspector} from "https://cdn.jsdelivr.net/npm/@observablehq/runtime@4/dist/runtime.js";
  import define from "https://api.observablehq.com/@sean-ohagan/polyas-urn.js?v=3";
  (new Runtime).module(define, name => {
    if (name === "display") return Inspector.into("#observablehq-eb912ae0 .observablehq-display")();
  });
</script>

## Limits of Polya Urn Processes

For a given set of initial conditions $\alpha$, for each sequence of draws from the urn, the proportion of each color ball in the urn will converge to some _limiting proportion_. Since each sequence of draws may produce different limiting proportions, we consider the limiting proportion as a random variable $X$, with realizations in $S_{k-1}$. Thus, we can randomly sample over sequences of draws from Polya urn processes with given initial conditions to approximation the distribution of $X$.

<div id="observablehq-bd1717a0">
  <div class="observablehq-body"></div>
</div>
<script type="module">
  import {Runtime, Inspector} from "https://cdn.jsdelivr.net/npm/@observablehq/runtime@4/dist/runtime.js";
  import define from "https://api.observablehq.com/@sean-ohagan/polyas-urn-sampling.js?v=3";
  (new Runtime).module(define, name => {
    if (name === "body") return Inspector.into("#observablehq-bd1717a0 .observablehq-body")();
  });
</script>

From the approximation, we see that distribution of $X$ converges that of $\text{Dirichlet }(\alpha)$. More information on this phenomenon can be found in [Blackwell and MacQueen](https://projecteuclid.org/download/pdf_1/euclid.aos/1176342372).
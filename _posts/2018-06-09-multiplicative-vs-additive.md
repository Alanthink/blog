---
excerpt: ""
comments: true
title: "Multiplicative Chernoff vs. additive Chernoff: which one is stronger?"
categories:
  - math
tags:
  - chernoff bound
  - probability
---

Let me first show their definitions from Wikipedia [[1]](#chernoff-bound). Note that the domain of random variables can be extended from $\\{ 0, 1 \\}$ to $[0, 1]$ just noting that $\E \left[ e^{tX_i} \right] \leq \E[X_i] \cdot e^t + (1 - \E[X_i])$.

### Additive Chernoff bound

Suppose $X_1, \dots, X_n$ are *i.i.d.* random variables supported on $[0, 1]$. Let $\E[X_i] = \mu$ and $\bar{X} = \frac{1}{n} \sum_{i = 1}^n X_i$. Then, we have

$$
\Pr\left( \bar{X} > \mu + \eps \right) \leq \left(  \left( \frac{\mu}{\mu + \eps} \right)^{\mu + \eps} \cdot \left( \frac{1 - \mu}{1 - \mu - \eps} \right)^{1 - \mu - \eps} \right)^n,
$$

and 

$$
\Pr\left( \bar{X} < \mu - \eps \right) \leq \left(  \left( \frac{\mu}{\mu - \eps} \right)^{\mu - \eps} \cdot \left( \frac{1 - \mu}{1 - \mu + \eps} \right)^{1 - \mu + \eps} \right)^n.
$$

### Multiplicative Chernoff Bound

Suppose $X_1, \dots, X_n$ are *i.i.d.* random variables supported on $[0, 1]$. Let $\E[X_i] = \mu$ and $\bar{X} = \frac{1}{n} \sum_{i = 1}^n X_i$. Then, we have 

$$
\Pr\left( \bar{X} > (1 + \delta)\mu \right) \leq \left( \frac{e^{\delta}}{ (1 + \delta)^{(1 + \delta)} } \right)^{n \mu},
$$

and 

$$
\Pr\left( \bar{X} < (1 - \delta)\mu \right) \leq \left( \frac{e^{-\delta}}{ (1 - \delta)^{(1 - \delta)} } \right)^{n \mu}.
$$

If you check their proofs in wikipedia, you will find that multiplicative Chernoff uses one more relaxation by $1 + x \leq e^x$. So technically additive chernoff bound is stronger than multiplicative chernoff bound which means I agree with the second answer in [[2]](#stack-exchange).

However, in practice, usuallly we are not referring to this version of additive chernoff bound when we are talking about additive chernoff bound. A more often way to bound $\Pr\left( \bar{X} > \mu + \eps \right)$ is as the following:

$$
\begin{align} \label{eq:sample}
\Pr\left( \bar{X} > \mu + \eps \right) & \leq \frac{ \E[\exp(t (X_i - \mu) )] }{ \exp(t n \eps) } \\
& = \frac{ \prod_{i = 1}^n \E[ \exp(t (X_i - \mu) )] }{ \exp(tn \eps) } \\
& \leq \exp( n(t^2/8 - t\eps ) ) ,
\end{align}
$$

where the last but second inequality is due to Hoeffding's lemma. By letting $t = 4\eps$, we get 

$$
\Pr\left( \bar{X} > \mu + \eps \right) \leq e^{ -2 n \eps^2 }.
$$

This is a weaker additive chernoff bound partly due to Hoeffding's lemma holds for any domain with length at most 1. So it does not make most use of domain $[0, 1]$. And if you are referring to this version of additive chernoff bound, then it is weaker than the multiplicative chernoff bound. This phenomenon can be observed when $\mu \ll 1$.

## References

1. <a name="chernoff-bound"></a> [Chernoff bound](https://en.wikipedia.org/wiki/Chernoff_bound#cite_note-1), Wikipedia.
2. <a name="stack-exchange"></a> [StackExchange](https://math.stackexchange.com/questions/283487/is-the-multiplicative-chernoff-bound-stronger-than-additive-one).
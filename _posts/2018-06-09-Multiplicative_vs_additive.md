---
excerpt: ""
comments: true
title: "Multiplicative Chernoff vs Additive Chernoff: which one is stronger?"
categories:
  - math
tags:
  - chernoff bound
  - probability
---

## Definitions

Let me first show their definitions from Wikipedia [[1]](#chernoff-bound). Note that the domain of random variables can be extended from $\\{ 0, 1 \\}$ to $[0, 1]$ just noting that $\mathrm{E}\left[ e^{tX_i} \right] \leq E[X_i] \cdot e^t + (1 - \mathrm{E}[X_i])$.

### Additive Chernoff Bound

Suppose $X_1, \dots, X_n$ are *i.i.d.* random variables supported on $[0, 1]$. Let $\mathrm{E}[X_i] = \mu$ and $\bar{X} = \frac{1}{n} \sum_{i = 1}^n X_i$. Then, we have 

$$
\begin{equation*}
\Pr\left[ \bar{X} > \mu + \epsilon \right] \leq \left(  \left( \frac{\mu}{\mu + \epsilon} \right)^{\mu + \epsilon} \cdot \left( \frac{1 - \mu}{1 - \mu - \epsilon} \right)^{1 - \mu - \epsilon} \right)^n,
\end{equation*}
$$

and 

$$
\begin{equation*}
\Pr\left[ \bar{X} < \mu - \epsilon \right] \leq \left(  \left( \frac{\mu}{\mu - \epsilon} \right)^{\mu - \epsilon} \cdot \left( \frac{1 - \mu}{1 - \mu + \epsilon} \right)^{1 - \mu + \epsilon} \right)^n.
\end{equation*}
$$

### Multiplicative Chernoff Bound

Suppose $X_1, \dots, X_n$ are *i.i.d.* random variables supported on $[0, 1]$. Let $\mathrm{E}[X_i] = \mu$ and $\bar{X} = \frac{1}{n} \sum_{i = 1}^n X_i$. Then, we have 

$$
\begin{equation*}
\Pr\left[ \bar{X} > (1 + \delta)\mu \right] \leq \left( \frac{e^{\delta}}{ (1 + \delta)^{(1 + \delta)} } \right)^{n \mu},
\end{equation*}
$$

and 

$$
\begin{equation*}
\Pr\left[ \bar{X} < (1 - \delta)\mu \right] \leq \left( \frac{e^{-\delta}}{ (1 - \delta)^{(1 - \delta)} } \right)^{n \mu}.
\end{equation*}
$$

## Answer

If you check their proofs, you will find that Multiplicative Chernoff uses one more relaxation by $1 + x \leq e^x$ in its proof. So technically additive chernoff bound is stronger than multiplicative chernoff bound which means I agree with the second answer in [[2]](#stack-exchange).

## However...

In practice, usuallly we are not referring to this version of additive chernoff bound when we are talking about additive chernoff bound. A more often way to bound $\Pr\left[ \bar{X} > \mu + \epsilon \right]$ is as the following:

$$
\begin{align*}
\Pr\left[ \bar{X} > \mu + \epsilon \right] & \leq \frac{ \mathrm{E}[\exp(t (X_i - \mu) )] }{ \exp(t n \epsilon) } \\
& = \frac{ \prod_{i = 1}^n \mathrm{E}[ \exp(t (X_i - \mu) )] }{ \exp(tn \epsilon) } \\
& \leq \exp( n(t^2/8 - t\epsilon ) ) 
\end{align*},
$$

where the last but second inequality is due to Hoeffding's lemma. By letting $t = 4\epsilon$, we get 

$$
\begin{equation*}
\Pr\left[ \bar{X} > \mu + \epsilon \right] \leq e^{ -2 n \epsilon^2 }.
\end{equation*}
$$

This is a weaker additive chernoff bound partly due to Hoeffding's lemma holds for any domain with length at most 1. So it does not make most use of domain $[0, 1]$. And if you are referring to this version of additive chernoff bound, then it is weaker than the multiplicative chernoff bound. This phenomenon can be observed when $\mu \ll 1$.

## References

1. <a name="chernoff-bound"></a> [Chernoff bound](https://en.wikipedia.org/wiki/Chernoff_bound#cite_note-1), Wikipedia.
2. <a name="stack-exchange"></a> [StackExchange](https://math.stackexchange.com/questions/283487/is-the-multiplicative-chernoff-bound-stronger-than-additive-one).
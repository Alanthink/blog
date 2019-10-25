---
excerpt: ""
comments: true
title: An application of Doob's martingale inequality
categories:
  - math
tags:
  - sub-gaussian 
  - doob's inequality
  - chernoff's method
---

## Problem

Suppose we have a sequence of *i.i.d.* Gaussian random variables $X_t$'s with bounded variance. Let $S_n = \sum_{t = 1}^n X_t$. How can we bound the probability of 

$$
\begin{equation} \label{eq:problem}
\left\{\max_{1 \leq t \leq n} S_t > \eps \right\}? 
\end{equation}
$$

## Sub-gaussian random variables

To make this problem more realistic, it is always safe to loose Gaussian random variables to zero mean *sub-gaussian* random variables. 

Intuitively, sub-gaussian r.v.'s have tails decreasing as fast as those of Gaussian r.v.'s. Hence, most of the inequalities related to Gaussian r.v.'s can be safely applied to sub-gaussians without any modifications! You are suggested to refer to [Subgaussian random variables: An expository note](http://www.stat.cmu.edu/~arinaldo/36788/subgaussians.pdf) for more details. Here, we assume $X_t$ is a $\sigma$-subgaussian r.v. which is comparable to a Gaussian r.v. with variance $\sigma^2$.

An elementary property of $\sigma$-subgaussian random variable $X$ is 

$$
\begin{equation} \label{eq:ele-prop-subgaussian}
\E[ \exp( \lambda X ) ] \leq \exp( \lambda^2 \sigma^2  / 2 ).
\end{equation}
$$

Also, it is useful to know the following facts:

+ $S_t$ is $\sqrt{t} \sigma$-subgaussian,
+ and $\Pr( X > \eps ) \leq \exp\left( - \frac{ \eps^2 }{2\sigma^2} \right)$.

## A naive way

Obviously, we can use a union bound to give a naive bound. 

For each $t$, since $S_t$ is $\sqrt{t} \sigma$-subgaussian we have $\Pr( S_t > \eps ) \leq \exp\left( - \frac{\eps^2}{2 t \sigma^2} \right)$. Via a union bound, we get

$$
\begin{equation*}
\Pr\left( \max_{1 \leq t \leq n} S_t > \eps \right) \leq \sum_{t = 1}^n \exp\left( - \frac{\eps^2}{2 t \sigma^2 } \right),
\end{equation*}
$$

from which we can see the upper bound is no less then 

$$
\begin{equation} \label{eq:union-bound}
n \exp\left( - \frac{\eps^2}{2 n \sigma^2 } \right).
\end{equation}
$$

This bound is very loose. Later, you will see why it is useless. 

## An alternative way

### Doob's martingale inequality

The formal statement of Doob's martingale inequality can be found in [[1]](#doob-inequality). We restate it in the following.

Suppose the sequence $T_1, \dots, T_n$ is a submartingale, taking non-negative values. Then it holds that

$$
\begin{equation} \label{eq:doob-inequality}
\Pr\left( \max_{1\leq t \leq n} T_t > \eps \right) \leq \frac{ \E[T_n] }{\eps}. 
\end{equation}
$$

With this tool in mind, we are now ready to bound $\eqref{eq:problem}$ in another way.

Using standard Chernoff's method, for any $\lambda > 0$, we have

$$
\begin{align*}
\Pr\left( \max_{1 \leq t \leq n} S_t > \eps \right) & = \Pr\left( \max_{1 \leq t \leq n} \exp(\lambda S_t) > \exp( \lambda \eps) \right ).
\end{align*}
$$

Since $\E[ \exp( \lambda X_t ) ] \geq \exp( \E[ \lambda X_t )] = 1$, sequence $ \exp(\lambda S_1), \dots, \exp(\lambda S_t)$ is a submartingale. (This is a good exercise. You can validate it by yourself.) By \eqref{eq:doob-inequality}, we further have  

$$
\begin{align*}
\Pr\left( \max_{1 \leq t \leq n} S_t > \eps \right) & \leq \frac{ \E[\exp(\lambda S_n )] }{ \exp(\lambda \eps) } \\
& = \frac{ \prod_{t = 1}^n \E[ \exp(\lambda X_t )] }{ \exp(\lambda \eps) } \\
& \leq \exp\left( \frac{\lambda^2 \sigma^2 n}{2} - \lambda \eps \right),
\end{align*} 
$$

where the second equality is due to the mutual indenpendency of $X_t$'s, and the last inequality is due to \eqref{eq:ele-prop-subgaussian}.

The minimum is achieved when $\lambda = \frac{\eps}{ \sigma^2 n}$. So we finally get

$$
\Pr\left( \max_{1 \leq t \leq n} S_t > \eps \right) \leq \exp\left( - \frac{\eps^2}{2 n \sigma^2 } \right),
$$

which is only one $n$-th of \eqref{eq:union-bound}!

Note that Lemma 2 in [[2]](#lemma-2) gives the same statement. However, I think it is more direct to use Doob's martingale inequality.

## References

1. <a name="doob-inequality"></a> [Doob's martingale inequality](https://en.wikipedia.org/wiki/Doob%27s_martingale_inequality), Wikipedia.

2. <a name="lemma-2"></a> Shengjia Zhao, Enze Zhou, Ashish Sabharwal, and Stefano Ermon. [Adaptive Concentration Inequalities for Sequential Decision Problems
](https://papers.nips.cc/paper/6493-adaptive-concentration-inequalities-for-sequential-decision-problems). In Advances in Neural Information Processing Systems, pages 1343–1351, 2016.

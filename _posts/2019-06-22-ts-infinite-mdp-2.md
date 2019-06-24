---
excerpt: ""
comments: true
title:  Bayesian regret of Thompson Sampling in infinite undiscounted MDP (part 2)
categories:
  - math
tags:
  - infinite
  - MDP
  - Thompson Sampling
  - undiscounted
---

If you are familiar with bandit literature, it is typical that we want to find a policy such that the regret is bounded by $\widetilde{\mathcal{O}}( \sqrt{T} )$ from which we can see as time goes to infinity, the regret added per step will approach $0$.

To make it possible, we must add some constraint to the underlying MDP. Otherwise, no policy could achieve such bound. For example, we construct a MDP as the following:
* $\mathcal{S} = \\{ S_1, S_2 \\}$
* $\mathcal{A} = \\{ a_1, a_2 \\}$
* $c(s_1, a_1) = 5$, $c(s_1, a_2) = 10$, $c(s_2, a_1) = 10$, $c(s_2, a_2) = 10$
* $\theta^{\star}(s_1 \cond s_1, a_1) = 1$, $\theta^{\star}(s_2 \cond s_1, a_2) = 1$, $\theta^{\star}(s_2 \cond s_2, a_1) = 1$, $\theta^{\star}(s_2 \cond s_2, a_2) = 1$
* $s_0 = S_1$

![alt text](../../images/mdp.png "Bad MDP example")

It is clear that the optimal policy is $\pi = a_1$. However, no policy could achieve this. We prove this by contradiction. Suppose such a policy exists. We call it $p_0$. Then during the first $T/2$ steps, it must not try action $a_2$. Otherwise, the regret would be at least $2.5T = \Omega(T)$. A key point is when we change the cost function a little bit such that $c(s_1, a_2) = 0$, $c(s_2, a_1) = 0$ and $c(s_2, a_2) = 0$, $p_0$ will not change its behavior in the first $T/2$ steps since it does not even try action $a_2$. Note that during the first $T/2$ steps, it already incurs a $\Omega(T)$ regret. A contradiction happens.

## Weakly communicating MDP

We say a MDP is *weakly communicating* if $\mathcal{S}$ can be divided into two groups $\mathcal{S}_1$ and $\mathcal{S}_2$ such that every state in $\mathcal{S}_1$ is transient under any stationary policy and every pair of two states in $\mathcal{S}_2$ can reach from each other under some stationary policy. It can be shown that
<div class="theorem">
The optimal policy of a weakly communicating MDP satisfies 

$$
J_{\star}(\theta^{\star}) + v(s) = \min_{a \in \mathcal{A}} \left\{ c(s, a) + \sum_{s' \in \mathcal{S}} \theta^{\star}(s' \cond s, a) v(s') \right\}
$$
for any $s \in \mathcal{S}$.
</div>

Here $v$ is called the *bias vector*. Note that if $v$ is a bias vector, so does $v + c$. For convenience, we can assume $\min_{s \in \mathcal{S}} v(s) = 0$. Usually, we assume an upper bound of $\max_{s \in \mathcal{S}} v(s) \leq H$. $H$ is closely related to the diameter of the MDP which will not be expanded in this post. Intuitively, it means that the time needed from one state to another state in group $\mathcal{S}_2$ is limited.

For the subsequent context, unless otherwise specified, the underlying MDP is assumed weakly communicating, $\min_{s \in \mathcal{S}} v(s) = 0$ and $\max_{s \in \mathcal{S}} v(s) \leq H$.


## Policy and guarantee

In the following step, I will present an algorithm originally introduced in [[1]](#OGNJ17) achieving such regret bound.

Recall that the basic framework of Thompson Sampling is described in Algorithm 1 of [Bayesian regret of Thompson Sampling in infinite undiscounted MDP (part 1)]({{ site.baseurl }}{% post_url 2019-05-01-ts-infintie-mdp %}). To describe the stopping criteria of aforementioned algorithm, we need to introduce some notations. Let $T_k = t_{k + 1} - t_k$ be the length of the $k$-th episode and $N_t(s, a)$ be the number of visits of state-action pairs *before* time $t$. Basically, the algorithm states that episode $k$ finishes if one of the following happens:
* $T_k > T_{k - 1}$; 
* $\exists (s, a) \in \mathcal{S} \times \mathcal{A}, ~\text{s.t.}~ N_{t}(s, a) > 2 N_{t_k}(s, a)$.

It can be shown that
<div class="theorem">
The Bayesian regret of aforementioned algorithm is bounded by $\widetilde{\mathcal{O}}( HS \sqrt{AT} )$, where $S = | \mathcal{S}|$, $A = | \mathcal{A}|$.
</div>


## Reference

1. <a name="OGNJ17"></a> Yi Ouyang, Mukul Gagrani, Ashutosh Nayyar, and Rahul Jain. Learning unknown markov
decision processes: A thompson sampling approach. In NIPS, pages 1333–1342, 2017.


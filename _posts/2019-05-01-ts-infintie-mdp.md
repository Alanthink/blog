---
excerpt: ""
comments: true
title:  Bayesian regret of Thompson Sampling in infinite undiscounted MDP (part 1)
categories:
  - math
tags:
  - MDP
  - Thompson Sampling
  - infinite
  - undiscounted
---

In this post, I will present how to bound the regret in an infinite undiscounted Markov Decision Process (MDP) using some appropriate policy. Note that recently MDP has become the most famous model-based model to handle reinforcement learning problems.

To warm up, I will first go over the basic settings which I am currently talking about. Then I will introduce the general schema of Thompson Sampling algorithm. Finally, I will describe the policy which I am going to use and present the theoretical guarantee of this policy. The last part will be deferred to the next post.

## Basic settings

### Infinite undiscounted MDP

An infinite undiscounted MDP can be described using a 5-tuple $(\mathcal{S}, \mathcal{A}, c, \theta^{\star}, s_0)$ where their meanings are explained in the following:
* $\mathcal{S}$: set of states;
* $\mathcal{A}$: set of possible actions can be taken in *every* state;
* $c$: cost function and $c(s, a) \in [0, 1]$ denotes the cost when the agent takes action $a$ on state $s$;
* $\theta^{\star}$: transition possibility and $\theta^{\star}(s' \cond s, a)$ denotes the possibility when the agent takes action $a$ on state $s$ and the next state is $s'$ (Note that for any pair $(s, a) \in \mathcal{S} \times \mathcal{A}$ it holds that $\sum_{s' \in \mathcal{S}} \theta^{\star}(s' \cond s, a) = 1$);
* $s_0$: start state of the MDP.

For simplicity, it is assumed that $\mathcal{S}$, $\mathcal{A}$, $c$ and $s_0$ are known to the agent before the start of the process. In other words, only the transition possibility is unknown. The goal is that we want to design a policy such that given time horizon $T$ (after $T$ actions), the cost incurred by the agent using this *policy* is as small as possible.

Next, I will formalize some aforementioned terms.

### Policy

At time step $t$, we use $H_t = (s_0, a_1, s_1, \dots, a_t, s_t)$ to denote the history available to the agent. The policy at time $t$ denoted by $\pi_t$ is formally defined as $\pi_t: H_t \rightarrow \rho(\mathcal{A})$ where $\rho(\mathcal{A})$ is the set of distributions with support on $\mathcal{A}$. The reason to use $\rho(\mathcal{A})$ is that a policy might be randomized. Hence a policy can be represented by $\pi = (\pi_1, \dots, \pi_T)$. Note that this kind of definition is very general.

When the underlying model has transition probability $\theta^{\star}$, we use 

$$
J_{\pi}( \theta^{\star} ) \eqdef \lim_{T \rightarrow \infty} \frac{1}{T} \cdot \E \left[ \sum_{t = 1}^T c(s_{t - 1}, a_t^{\pi}) \right]
$$

to denote the cost incurred by the agent using policy $\pi$ per step/action, where $a_t^{\pi}$ represents the action taken by the policy at time $t$. Here, the expectation is taken with respect to the randomness of the policy and the transition probability. For simplicity, I will drop the dependency of policy in the representation when it is clear from the context.

We say a policy $\pi$ is *optimal* if it satisfies 

$$
J_{\pi}(\theta^{\star}) = \min_{\pi'} J_{\pi'}(\theta^{\star}).
$$

Note that $c(\cdot, \cdot) \in [0, 1]$. Hence $J_{\pi}(\theta^{\star}) \in [0, 1]$.

We use $J_{\star}(\theta^{\star})$ to denote the minimum cost per action/step.

#### Stationary Markov policy

It is very difficult to search for the optimal policy under the scope of the general definition of policy used before. Fortunately, we only need to consider the policy when it is *stationary*, *Markov*. We say a policy $\pi = (\pi_1, \dots, \pi_T)$ is stationary if $\pi_t = \pi_{t'}$ for any $t \neq t'$. It is Markov if the next action is only decided by the current state. 

Hence a stationary Markov policy $\pi$ can be formally defined by $\pi: \mathcal{S} \rightarrow \rho(\mathcal{A})$.

It can be shown that
<div class="theorem">
There exists a stationary Markov policy $\pi$ such that
\[
J_{\pi}(\theta^{\star}) = J_{\star}(\theta^{\star}).
\]
</div>

Therefore, in the subsequent material, it suffices to look at policies which are stationary and Markov.

### Regret

Intuitively, we want to minimize $\E\left[ \sum_{t = 1}^T c(s_{t - 1}, a_t) \right]$ which is equivalent to minimize the *regret* defined as 

$$
\begin{equation} \label{def:regret}
\mathcal{R}_T(\pi) \eqdef \E\left[ \sum_{t = 1}^T c(s_{t - 1}, a_t) \right] - T \cdot J_{\star}(\theta^{\star}).
\end{equation}
$$

Here is the intuition to use the notation of regret. It is possible that different model may have different minimum cost. If we use the expected cost as a measure, it is hard to compare different policy. The usage of regret can remove the effect of the difference of different minimum cost and lead to fair comparison.

## Thompson Sampling

Thompson Sampling is a kind of Bayesian schema to handle reinforcement learning problems. It gains increasing popularity recently. Basically, it divides the whole procedure into several episodes. At the begining of each episode, the agent just samples a virtual environment from a pre-defined distribution over the unknown parameters and then takes optimal action assuming the underlying model is the sampled one. To be more specific, before the algorithm starts, it assumes a prior distribution over the unknown parameters of the underlying model. In the case of MDP discussed here, the unknown parameter is just the transition probability i.e., $\theta^{\star}$. After taking some action and getting the information of the next state, the agent uses this obervation to update the prior distribution and derive a posterior distribution via applying Bayes’ Theorem. In the next episode, the agent uses the posterior distribution derived at the end of the last episode as a new prior distribution. The following pseudocode shows the aforementioned learning procedure.

<p id="algorithm">
<pre id="ts" style="display:none">
    \begin{algorithm}
    \caption{Thompson Sampling}
    \begin{algorithmic}
    \STATE initialization: prior distribution $p_0$, start state $s_0$, start of episode $k \leftarrow 1$ and start time $t \leftarrow 1$
    \WHILE{$t \leq T$}
    \STATE $t_k \leftarrow t$ \COMMENT{record start time of $k$-th episode}
    \STATE compute $p_k \leftarrow p_{k - 1} \ |\ H_{t_k}$
    \STATE sample $\theta_k$ from distribution $p_k$
    \STATE solve optimal policy $\pi_k$ assuming the underlying model has transition probability $\theta_k$
    \WHILE{$t \leq T$ and stopping criteria of episode $k$ isn't satisfied}
    \STATE take action $a_t$ according to $\pi_k$ and observe next state $s_t$
    \STATE $t \leftarrow t + 1$
    \ENDWHILE
    \ENDWHILE
    \end{algorithmic}
    \end{algorithm}
</pre>
</p>

Thompson Sampling is naturally appealing and all it needs is an appropriate prior distribution over the unknown parameters.

With this background, we can define *Bayesian regret* which can be written as

$$
\begin{align}
\mathrm{BR}_T(\pi) & \eqdef \E_{\theta^{\star} \sim p_0}\left[ \E\left[ \sum_{t = 1}^T c(s_{t - 1}, a_t)  - T \cdot J_{\star}(\theta^{\star}) \cond \theta^{\star} \right] \right] \notag \\
& = \E_{\theta^{\star} \sim p_0}[ \mathcal{R}_T(\pi) \cond \theta^{\star}] \label{def:bayes-regret},
\end{align}
$$

where $p_0$ is the initial prior distribution of the Thompson Sampling policy. Note that \eqref{def:bayes-regret} is a weaker definiton than \eqref{def:regret} since $\mathcal{R}_T(\pi) \leq \mathcal{O}(f(\cdot))$ implies $\br_T(\pi) \leq \mathcal{O}(f(\cdot))$ and the other direction does not always hold, and it is a dedicated definition in the Bayesian learning literature.

## Reference

1. <a name="OGNJ17"></a> Yi Ouyang, Mukul Gagrani, Ashutosh Nayyar, and Rahul Jain. Learning unknown markov
decision processes: A thompson sampling approach. In NIPS, pages 1333–1342, 2017.

<script type="text/javascript">
    var testExamples = document.getElementById("ts").textContent;
    pseudocode.render(testExamples, document.getElementById("algorithm"), {
        lineNumber: true,
        noEnd: true
    });
</script>



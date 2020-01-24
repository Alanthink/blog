---
excerpt: ""
comments: true
title:  Bayesian regret of Thompson Sampling in infinite undiscounted MDP (part 1)
categories:
  - math
tags:
  - infinite MDP
  - Thompson Sampling
  - undiscounted
---

In this and (possibly) the following several posts, I will present how to bound the regret in an infinite undiscounted Markov Decision Process (MDP) using some appropriate policy. Note that MDP is the most famous model to handle reinforcement learning problems.

To warm up, I will first go over the basic settings which I am currently talking about. Then I will introduce the general schema of Thompson Sampling policy. Finally, I will describe the policy which I am going to use and present the theoretical guarantee of this policy.

## Basic settings

### Infinite undiscounted MDP

An infinite undiscounted MDP can be described using a 5-tuple $(\mathcal{S}, \mathcal{A}, R, \theta, s_1)$ where their meanings are explained in the following:
* $\mathcal{S}$: set of states;
* $\mathcal{A}$: set of possible actions can be taken in *every* state;
* $R$: reward function and $R(s, a) \in [0, 1]$ denotes the reward received when the agent takes action $a$ on state $s$;
* $\theta$: transition possibility and $\theta(s' \cond s, a)$ denotes the possibility when the agent takes action $a$ on state $s$ and the next state is $s'$ (Note that for any pair $(s, a) \in \mathcal{S} \times \mathcal{A}$ it holds that $\sum_{s' \in \mathcal{S}} \theta(s' \cond s, a) = 1$);
* $s_1$: start state of the MDP.

For simplicity, it is assumed that $\mathcal{S}$, $\mathcal{A}$, $R$ and $s_1$ are known to the agent before the start of the process. In other words, only the transition possibility is unknown. 

The goal is that we want to design a *policy* such that given time horizon $T$ (after $T$ actions), the reward received by the agent using this policy is as large as possible.

In infinite undiscounted MDP, given a policy $\pi$, the average reward it obtains is defined as the following:

$$
\bar{R}^{\pi} \eqdef \liminf_{T \rightarrow \infty} \frac{1}{T} \cdot \E \left[ \sum_{t = 1}^T R(s_t, a_t) \right],
$$

where $s_t$ and $a_t$ denote the state and action at time $t$ when policy $\pi$ is token respectively. A policy is said to be *optimal* if it achieves the maximum average reward. Note that $R(\cdot, \cdot) \in [0, 1]$. Hence $\bar{R}^{\pi} \in [0, 1]$. And we use $\pi^{\*}$ and $\bar{R}^{*}$ to denote the optimal policy and the optimal average reward respectively.

Next, I will formalize some aforementioned terms.

### Policy

At time step $t$, we use $H_t = (s_1, a_1, \dots, s_{t-1}, a_{t-1}, s_t)$ to denote the history available to the agent. The policy at time $t$ denoted by $\pi_t$ is formally defined as $\pi_t: H_t \rightarrow \rho(\mathcal{A})$ where $\rho(\mathcal{A})$ is the set of distributions with support on $\mathcal{A}$. The reason to use $\rho(\mathcal{A})$ is that a policy might be randomized. Hence a policy can be represented by $\pi = (\pi_1, \dots, \pi_T)$. Note that this kind of definition is very general.

#### Stationary Deterministic Markov policy

It is very difficult to search for the optimal policy under the scope of the general definition used before. Fortunately, we only need to consider the policy when it is *stationary*, *deterministic*, and *Markov*. We say a policy $\pi = (\pi_1, \dots, \pi_T)$ is stationary if $\pi_t = \pi_{t'}$ for any $t \neq t'$. It is deterministic if $\pi(s) \in \mathcal{A}$ for any $s \in \mathcal{S}$. It is Markov if the next action is only decided by the current state. 

Hence a stationary deterministic Markov policy $\pi$ can be formally defined by $\pi: \mathcal{S} \rightarrow \mathcal{A}$.

It can be shown that
<div class="theorem">
There exists a stationary deterministic Markov policy $\pi'$ such that
\[
\bar{R}^{\pi'} = \bar{R}^{*}.
\]
</div>

Therefore, in the subsequent material, it suffices to look at policies which are stationary, deterministic and Markov.

### Regret

Intuitively, we want to maximize $\bar{R}^{\pi}$ which is equivalent to minimize the *regret* defined as 

$$
\begin{equation} \label{def:regret}
\mathcal{R}_T^{\pi} \eqdef \E\left[ T \cdot \bar{R}^{*} - \sum_{t = 1}^T R(s_t, a_t) \right] .
\end{equation}
$$

Here is the intuition to use the notation of regret. It is possible that different model may have different maximum reward. If we use the expected rewards as a measure, it is hard to compare different policy. The usage of regret can remove the effect of different maximum reward and lead to fair comparison. Secondly, from the regret, we can directly read how far the current policy is away from the optimal policy.

## Thompson Sampling

Thompson Sampling is a kind of Bayesian schema to handle reinforcement learning problems. It gains increasing popularity recently. Basically, it divides the whole procedure into several episodes. At the begining of each episode, the agent just samples a virtual environment from a pre-defined distribution over the unknown parameters and then takes optimal action assuming the underlying model is the sampled one. To be more specific, before the algorithm starts, it assumes a prior distribution over the unknown parameters of the underlying model. In the case of MDP discussed here, the unknown parameter is just the transition probability i.e., $\theta$. After taking some action and getting the information of the next state, the agent uses this obervation to update the prior distribution and derive a posterior distribution via applying Bayes’ Theorem. In the next episode, the agent uses the posterior distribution derived at the end of the last episode as a new prior distribution. The following pseudocode shows the aforementioned learning procedure.

<p id="algorithm">
<pre id="ts" style="display:none">
    \begin{algorithm}
    \caption{Thompson Sampling}
    \begin{algorithmic}
    \STATE initialization: prior distribution $p_1$, start state $s_1$, start of episode $k \leftarrow 1$ and start time $t \leftarrow 1$
    \WHILE{$t \leq T$}
    \STATE $t_k \leftarrow t$ \COMMENT{\textit{record start time of $k$-th episode}} 
    \STATE sample $\theta_k$ from distribution $p_k$
    \STATE solve optimal policy $\pi_k$ assuming the underlying model has transition probability $\theta_k$
    \WHILE{$t \leq T$ and stopping criteria of episode $k$ isn't satisfied}
    \STATE take action $a_t$ according to $\pi_k$ and observe next state $s_{t+1}$
    \STATE $t \leftarrow t + 1$
    \ENDWHILE
    \STATE \COMMENT{\textit{end of $k$-th episode}}
    \STATE compute $p_{k+1} \leftarrow p_k\ |\ H_{t}$
    \STATE $k \leftarrow k + 1$
    \ENDWHILE
    \end{algorithmic}
    \end{algorithm}
</pre>
</p>

Thompson Sampling is naturally appealing and all it needs is an appropriate prior distribution over the unknown parameters. Unlike algorithms using principle *optimism in the face of uncertainty*, it does not need to design dedicated confidence bound when the underlying problem setting is changed.

With this background, we can define *Bayesian regret* which can be written as

$$
\begin{align}
\br_T^{\pi} & \eqdef \E_{\theta \sim p_1} \left[ \E\left[ T \cdot \bar{R}^{*} - \sum_{t = 1}^T R(s_t, a_t) \cond \theta \right] \right] \notag \\
& = \E_{\theta \sim p_1} [ \mathcal{R}_T^{\pi} \cond \theta] \label{def:bayes-regret},
\end{align}
$$

where $p_1$ is the initial prior distribution of the Thompson Sampling policy. Within the context of Thompson Sampling, it is typical to use Bayesian regret instead of normal regret. Note that $\eqref{def:bayes-regret}$ is a weaker definiton than $\eqref{def:regret}$ since $\mathcal{R}_T^{\pi} \leq \mathcal{O}(f(\cdot))$ implies $\br_T^{\pi} \leq \mathcal{O}(f(\cdot))$ and the other direction does not hold.

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



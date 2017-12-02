---
layout: post
title: "A Useful Tool in Probability Extension"
date: 2016-10-03 19:05:33 +0000
categories: tool probability extension
---

## Question

Suppose that \\( \mathscr{F}_0 \\) is a field and \\( P_1 \\) and \\( P_2 \\) are probability measures on \\( \sigma(\mathscr{F}_0) \\). Show by the monotone class theorem that if \\( P_1 \\) and \\( P_2 \\) agree on \\( \mathscr{F}_0 \\), then they agree on \\( \sigma(\mathscr{F}_0) \\). [Q3.13 from Probability and Measure]

## Proof

Let \\( \mathscr{H} \\) be the set of all events \\( B \in \sigma(\mathscr{F}_0) \\) such that \\( P_1(B) = P_2(B) \\). 

To solve the problem, it suffices to prove that \\( \sigma(\mathscr{F}_0) = \mathscr{H} \\).

It is easy to get that \\( \mathscr{F_0} \subset \mathscr{H} \subset \sigma(\mathscr{F}_0) \\).

Next, we are going to prove that \\( \mathscr{H} \\) is a monotone class. It suffices to prove that following properties:

* if \\( A_1, \dots, A_n, \dots \in \mathscr{H} \\) and \\( A_1  \subset \dots \subset A_n \subset \dots \\), then 
\\( \bigcup_{i=1}^{\infty} A_i \in \mathscr{H} \\),
* and, if \\( A_1, \dots, A_n, \dots \in \mathscr{H} \\) and \\( A_1  \supset \dots \supset A_n \supset \dots \\), then 
\\( \bigcap_{i=1}^{\infty} A_i \in \mathscr{H} \\).

We are going to prove the first one. The second one can be proved in a similar way.

Let \\( A_{\infty} = \bigcup_{i=1}^{\infty} A_i \\). Then \\( P_1 (A_{\infty}) = \lim_{i\rightarrow \infty} P_1(A_i) = \lim_{i\rightarrow \infty} P_2(A_i) = P_2(A_{\infty}) \\). Hence, \\( A_{\infty} \in \mathscr{H} \\). The first property holds.

This completes the proof that \\( \mathscr{H} \\) is a monotone class. By MONOTONE CLASS THEOREM, we have \\( \mathscr{H} \supset \sigma(\mathscr{F}_0) \\). 

Above all, we can get \\( \sigma(\mathscr{F}_0) = \mathscr{H} \\).

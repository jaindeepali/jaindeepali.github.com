---
layout: post-no-feature
title: "Notes on Reinforcement Learning: Exact Methods for Solving MDP"
comments: true
category: articles
tags: [Reinforcement Learning, Lecture Notes]
description: ""
---

In the last post, I introduced the concept of Markov Decision Processes. The solution of MDP is an optimal policy.

**Utilities:** Now, let us think more about the objective of an optimal policy. As mentioned earlier, the goal of the agent is to maximize long-term expected reward. What exactly do we mean by that? We want to take the best action in the current state such that the sequence of states and actions that we observe here-after leads to maximum total reward. It makes sense then, to talk about some kind of measurable _utility_ of that sequence. If the utility of a sequence, $[(s_1,a_1),(s_2,a_2),(s_3,a_3),...]$ is greater than $[(s'_1,a'_1),(s'_2,a'_2),(s'_3,a'_3),...]$, then, we would like to choose an action such that the probability of observing the first sequence increases. Now we can state a property of this utility measure called **stationarity of preferences**. By that, we mean that the relative difference in utilities of two sequences stays the same at each time-step. Formally,

$$\text{If} \quad U([(s_1,a_1),(s_2,a_2),(s_3,a_3),...]) > U([(s'_1,a'_1),(s'_2,a'_2),(s'_3,a'_3),...]) \\

\text{then,} \quad U([(s_0,a_0),(s_1,a_1),(s_2,a_2),...]) > U([(s_0,a_0),(s'_1,a'_1),(s'_2,a'_2),...])$$

Note that, this property only holds in the case of infinite horizons. This stationarity of preferences leads to a specific formulation of utility as the sum of rewards of states over the sequence. Let us derive this formulation.

Utility in scenarios other than infinite horizons need not take this form. Here are some examples: 

Now, this is all good but we have a little problem here. We can't really compare utility of infinite sequences with positive rewards. Because utility defined as the cumulative sum will always tend to infinity irrespective of the individual rewards values. To tackle this problem we define our utility to be _discounted_ sum of rewards like so:

$$U([s_1,s_2,s_3,...]) = R(s_1) + \gamma R(s_2) + \gamma^2 R(s_3) + ...$$

This gives us a finite sum and is still consistent with the assumption of stationarity of preferences.

**Bellman Equation:** Okay, so now that we know about utilities, we can try to start solving for optimal policy. Let us say, an agent is in state $s_0$ and has decided to follow a policy $\large{\pi}$ from that point on. What is the sequence of events it will encounter? It depends on the policy $\large{\pi}$ and the transition function. Let the observed sequence be $[(s_1,a_1),(s_2,a_2),(s_3,a_3),...]$. Then we can say that the utility or value of being in state $s_0$ is equal to the expected utility of the sequence $[(s_0,a_0),(s_1,a_1),(s_2,a_2),...]$. This is written as:

[Insert equation]

Note that the value of the state is different from the reward received in that state. Reward gives us the immediate feedback from the environment but the value of a state captures the long-term feedback and helps in tackling the credit assignment problem. Also note that this utility is dependent on the policy that is being followed. Ideally, to get the 'true value' of a state we should consider what will happen if we somehow  become very smart and start following the optimal policy from then on. But what *is* the optimal policy? Well, it is the policy that requires me to take an action that maximizes my expected utility in every state. Ponder on that recursive nature of this problem for a while. Can we tame this circular relationship between value and optimal policy? Let's first try to formally write it down.

[Insert bellman equation]

This is the Bellman Equation.

**Value Iteration:** Now we are going to solve the Bellman Equation. Essentially, here we have a set of n non-linear (because of the max operator) equations in n unknowns. I seems a bit tricky to solve this. But there is a pretty simple algorithm to solve this, called 'Value Iteration' which is carried out as follows:

Now, let us implement this algorithm for our Frozen Lake environment.

[Insert value iteration algo]

But why does value iteration work? It turns out that there is a beautiful proof of convergence of value iteration. We will talk about that later in more detail. Intuitively, the true value of every state keeps getting propagated back to its neighboring states over the iterations. Although we start out with arbitrary values, the true value indicated by the sequence of future rewards overwhelms this noise in the long run. 

**Policy Iteration:** Let's discuss another algorithm for solving the Bellman Equation, called Policy Iteration. This one is conceptually pretty similar to value iteration but this time the update happen directly in the policy space instead of the value space. 

Iterating in policy space feels like we are taking bigger jumps towards the true solution as compared to value iteration. Let us test this intuition and verify if indeed we end reaching the solution in fewer steps as compared to value iteration. So let's go ahead and implement the policy iteration algorithm.
[Insert policy iteration algo]
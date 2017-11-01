---
layout: post-no-feature
title: "Notes on Reinforcement Learning: Overview and MDP"
comments: true
category: articles
tags: [Reinforcement Learning, Lecture Notes]
description: ""
---

<!-- I recently completed a Udacity course on [Reinforcement Learning](https://in.udacity.com/course/reinforcement-learning--ud600). It is an advanced course on the subject being taught at Georgia Tech as CS 8803. 
The instructors of this course are [Charles Isbell](https://www.cc.gatech.edu/fac/Charles.Isbell/) and [Michael Littman](http://cs.brown.edu/~mlittman/).
The course does an awesome job of teaching this involved material in a very engaging manner. I highly recommend checking out this course on Udacity if you are excited by Reinforcement Learning and are looking to get deeper and more theoretical understanding of the field. You may also explore the more basic and general [Machine Learning](https://in.udacity.com/course/machine-learning--ud262) course taught by the same instructors. 
 -->
<!-- In this series of posts I will document my learnings from the course, the corresponding literature research and some experimentation. This is an exercise for me to revisit the content and explore some interesting research problems in more depth and to keep everything I have learned in one place. -->

### Reinforcement Learning Overview
RL is a sub-field of artificial intelligence that deals with making agents learns to make decisions in new environments. The agents should learn by interacting with the environment and observing occasional reward (or reinforcement) signals rather than from direct supervision. <!-- Since you are here, I am sure you know how RL is awesome and has been used to solve all kinds of exciting problems. Okay, enough talk! Let's get to the real stuff. -->

### Markov Decision Process
**Definition:** The single agent reinforcement learning problem is usually formalized using the classical concept of Markov Decision Processes. An MDP is characterized by the tuple $\langle S, A, R(s,a), T(s,a,s') \rangle$. Consider an environment with $n$ states. $S$ denotes the set of all states in the environment. The environment is stochastic and stationary. An agent interacting with this environment can take an action out of the $A$ of available actions in any state s. The probability of transitioning from a state, $s$ to the next state, $s'$ after the agent has taken an action $a$, is given by the transition function $T(s,a,s')$. $T$ represents the set of rules or physics of the environment. The Markovian property holds i.e. only the current states matters in deciding the probability of transition to next state. Thus agents actions have consequences in the environment as they influence the probability distribution of transition to next state. $R(s,a)$ represents the scalar reward that the agent receives for taking an action a in state s. The process of observing a state, taking an action, receiving the reward and moving to the next state keeps repeating till the end of the episode.

<!-- Charles and Michael take the example of a grid world to explain MDPs. That example is very similar to the frozen lake environment in [OpenAI Gym](https://gym.openai.com/envs/FrozenLake-v0/).  -->
Consider the frozen lake environment in [OpenAI Gym](https://gym.openai.com/envs/FrozenLake-v0/). Let us play with this environment to get the hang of the concept of MDPs. Install gym from [here](https://github.com/openai/gym) and get started.

{% gist jaindeepali/d08505b9ab35661b47e324dc391cde4a %}
<!-- {% include jupyter_notebooks/gym_demo.md %} -->

You can explore this simple MDP and observe the stochastic transition between states when the agent takes various actions. Now that you understand the problem set up, we can start thinking about how to solve it.

**Solving an MDP:** There exist many algorithms for solving an MDP. Let us first discuss what it means to solve an MDP. The goal of the agent in an MDP is to maximize the long-term expected reward. The solution of an MDP is a **policy** which is a mapping from every state to a specific action. The goal is to learn an optimal policy which guarantees maximum reward. This is what makes RL different from supervised learning. We don't have any kind of supervision here which gives us examples of good state-action pairs. We have to learn this mapping solely based on the reward signal which is often delayed. Delayed in the sense that for a good action that you took right now you might not see any payoff immediately but it might put in such subsequent states that eventually get a high reward. Figuring out what actions lead to a certain sequence of future rewards is the difficult problem of **temporal credit assignment**.

Note at this point, the difference between a plan and a policy. A plan is a sequence of actions that the agent has decided to execute in the beginning of the MDP, while a policy just describes what action to take when it reaches a certain state, if ever. Since we are dealing with a probabilistic transition function, executing a plan might not be the best idea since the actions don't always have the intended effects in a stochastic world. And as soon as the world throws agent off its plan to a completely new state, it will have no clue as to how to proceed. 

Another important point to note is that assignment of rewards while designing an MDP to capture certain environment is a way of injecting our domain knowledge into the system. The reward function should be designed very carefully as slightly different reward structures can induce very different agent behavior.

**Horizon:** Okay so, here we are dealing with infinite horizons. What that means is that there is no fixed duration for the sequence of time-steps till end of the process. There may be terminal states such that the process ends upon reaching them but time to reach those states is indefinite. In essence, the agent can live forever and has infinite time to work with until it reaches a terminal state. It is to be appreciated that absence of infinite horizon assumption, can lead to drastically different agent strategies. For example, in a finite horizon shorter version of Frozen Lake environment, it might make more sense to take the quicker risky route instead of the safer and longer one as you don't have enough time for the latter. More importantly, in a finite horizon case, the policy will be time dependent, i.e. you might want to take different actions from the same state depending on how much time is left before the end of process. The policy no longer remains stationary as in infinite horizon case.

Alright! This gives us a pretty good understanding of the formal set-up of Markov Decision Process. As I mentioned in the beginning, this is the most popular framework for many Reinforcement Learning tasks. But, it is to be remembered that this is not the *only* framework. There exist scenarios where this framework is not sufficient to model all the complexity of the environment, for example, in the case of partially observed states or multi-agent environments. Moreover, other frameworks exist, such as [predictive state representations](http://web.eecs.umich.edu/~baveja/Papers/psr.pdf) which provide a *non-Markov* way of representing dynamical systems. Different formalisms have their own pros and cons. Each one lends itself naturally to a certain environment and facilitate learning in that environment when others don't. Choosing a good representation itself should be seen as a part of the solution of the general problem of creating intelligent agents. That said, MDPs are often very successful in modeling many scenarios and thus, it is a great place to start getting a deeper understanding into the subject of Reinforcement Learning.

I hope you enjoyed this introduction and are now excited to learn some algorithms solve the MDP already! Let's do that in the next post.


### References
* [Reinforcement Learning](https://in.udacity.com/course/reinforcement-learning--ud600), Udacity
* [Deep RL Bootcamp](https://sites.google.com/view/deep-rl-bootcamp/lectures), Berkeley CA

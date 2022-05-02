---
title: Reinforcement Learning Based Path Planning for Robots
date: 2021-11-29 16:56:23
tags: [path planning, robots, reinforcement learning, optimization]
lang: zh-CN
categories:
- [科研]
- [Research]
---

These days I have studied unmanned driving path planning and reinforcement learning, and have gained some preliminary knowledges.

In reinforcement learning, there are some simple models, such as letting the agents find a way out of the maze, which may be similar to unmanned driving path planning. To do this kind of reinforcement learning problems, the most used way is try-and-error. That is a lot of attempts, according to the final return (find the exit or not) to update themselves in a certain state (position). Then the agents select a certain action (moving forward, backward, left, and right), so as to explore an optimal policy.

The advantage of using reinforcement learning to do this is that you only need to tell the agents your purpose (find the way out), without telling them how to do it. As long as you try more times, the agent can always find a more ideal strategy (the value function converges), which is the path of action. The specific algorithms can be Monte Carlo or Q-learning. You also can try the Dyna-Q algorithm.

<!-- more -->

$ Q-learning is the core of RL $

$ Q stands for Quality $

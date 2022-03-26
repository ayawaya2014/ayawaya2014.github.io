---
title: Adaptive sliding mode formation control
date: 2019-04-20 09:17:19
tags: [Programming, AUV, Water Flow, Formation Tracking]
categories: 科研
mathjax: true
lang: zh-CN
---

Adaptive sliding mode formation control for AUVs in water flow environment.

[Source Code](https://github.com/ayawaya2014/smc_code)

<!-- more -->

1. Sliding mode control (dynamics).

2. Water flow (Ocean Current) mathematic model. 

   Model of water flow is established for simulation. Red lines defines the potentials and blue arrows are velocities of the flow field.

   The first current model is 
   $$
   Fc = 1 - tanh\frac{y-B(t)*cos(k*(x-c*t)+\alpha)}{\sqrt{(1+k^2*B^2(t)*sin^2(k*(x-c*t)+\alpha)}}
   $$

   where 

   $$
   B(t) = B_0+e*cos(w*t+\theta)
   $$

   according to literature [1], [2].

   ​

   ![oc](/images/oc1.png)

   ![]()

3. To  be continued...


[1] 徐玉如, 姚耀中. 考虑海流影响的水下机器人全局路径规划研究[J]. 中国造船, 2008, 49(4): 109-114.

[2] Alvarez A, Caiti A and Onken R. Evolutionary path planning for autonomous underwater vehicles in a variable ocean[J]. IEEE Journal of Oceanic Engineering, Apr. 2004, 29(2): 418-429.
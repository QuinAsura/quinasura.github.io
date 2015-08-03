---
layout: page
title: Projects
excerpt: "My Publications"
modified: 2014-08-08T20:04:41.231140-04:00
image:
  feature: front_page.jpg
---
* Table of Contents
{:toc}

##Semi-Autonomous Control of Quadrotor
`mentor`- *Prof. V.Kamaraj (HOD EEE/SSNCE)* , for my final year project.<br/>
`Team of two members`

<iframe width="560" height="315" src="http://www.youtube.com/embed/PWf4WUoMXwg" frameborder="0"> </iframe>

* Responsible for implementation of the two-channel PID controller for roll and pitch control of a quadcopter
on Spartan 3E FPGA using Matlab/Xilinx(ISE).
* Aided in the indigenous construction of the quadcopter frame and interfacing of the 9 degree of freedom
inertial measurement unit with the FPGA.
* Aided in the testing and implementation of the control algorithm in arduino microcontroller.
 
---

##LOC (Localization, Obstacle avoidance and Control) 
* Implemented Localization and Pose estimation based on arbitrary defined color contours detection using
OpenCV C++ library.
* Implemented A* based search on the image to find the optimal path through a obstacle maze.
* Implemented a PID based control system based on the kinematics of the differential driving system.

>[Leo Raju, Sibi Sankar, R.S. Milton, Procedia Computer Science, Volume 46, 2015, Pages 231-239, ISSN 1877-0509](http://www.sciencedirect.com/science/article/pii/S1877050915000800)

![multi-agent cq-learning]({{ site.url }}/images/rl1.png)
{: .pull-left}
In the distributed optimization of micro-grid, we consider grid connected solar micro-grid system which contains a local consumer, a solar photovoltaic system and a battery. The consumer as an agent continuously interacts with the environment and learns to take optimal actions. Each agent uses a model-free reinforcement learning algorithm, namely Q Learning, to optimize the battery scheduling in dynamic environment of load and available solar power. Multiple agents sense the states of the environment components and make collective decisions about how to respond to randomness in load, intermittent solar power using a Multi-Agent Reinforcement Learning algorithm, called Coordinated Q Learning (CQL). The goals of each agent are to increase the utility of the battery and solar power in order to achieve the long term objective of reducing the power consumption from grid.

Keywords - Solar micro-grid; Multi-agent Reinforcement Learning; CQ-learning; battery scheduling; optimization
{:.notice}

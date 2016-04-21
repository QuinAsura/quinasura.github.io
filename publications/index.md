---
layout: page
title: My Publications
excerpt: "My Publications"
search_omit: false
modified: 2014-08-08T20:04:41.231140-04:00
---
* Table of Contents
{:toc}

## Feedback Linearization and PID Control of Aero Thrust Pendulum using FPGA.

>[Aust. J. Basic & Appl.Sci, 2014, pp.466,472, Dec.](http://ajbasweb.com/old/ajbas/2014/December/466-472.pdf)

![Schematic Representation of Aero Thrust Pendulum]({{ site.url }}/images/aero1.png)
{: .pull-left}
This paper presents a model based design and implementation of a closed loop PID
controller for `Aero Thrust Pendulum`. It was done using `MATLAB - System Generator Toolbox`, to
demonstrate the Direct Real Time Simulation and Implementation `(DRTSI)` scheme.
A method for live tuning of proportional, integral, derivative and system gain is validated and implemented to facilitate robust
tuning. This serves to highlight the effect of each component of the PID controller on
the response of the system for `educational` purposes. Also, `Serial` communication is established between FPGA and
MATLAB/Simulink to facilitate extensive logging of the system’s output and provides
visual representation of the system’s response for various gains.
 
Keywords - MATLAB – System Generator; FPGA-Serial; Aero Thrust pendulum; FPGA-ADC
{:.notice}
---

## Reinforcement learning for optimal energy management of a solar microgrid.

>[Global Humanitarian Technology Conference - South Asia Satellite (GHTC-SAS), 2014 IEEE vol., no., pp.183,188, 26-27 Sept](http://ieeexplore.ieee.org/xpl/articleDetails.jsp?arnumber=6967580)

![Switching States]({{ site.url }}/images/rl2.png)
{: .pull-left}

In an optimization based control approach for solar microgrid energy management, consumer as an agent continuously interacts with the environment and learns to take optimal actions autonomously to reduce the power consumption from grid. A model-free Reinforcement Learning algorithm, namely three-step-ahead Q-learning, is used to optimize the battery scheduling in dynamic environment of load and available solar power. Solar power and the load feed the reinforcement learning algorithm. Simulation results using real numerical data are presented for a reliability test of the system. The uncertainties in the solar power and the load are taken into account in the proposed control framework.

Keywords - Optimization; Q-learning; Reinforcement learning; Solar microgrid.
{:.notice}
---

## Distributed Optimization of Solar Micro-grid Using Multi Agent Reinforcement Learning.

>[Leo Raju, Sibi Sankar, R.S. Milton, Procedia Computer Science, Volume 46, 2015, Pages 231-239, ISSN 1877-0509](http://www.sciencedirect.com/science/article/pii/S1877050915000800)

![multi-agent cq-learning]({{ site.url }}/images/rl1.png)
{: .pull-left}
In the distributed optimization of micro-grid, we consider grid connected solar micro-grid system which contains a local consumer, a solar photovoltaic system and a battery. The consumer as an agent continuously interacts with the environment and learns to take optimal actions. Each agent uses a model-free reinforcement learning algorithm, namely Q Learning, to optimize the battery scheduling in dynamic environment of load and available solar power. Multiple agents sense the states of the environment components and make collective decisions about how to respond to randomness in load, intermittent solar power using a Multi-Agent Reinforcement Learning algorithm, called Coordinated Q Learning (CQL). The goals of each agent are to increase the utility of the battery and solar power in order to achieve the long term objective of reducing the power consumption from grid.

Keywords - Solar micro-grid; Multi-agent Reinforcement Learning; CQ-learning; battery scheduling; optimization
{:.notice}

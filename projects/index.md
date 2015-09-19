---
layout: page
title: Projects
excerpt: "My Projects"
modified: 2014-08-08T20:04:41.231140-04:00
image:
  feature: front_page.jpg
---
* Table of Contents
{:toc}

##Semi-Autonomous Control of Quadrotor
`mentor`- *Prof. V.Kamaraj (HOD EEE/SSNCE)* <br/>
`Team Members: Sibi Sankar, Sanjay Shreedharan`

<iframe width="640" height="360" src="https://www.youtube.com/embed/Ulz9EfYzbyA" frameborder="0" allowfullscreen></iframe>

* Responsible for implementation of the two-channel PID controller for roll and pitch control of a quadcopter
on Spartan 3E FPGA using Matlab/Xilinx(ISE).
* Aided in the indigenous construction of the quadcopter frame and interfacing of the 9 degree of freedom
inertial measurement unit with the FPGA.
* Aided in the testing and implementation of the control algorithm in arduino microcontroller.
 
---

##Modelling and Feedback Linearization of Aero-Thrust Pendulum
`mentor`- *Prof. M.Senthil Kumaran* (EEE/SSNCE)<br/>
`Sibi Sankar, Sanjay Shreedharan`

<figure class="side">
	<img src="/images/aero1.png" alt="image" style="width: 200px;float: centre;">
        <img src="/images/aero2.jpeg" alt="image" style="width: 200px;float: centre;">
	<img src="/images/aero3.jpeg" alt="image" style="width: 200px;float: centre;">
        <figcaption>Hardware Implementation of the Aeropendulum Setup.</figcaption>
</figure>

* Responsible for implementation of the single-channel PID controller with anti-reset for aero-thrust pendulum
on Spartan 3E FPGA using Matlab/Xilinx(ISE)
* As a part of a team of two, implemented the data logging system on Spartan 3E FPGA interfaced to
Matlab through Real time Windows target and aided in feedback-linearization of the non-linear system.
* Facilitated the implementation of the online tuning of PID parameters with on-board switch of the FPGA.

---

##LOCK
 
######(Localization, Obstacle avoidance , Control and Kinematic framework) 
`mentor` - *Prof. M.Senthil Kumaran* (EEE/SSNCE)<br/>
`Sibi Sankar, Sanjay Shreedharan, Adithya S`

* Implemented Localization and Pose estimation based on arbitrary defined color contours detection using
OpenCV C++ library.
* Implemented A* based search on the image to find the optimal path through a obstacle maze.
* Implemented a PID based control system based on the kinematics of the differential driving system.

---

##Magnetic Levitation System
`mentor` - *Prof. M.Senthil Kumaran* (EEE/SSNCE)
`Sibi Sankar, Sanjay Shreedharan, Adithya S`

<iframe width="560" height="315" src="https://www.youtube.com/embed/DL33ijUAX18" frameborder="0" allowfullscreen></iframe>
* Designed a Prototype of Magnetic Levitation System.
* Implemented Hysteresis Control to achieve magnetic levitation using Spartan 3E FPGA and Arduino.

---

##Digital Painter
`mentor` - *Prof. M.Senthil Kumaran* (EEE/SSNCE)
`Adithya S, Sibi Sankar, Sanjay Shreedharan`
<img src="/images/output.gif" alt="image" style="width: 400px; float: centre;">

* Implemented mathematical functions on Spartan 3E FPGA and used PWM technique to display basic shapes in a DSO.
* Developed a technique to display any given image in a DSO with the help of image processing and python.

Checkout the blog post about this project [here](https://adithyaselv.quora.com/Engineering-Art-and-Python-Connecting-the-Dots)

---

##Quickbot using Beaglebone Black
* As a team of two, implemented the quickbot and responsible for design of the three amps regulator circuit
using 7805 and callibrated the infrared proximity sensor.
* Compiled from source the wifi driver rtl8192E firmware on the debian distribution of the Beaglebone
black enabling wireless debugging of data.
* Tested various control theory concepts from PID control to hybrid automata model for switching between
various behaviour models.

---

##Space Invaders Game using TM4C123G

* Interfaced the cortex-M4 processor with the Nokia 5110 LCD display used to display the graphics required
for the game.
* Interfaced the microcontroller with a potentiometer which was used as a joystick to control the spaceship.
* Implemented the algorithm that provides for the joint operation of the DAC to produce the sound required
in adherence with Nyquist theorem and interrupt triggering of the entire game.

---

##Variegated Learner Anime Downloader(VLAD) using Wxpython

![VLAD Load Episode window](http://i.imgur.com/eeJeeRU.png?1)

* Responsible for the download link extraction algorithm and multi-threaded downloader implementation.
* Implemented a GUI based data scraping, multithreaded downloader using WxPython for anime shows
and its source code was published in [github](https://github.com/QuinAsura/VLAD)


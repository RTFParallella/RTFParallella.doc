############
Introduction
############

This file is intended to be an introduction that explains the main functionality of RTFParallella framework. 

RTFParallella is a framework that allows the easy implementation of nulti-core, real-time applications on the Adapteva Parallella_ hardware platform by using the 16-core Epiphany III co-processor on board and exploiting the Network on Chip (NoC) architecture of that chip for deterministic multi-core implementations of automotive software. 
The organization of implementation code within the framework as well as user code has been arranged according to Amalthea_. models of a multi-core system (both hardware and software artifacts), The code will only require attributes of tasks and their deployment on cores as a tuple of Amalthea parameters. 

This documentation will include the following:

*	A step-by-step guide on the setup of the Adapteva Parallella Hardware platform.
*	A guide on setting up the eclipse IDE for cross compilation nad deployment/ running of RTFParallella code on Adapteva Parallella. 
*	A guide showing the intended usage of the framework and how to manipulate such framework to realize the desired real time behavior. 
*	The design of RTFP and how it could be adapted to other hardware platforms. 
*	Limitations that should be taken into account when using RTFP. 


















.. _Amalthea : https://www.eclipse.org/app4mc/help/app4mc-0.9.4/index.html#section3.1.1
.. _Parallella : https://www.adapteva.com/parallella/
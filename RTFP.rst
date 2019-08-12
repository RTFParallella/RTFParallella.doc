##################################
Real Time Framework Parallella  
##################################

This page describes the core functionality of Real Time Framework Parallella, for example code and further technical explanation

Introduction
-----------------------------

This product is a C framework to help in early prototyping of multi core systems based on AMALTHEA models. It will accept parameters from the model and realize it in C code which can be tested and benchmarked on existing hardware. As the name suggests, this framework is designed to run on the Adapteva Parallella platform. However, it is possible to adapt it to other platforms. 

Platform dependencies 
---------------------------

This framework has been bult using FreeRTOS. It uses task control blocks (TCBs), critical section APIs, and task handling APIs from FreeRTOS.

The FreeRTOS port for Adapteva Parallella has been first written `here <https://github.com/ralisi/FreeRTOS>`_. That implementation has been modified to achieve tick accurate, reproducable task switching behaviour. 

This framework uses Epiphany Software Development Kit (ESDK). the latest ESDK version (2016.11) was used. 

How to use RTFP
-----------------------------

RTFP is designed to be used with AMALTHEA models (constructed and analyze using APP4MC). 

With this in mind, in order to use RTFP, the following steps can generally be used:

1.	Amalthea model is constructed and verified in APP4MC using analysis plugins, visualization plugins... etc. 

2.	Tasks attributes are used to construct tasks in RTFP. 

3.	Task and memory operation code (copying labels, etc) are constructed. 

4.	Runnable code is constructed. 

5.	Shared memory is allocated statically. 

6.	RTOS tasks are created using the AmaltheaTask objects created in step 2

7.	communication between the epiphany coprocessor and the host is defined (optional)

8. constarints are added on task status data to benchmark the system and verify real time aspects. 


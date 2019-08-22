.. rtfparallella.docs documentation master file, created by
   sphinx-quickstart on Sun Jun  2 14:41:49 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Real Time Framework Parallella : documentation
==============================================


############
Introduction
############

Source coode for this project can be currently found here_. It will be merged with eclipse App4MC repository shortly.

This file is intended to be an introduction that explains the main functionality of RTFParallella framework. 

What is RTFParallella
-----------------------------------------------

RTFParallella is a framework that allows the easy implementation of nulti-core, real-time applications on the Adapteva Parallella_ hardware platform by using the 16-core Epiphany III co-processor on board and exploiting the Network on Chip (NoC) architecture of that chip for deterministic multi-core implementations of automotive software. 
The organization of implementation code within the framework as well as user code has been arranged according to Amalthea_. models of a multi-core system (both hardware and software artifacts), The code will only require attributes of tasks and their deployment on cores as a tuple of Amalthea parameters. 

Contribution
-------------------------------------------

This project has been started in the context of Google summer of code. During Google Summer of Code the following has been contributed to RTFParallella:

*	Fixed the FreeRTOS port for Epiphany III processor to be able to realize RMS schedulling. 
*	Relaized RMS scheduling policy for tasks on Epiphany processor.
*	Adapted Amalthea model task structure to run using FreeRTOS.
*	Added support for shared memory management and distributed shared memory management. 
*	Added visuallization of real-time behavior of amalthea model on EpiphanyIII processor. 
*	Added examples that could be changed (manually or by code generation) to realize any Amalthea model. (given it fits within the parallella hardware constraints).

Quick start
-------------------------------------------

*	Clone RTFParallella source code and examples here_.
*	Set up cross complilation environment as in :ref:`Sphinx Overview`.
*	run `patch_init.sh`

.. code-block:: bash

   sh patch_inti.sh

*	run `make all` from `src/parallella`

*	set up the deployment script as in :ref:`RST parallella_deployment`.

*	deploy the binaries to Parallella (`.elf files`) and (`host_main`) using `parallellaDeploy`

*	SSH into parallella board and run host_main


Documentation scope
-----------------------------------

This documentation will include the following:

*	A step-by-step guide on the setup of the Adapteva Parallella Hardware platform.
*	A guide on setting up the eclipse IDE for cross compilation nad deployment/ running of RTFParallella code on Adapteva Parallella. 
*	A guide showing the intended usage of the framework and how to manipulate such framework to realize the desired real time behavior. 
*	The design of RTFP and how it could be adapted to other hardware platforms. 
*	Limitations that should be taken into account when using RTFP. 


Index
-------------------------------

.. toctree::
   :maxdepth: 2
   :caption: Contents:
.. toctree::
   :maxdepth: 2
   
   parallella_hardware

   parallella_setup

   parallella_deployment

   RTFP

   RTFP_tasks

   shared_mem

   dstr_shared_mem

   utils

   utils_2

   utils_3




.. _Amalthea : https://www.eclipse.org/app4mc/help/app4mc-0.9.4/index.html#section3.1.1
.. _Parallella : https://www.adapteva.com/parallella/
.. _here : https://github.com/mahmood1994ha/RTFParallella
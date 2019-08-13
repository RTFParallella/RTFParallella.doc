##########################
Task management in RTFP
##########################

Overview
-------------------

In real time multi-core systems, it is essential to realize separate tasks that perform -usually- independent actions. RTFP follows the `OSEK <https://www.irisa.fr/alf/downloads/puaut/TPNXT/images/os223.pdf>`_ standard to manage tasks. 

Since it was based on FreeRTOS, RTFP has the semantics of OSEK within the FreeRTOS kernel, with different names 

*	Running:

*	Blocked:

*	Waiting:

*	Ready:

TODO: explain the different states.


RMS task scheduling
-----------------------

RTFP will schedule tasks according to RMS (Rate Monotonic Scheduling) policy by default. 

TODO: is it required to elaborate further on RMS and how it works?.

EDF task scheduling
----------------------

TODO: explain EDF and how to choose it during compilation time. 


Creating tasks using RTFP
----------------------------



To create a task in RTFP, first initialize an amaltheaTask struct as follows 

.. code-block:: CPP

   AmaltheaTask createAmaltheaTask(void *taskHandler,
   	void *cInHandler,
   	void *cOutHandler,
   	unsigned int period,
   	unsigned int deadline, 
   	unsigned int WCET);

arguments:

*	taskHandler : pointer to the function that contains task code 

*	cInHandler	: pointer to the function that creates a copy of shared varialbles (labels) on the task stack. This function will be called at the beginning of every new instance of a task.

*	cOutHandler	: pointer to the functions that returns the local copy of shred variables into shared memory. (write operation).

*	period		: period of the task in ticks 

*	deadline	: relative deadline in ticks

*	WCET		: Worst Case Execution Time in ticks. 

This function will return a struct of type AmaltheaTask. This struct will be used to create an RTOS task as follows:

.. code-block:: CPP

   	xTaskCreate(generalizedRTOSTak	,
   				"TASK_IDENTIFIER"	,
   				configMINIMAL_STACK_SIZE,	
   				AmaltheaTask &task	,
   				int priority,
   				xTaskHandle handle);

arguments

*	generallizedRTOSTask	: This is the RTOS task structure used to handle the start (and end) of each job of the task. This task can be configured in two ways to allow either implicit or LET (Logical Execution Time) communication semantics. 

*	TASK_IDENTIFIER			: This string is normally used for tracing purposes in FreeRTOS, currently it is not being used by RTFP. 

*	configMINIMAL_STACK_SIZE:


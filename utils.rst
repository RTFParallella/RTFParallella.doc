#################################
Utility functions - task tracing
#################################

Overview
----------------------

Since RTFP runs a RTOS on the epiphany processor, it is not possible to output debugging and tracing messages in real time. 

The solution to thiis problem is writing specific debug flages to a memory buffer (which can be done in real time), and reading that memory buffer continuously from by the ARM host which can interpret and dispay these flages. 

To summarize, ARM host will be polling the debug buffer for timely information on the current state of every Epiphany core separately. 


Tracing running taks
----------------------

to trace a running task, the following function can be called at any point during a task's execution. 

.. code-block:: CPP

   	void traceRunningTask(unsigned taskNum);

To trace the instance of a task, the following function can be called at the release of a task. 

.. code-block:: CPP

   	void traceTaskPasses(unsigned taskNum, int currentPasses);

Where currentPasses is the current instance of the task identified by its number taskNum.

Tracing time
----------------------

To trace time progress in the Epiphany processo rwhen running RTFP, the following function has been added to the tick interrupt such that the tick changes will be traceable as soon as the tick interrupt handler is called. This will ensure tick accuracy of the time readout. 

.. code-block:: CPP

   	void updateTick(void);

To capture time progress on the ARM host side, the loop polling debug buffer should be timed to the same tick period set up in RTFP. The function 

.. code-block:: C

	int nsleep(long miliseconds);

can be used to time the loop to make sure the epiphany is traced fast enough while no undersampling occurs. 

Debug traces
----------------------

The following function will write a user specified debug code to the debug buffer

.. code-block:: CPP

	void updateDebugFlag(int debugMessage);

It can be used to troubleshoot the system in case of failure to switch tasks or start RTFP. 




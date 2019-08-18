#######################################
Utility functions - task operations
#######################################

Overview
-------------------------

RTFP is based on Amalthea models, therefore, in order to verify the behavior described by the model, without implementing the functiinality is "cycle wasting", Where the processor is used for the decided WCET (Worst Case Execution Time) of the task.

Sleep in Epiphany cores
------------------------------

Each Epiphany core has 2 hardware timers, in order to acheive cycle-accurate sleep times, the timer is used to pause the processor in its curent state for a given amount of clock cycles. The function 

.. code-block:: CPP

	void sleepTimerMs(int ticks, int taskNum);

will acheive sleep times accurate to one tick of 1 ms. 

It is important to note that any sleep funciton structured similarly to this function should not have a sleeping time that is larger than the tick period of RTFP. This funciton will devide the required sleep ticks to a series of timed sleep periods of 1 tick each, to allow the tick interrupt to run regularly while the sleep function is being used. 


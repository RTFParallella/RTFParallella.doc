##################################
Adapting FreeRTOS to RTFP
##################################

Overview
-----------------------------

RTFP uses the services from FreeRTOS to switch tasks and provide other utilities to be used in task management. 
In this file, the adaptation of FreeRTOS to RTFP will be explained, along with the limitations of FreeRTOS. 

RTFP task structure in FreeRTOS
-----------------------------------
Tasks in RTFP follow the read-execution-write semantics. while FreeRTOS tasks do not consider singl instances that start and finish at defined points time. FreeRTOS tasks are infinite loops which are swapped in and out of execution based on the scheduling policy used. 

The following adaptation of a generalized FreeRTOS task that takes AmaltheaTask struct as a parameter shows the semantics of read execution write and the determination of instances of the task as will as the way to make a FreeRTOS task periodic

.. code-block:: CPP

	void generalizedRTOSTak(AmaltheaTask task){
		TickType_t xLastWakeTime = xTaskGetTickCount();
		for (;;){
			task.cInHandler();
			task.taskHandler();
			task.cOutHandler();
			vTaskDelayUntil( &xLastWakeTime, task.period);
		}
	}

Creating FreeRTOS tasks for AmaltheaTask objects
------------------------------------------------------

Creating tasks in FreeRTOS is done using the xTaskCreate() function. This functions is used by RTFP to Connect the FreeRTOS generalized task described earlier to the AmaltheaTask structure such that it can be scheduled, switched and managed..

.. code-block:: CPP

	xTaskCreate(generalizedRTOSTak,
				"Task",
				int stack_size,	
				&(*AmaltheaTask Task),
				int priority,
				xTaskHandle handle);

Arguments:

*	stack_size : the size of the local stack of the task. The size of all labels used by the task should be considered when calculating the required stack size. Also, additional stack should be added for the task control block (TCB) of the task.

*	AmaltheaTask task : a pointer to the AmaltheaTask struct that will be passed to the generalized FreeRTOS task.

*	priority : Priority of the task. 

*	handle : handle to the FreeRTOS task created, currently not used by RTFP.


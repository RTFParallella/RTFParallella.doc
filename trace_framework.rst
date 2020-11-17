###################################################
BTF Tracing Framework
###################################################

Overview
--------

This section describes the BTF tracing framework over RTFParallella. The 
framework has been developed in C language. The framework however, is
completely independent of the hardware platform and any system model simulation
framework.


The tracing framework supports BTF trace generation of the tasks executed on a 
multicore heterogeneous platform. The current framework is restricted to task
execution on two cores. However, this capablility can be enhanced based on the 
cores used to simulate the system model.

Compilation Steps
-----------------

The framework has been been compiled using Epiphany Software Development Kit(ESDK) 
with the ESDK version being 2016.11.

The headers and the source file related to tracing framework can be compiled using any
standard C compiler. The user can also create a static library and integrate it with their
framework. The functionality of the tracing framework can be utilized by the APIs exposed
in the framework header file.


Framework Implementation Details
--------------------------------

This section describes the basic implementation details of the tracing framework. This enables
the developer or the user to get the basic understanding of the framework in order to make a 
better utilisation of this framework.

As mentioned in :ref:`btf_spec` the trace file consists of header section and data section.


The below enum defines the supported event type in tracing framework:

.. code-block:: CPP

    typedef enum btf_trace_event_type_t
    {
        TASK_EVENT,
        INT_SERVICE_ROUTINE_EVENT,
        RUNNABLE_EVENT,
        INS_BLOCK_EVENT,
        STIMULUS_EVENT,
        ECU_EVENT,
        PROCESSOR_EVENT,
        CORE_EVENT,
        SCHEDULER_EVENT,
        SIGNAL_EVENT,
        SEMAPHORE_EVENT,
        SIMULATION_EVENT
    } btf_trace_event_type;


The structure defined to hold and interpret the BTF data section is :

.. code-block:: CPP

    typedef struct btf_trace_data_t
    {
        int32_t ticks;                     /**< Tick count */
        int32_t srcId;                     /**< Source Id */
        int32_t srcInstance;               /**< Instance of the source */
        int32_t eventTypeId;               /**< Type of event Runnable , Task etc.. */
        int32_t taskId;                    /**< Task Id */
        int32_t taskInstance;              /**< Instance of the task */
        int32_t eventState;                /**< State of the event */
        int32_t data;                      /**< Notes */
    } btf_trace_data;


The supported events of the BTF specification are held in enum as defined below:

.. code-block:: CPP

    typedef enum btf_trace_event_name_t
    {
        INIT = -1,
        PROCESS_START,
        PROCESS_TERMINATE,
        PROCESS_PREEMPT,
        PROCESS_SUSPEND,
        PROCESS_RESUME,
        SIGNAL_READ,
        SIGNAL_WRITE
    } btf_trace_event_name;



Following APIs are used to generate the header section of the trace file:

.. code-block:: CPP

   void write_btf_trace_header_config(FILE *stream);

The above function is reponsible for writing the mandatory header section to the BTF trace file.
It includes the version, date of creation, input model file name and time scale.


.. code-block:: CPP

   void write_btf_trace_header_entity_type(FILE *stream, btf_trace_event_type type);

The above function writes the supported entity type in the BTF trace file.


To add the entry for entity table the following function can be used.

.. code-block:: CPP

   void write_btf_trace_header_entity_table(FILE *stream);


The entity type table can be dumped into the trace file using the below function.

.. code-block:: CPP

   void write_btf_trace_header_entity_type_table(FILE *stream);


The detailed documentation of the BTF trace framework APIs can be found `here <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.examples.git/tree/RTFParallella/docs>`_.




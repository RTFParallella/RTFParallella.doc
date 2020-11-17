#############################
BTF traces in RTFParallella
#############################

This section explains about the fixes and enhancements added to the existing RTFParallella framework. Several hooks have been added to the applications running on the Epiphany core and the Host core processor. 


Overview
--------

The capability of the RTFParallella framework has been enhanced by adding the BTF tracing framework. The current RTFParallella framework operates on two core and executes 3 tasks and 2 tasks respectively on each core. The task execution follows RMS scheduling. Addition of tracing functionality adds some overhead to the overall simulation of the system model. This overhead cannot be avoided as it takes some extra clock cycles to dump the trace information in the allocated memory area. The initial Worst Case Execution Time(WCET) was defined based on the initial requirements which did not have any scope of adding any tracing framework. Therefore, the WCET has been slightly modified to simulate the Amalthea task model along with the BTF tracing functionality.


Feature Enhancements
--------------------

* The initial implementation of RTParallella registered the timing information based on the tick count. This has now been replaced with the actual timing information. The default time scale is *ns*.

* The time taken per tick count on the Epiphany cores is now made configurable. The user can pass the time scale factor to configure the tick count. Currently, the tick count has been restricted to the 100 us and 1000 us.

* The visualization of the shared label values in the legacy RTFParallella trace was not showing the correct value, this has now been fixed.

* Synchronization between the epiphany cores as well as between epiphany core and host core processor is achieved using mutex implementation and software interrupts. 

* The RTFParallella Config Header file can be used to configuring the memory sections. However, defining the memory sections must conform to the linker script used in the application.

* Addition of the BTF tracing framework.


Adapdation on RTFParallella
---------------------------

This section covers the information about the utility functions that has been used to adapt the tracing framework to the RTFParallella. Similiar approach can be used to integrate the tracing framework on other platforms.

The below structure hold the information about the BTF trace to ensure proper synchronization within host and epiphany cores.

.. code-block:: CPP

    typedef struct btf_trace_info_t
    {
        int length;                            /**< To ensure that the mutex is initialized */
        unsigned int offset;                    /**< Mutex declaration. Unused on host  */
        unsigned int core_id;                   /**< BTF trace data buffer size which is to be read */
        unsigned int core_write;                /**< Read write operation between epiphany core and host */
    } btf_trace_info;

The supported entities in the BTF trace framework are Tasks, Runnables, Shared labels or Signals and Hardware Cores. The IDs of each entity must be defined in the RTFParallella config header file. The current ID values are allocated as:

* 0-15 integer value is reserved for the task ID.
* 16-63 integer value is reserved for the Runnables ID.
* 64-255 integer value is reserved for the Shared labels.
* 256 onwards is reserved for the hardware cores.

However, it is upto the developer to define the ID sections. Here is an example for this:

.. code-block:: CPP

    typedef enum entity_id_t
    {
        /* 0 to 15 entity ID is reserved for TASKS. */
        IDLE_TASK_ID = 0,
        TASK5MS0_ID,
        TASK10MS0_ID,
        TASK20MS0_ID,
        /* 256 to 264 reserved for HARDWARE */
        HW_CORE0_ID = 256,
        HW_CORE1_ID
    } entity_id;


Adapting Host Application
~~~~~~~~~~~~~~~~~~~~~~~~~

The host application is a normal linux executable file running on the ARM core. The command line arguments passed to the host executable is used to generate the BTF trace file along with the neccessary header information. The trace file is generated in the present working directory with the file name provided by the user. After creating the file, the BTF header is contructed using the below function:

.. code-block:: CPP

    static void construct_btf_trace_header(FILE *stream);

The above function write the mandatory header section, the supported entity type, entity table and entity type table. Each entity table must be generated based on the Amalthea task model. The model specific source file must holds the information about the Amalthea task model. This an example of the task model used in the RTFParallella:

.. code-block:: CPP

    static const char task_enum [][LABEL_STRLEN] =
    {
        "[idle]",
        "Task5ms0",
        "Task10ms0",
        "Task20ms0",
        "Task10ms1",
        "Task20ms1"
    };

The sequence of the tasks, runnables, hardware cores, shared labels must match to the sequence of the IDs created as an enumeration type in teh RTFParallella config header file.

The function parsing the command line arguments returns the scale factor which defines the time taken by each tick count on the epiphany core. The Epiphany core operates at a frequency of 700 MHz, the time taken by each tick count decided based on the scale factor and the operating frequency. The scale factor is stored in the shared memory which can be read by the Epiphany cores to adjust the timing of their tick count. The epiphany applications are then loaded on the respective cores and the host processor reads the shared memory area allocated for the trace metadata in order to read the actual BTR trace information. If any BTF data is read from the shared memory, this raw data is dumped in a temporary text file. Once the application processing is complete, the content of this text file is used to interpret and generate the BTF trace file which can be viewed by tools such as `Eclipse Trace Compass <https://www.eclipse.org/tracecompass/>`_.



Adapting Epiphany Application
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Each Epiphanny core runs FreeRTOS, which is capable of scheduling the tasks. RTFParallella has the capablility to schecdule in task based on the Rate Monotonic Scheduling(RMS) concept. In order to generate the BTF trace, the structure of the Amalthea task model has been modified to include the source instance and source ID.

.. code-block:: CPP

    typedef struct AmaltheaTask_t
    {
        unsigned int src_id;
        unsigned int src_instance;
        unsigned int task_id;
        unsigned int task_instance;
        void(* taskHandler)(int  src_id, int src_instance);
        unsigned int executionTime;
        unsigned int deadline;
        unsigned int period;
        void(* cInHandler)();
        void(* cOutHandler)();
    }AmaltheaTask;

The function which generates the Amalthea task model has also been modified as below:

.. code-block:: CPP

    AmaltheaTask createAmaltheaTask(void *taskHandler, void *cInHandler, void *cOutHandler,
        unsigned int period, unsigned int deadline, unsigned int WCET,
        unsigned int src_id, unsigned int src_instance, unsigned int task_id, unsigned int task_instance);

Apart from the previous arguments, this function also takes the argument for source ID of the task, source instance of the source, task instance and task ID. 

The function that writes the BTF trace to the shared memory area is defined below:

.. code-block:: CPP

    void traceTaskEvent(int srcID, int srcInstance, btf_trace_event_type type,
        int taskId, int taskInstance, btf_trace_event_name event_name, int data);


The synchronization between the epiphany cores as well as between the epiphany core and host core is fulfilled using the below function:

.. code-block:: CPP

    void signalHost(void);

This function make use of Epiphany SDK mutex implementation and software interrupt to achieve the synchronization.

The function details and the complete documentation of code can be found at this `link <https://git.eclipse.org/c/app4mc/org.eclipse.app4mc.examples.git/tree/RTFParallella/docs>`_.






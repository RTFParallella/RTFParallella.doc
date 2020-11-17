###############################
Limitation and Future Work
###############################

Limitations
------------

* The current trace framework supports time scale factor of 100 us and 1000 us.
* The tracing framework is restricted to two hardware cores.
* The CdGen application supports code generation for tracing framework only for RMS scheduler.



Future Work
-----------

* Support for wider range of time scaling factor.
* Adding the support for more than two hardware cores in the tracing framework.
* Reducing the memory footprint used by each packet of the BTF trace data.
* Adding the tracing framework support for other schedulers like FreeRTOS and POSIX in CdGen tool.

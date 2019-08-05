############
Adapteva Parallella setup 
############

This chapter will detail the process of setting up all tooling required for development on the parallella board for RTFP.

This setup is intended to be run on a linux machine, The steps shown here have been realized on an Ubuntu 18.04 system, it should also work with any other distributions -and versions- of linux.

Cross compilation setup
---------------------------

This framework usually requires cross compilation on a host machnine (development machine) and deployment to the Parallella board. 

This section will explain the setup for cross compilation. Deployment setup will be explained in a different section.

steps are as follows:

*	Download The Epiphany SDK (ESDK) from `Github <https://github.com/adapteva/epiphany-sdk/releases>`_. This framework has been written using the latest ESDK (v2016.11), it has not been tested on previous ESDK versions. 

*	The ESDK is released in 3 versions:

	*	armv7l: for compilation on the parallella board (this version has an arm-compatible GCC).
	*	x86-64: This is the version that was used for development of RTFP and it must be used for cross compilation of Parallella binaries on thelocal
				machine. 
	*	x86-64: This version could be used for simulating the operation of parallella on the local machine (requires at least 8GB of ram, for more details on
				that, refer to this repository).
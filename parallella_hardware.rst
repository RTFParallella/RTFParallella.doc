#####################
Parallella hardware
#####################

The Adapteva Parallella development board is designed to provide multicore functionality for high parallellism applications in a small form factor. 

Adapteva Parallella achieves a high level of parallellism by using the the 16 core Epiphany III co-processor. 

The Epiphany III co-processor is a network on chip (NoC) processor. Which performs message passing between different cores in deterministic time. 

In order to access the Epiphany co-processor, the board includes an FPGA with a soft dual core, ARM v7 processor running on it. 

Since the Epiphany co-processor is a bare-bones processor, no operating system is running on it. Therefore, many of the IO functionalities that are typical in any C program running on linux (or windows), such as pritf does not work.  
Because of this, IO of the Epiphany chip is done by simply writing the outputs to a DRAM shared memory buffer where it would be read by the ARM core running on the FPGA (since it runs linux). 

Additionally, many of the Adapteva Parallella boards come without an HDMI connector, and therefore the only way to access it is by using SSH. 

It is expectd that the user has already set up all prerequisites to establish SSH connection to the board before trying to use RTFP.


Notes
------------------------------

This board will overheat. A fan shoudl be used for cooling. If a fan is not avaiilable, place the board vertically to allow for air flow through the heat sink. 


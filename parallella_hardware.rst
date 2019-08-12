#####################
Parallella hardware
#####################

The Adapteva Parallella development board is designed to provide multicore functionality for high parallellism applications in a small form factor. 

Adapteva Parallella achieves a high level of parallellism by using the the 16 core Epiphany III co-processor. 

The Epiphany III co-processor is a network on chip (NoC) processor. Which performs message passing between different cores in deterministic time. 

In order to access the Epiphany co-processor, the board includes an FPGA with a soft dual core, ARM v7 processor running on it. 

Since the Epiphany co-processor is a bare-bones processor, no operating system is running on it. However, in order to communicate with the board and 
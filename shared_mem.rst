######################################
Shared memory management in RTFP
######################################

Overview
----------------------------------

Adapteva Parallella has a 1GB DRAM that is considered as shared memory, it is accessible by the Epiphany processor and the host arm processor as well. This memory will be used to store variables (labels) that are shared between tasks. 

shared variables are allocated in the shared memory statically. 

The labels are grouped together by size in contiguous memory blocks.

Shared memory model
-------------------------------

Shared memory address space ranges from address 0x01000000 to address 0x03FFFFFF, within this address space, all shred labels should assigned. 

All labels of the same type will be allocated to an array of that type. Accessing those labels can be done by simply accessing the index of the array. 

This figure shows the memory model of DRAM on parallella. 


Shared memory initialization and allocation in RTFP
------------------------------------------------------------

Each shared memory section will be initialized individually. If the code using RTFP is automatically generated, initialization function will be replecated for each section. 
To intialize a new shared memory section in RTFP, the following steps are required:

*	Declare the section as an array of the desired type and number of labels globally Example:
.. code-block:: CPP

   	unsigned int *outbuf_shared[10];


In this example, a block of 10 labels, each of which is of size unsigned int has been declared. Similarly, any other type could be declared. For label blocks that are too large to be declared as a standard C type, blocks of structs can also be declared. 


*	Allocate the declared memory block (array) in shared memory, example:


.. code-block:: CPP

   	//allocate initial address
	outbuf_shared[0] = (unsigned int *) shared_mem_section;
	//allocate other addresses sequentially
	for (int i=1;i<shared_section_label_num;i++){
		outbuf_shared[i] = outbuf_shared[i-1] + 1;
	}

where shared_mem_section is a macro defined with the start address of the block to be allocated, and shared_section_label_num is the number of labels in that section.

shared memory access in RTFP
----------------------------------------------------

Declared memory sections in RTFP are pointers to actual memory addresses. In order to write to a given label in a section:

.. code-block:: CPP

   	//write to shared label
   	*outbuf_shared[index] = value;

Similarly to read the label:

.. code-block:: CPP

   	read_value = *outbuf_shared[index];

In order to access the declared memory section anywhere in the project, the read and write operations should be wrapped into functions. Example:

.. code-block:: CPP

   	uint8_t shared_label_write	(int label_indx,int payload);

	unsigned int shared_label_read(int label_indx);

Those functions could be replicated for different sections.

known issues
-----------------------------------

Due to the communication semantics of task to task communication in Amalthea models, a copy of every shared label will have to be created at the beginning of the task.
However, the stack size of every task is limited and therefore on certain Amalthea models, it might be required to adjust the task's stack to prevent stack overflow.


Future developments
------------------------------------

In the next release of RTFP, the following functionalities will be added to shared memory management:

*	Allocation of memory section will be done with the use of function calls instead of creating a pointer array. Each section will have a string identifier to refer to it throughout the code.

*	Read and write functions will be standardized to access shared memory sections by their identifier string. Also automatic checks on illegal accesses will be added. 

*	Automatic allocation mechanism will be added to insure that sections are contiguous and hence avoid memory fragmentation. 





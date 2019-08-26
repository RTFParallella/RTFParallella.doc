#########################################
Utility functions - Visualization
#########################################

Overview 
--------------------------------------

RTFP includes utlities to translate the contents of output buffers of each core as well as the contents of shared memory. This is done to conform the user output from Parallella with the content of the Amalthea model used to construct the system.

Distributed shared memory visulaization 
---------------------------------------------

The struct `LabelVisual` is used to while printing the legend of the output table seen when running RTFParallella.

.. code-block:: CPP

	typedef struct{
	unsigned row;
	unsigned col;
	unsigned num_visible_labels;
	}LabelVisual;

where:

* `row`					is absolute row of the target core (core to be visualized)
* `col` 				is absolute column of the target core 
* `num_visible_labels`  is the total number of labels in core memory that will be shown in the output table.

An array that holds the indices of labels to be shown should be declared and initialized separately. Example:

.. code-block:: CPP

	unsigned index_array1[dstr_mem_sec_1_label_count];

To avoid any segmentation fault or accessing elemnts outside the array size, declare the size of the index array to be the number of elements in the target core memory section that is required to be shown. 

Shared memory visualization 
-------------------------------------

To visualize shared memory, only an array containing the required visible values of a shared memory section needs to be declared and initialized. Example:

.. code-block:: CPP

	unsigned index_array_DRAM[shared_mem_section1_label_count];

Output
-----------------------------------

As in the provided examples, all outputs from the host code running on parallella board is being printed into `stderr` buffer. RTFP provides a view utilies to print shared and distributed shared memory values and indicate value changes. 

The following sections will outline those functions and their usage. 

Output table legend
-------------------------------------

.. code-block:: CPP

	user_config_print_legend(LabelVisual core_config,unsigned array[])

Arguments:

*	core_config	:	row and column and number of visible labels for the core to be visualized. 
*	array 		:	indices of visible labels. 

.. code-block:: CPP

	user_config_print_legend_auto(unsigned array_length,unsigned array[]);

Arguments:

*	array_length	:	number of visible labels for the shared memory section to be visualized. 
*	array 			:	indices of visible labels. 

Output table values 
-----------------------------------------

.. code-block:: CPP

	user_config_print_values(LabelVisual core_config,unsigned array[],unsigned int values_array[],unsigned int prv_val_array[])

Arguments:

*	core_config		:	row and column and number of visible labels for the core to be visualized. 
*	array 			:	indices of visible labels. 
*	values_array	:	holds values of all labels in a shared memory section. 




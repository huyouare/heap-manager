/**********************************************
 * Please DO NOT MODIFY the format of this file
 **********************************************/

/*************************
 * Team Info & Time spent
 *************************/

	Name1: Jesse
	NetId1: jrh52
	Time spent: 24 hours

/******************
 * Files to submit
 ******************/

	dmm.c 	// Header file is not necessary
	README	// This file filled with the lab implementation details

/************************
 * Implementation details
 *************************/

/* 
 * This section should contain the implementation details and a overview of the
 * results. 

 * You are required to provide a good README document including the
 * implementation details. In particular, you can use pseudocode to describe your
 * implementation details where necessary. However that does not mean to
 * copy/paste your C code. Rather, describe the evaluation choices you made in
 * the design of your heap manager.  For example, it should contain about the
 * list of information you store in the metadata structure, how the coalesce,
 * free, and split operations benefited from the metadata structure, the space
 * and time overheads and tradeoffs, the results of your test cases, the amount
 * of time you spend on the lab, your name and any collaborators involved,
 * references to any additional resources used, and your feedback on the lab).

 * We expect the design and implementation details to be at most 1-2 pages.  A
 * plain textfile is encouraged. However, a pdf is acceptable. No other forms
 * are permitted.

 * In case of lab is limited in some functionality, you should provide the
 * details to maximize your partial credit.  
 * */

 First implementation:

 I used the provided metadata and macros. The metadata template provides a header that contains three pieces of data: the block size, a pointer to the next free block, and a pointer to the previous free block. An allocated block will have NULL pointers to both the previous and free block. The included variables and functions in the header provide ease of aligning the blocks.

 We then define the freelist pointer as the head of the freelist linked list. The linked list is doubly linked, and will point to NULL at the head and tail. The linked list describes all free blocks, in order of address. Though we may use another choice of implementation, this was suggested in the guide for simplicity and availability of nearby blocks in memory.

 When no free blocks are available, freelist will be NULL. Upon reading on Piazza about the initialization, my code uses a boolean to prevent more calls for virtual memory.

 The dmalloc function uses the given code, returning NULL when no blocks available, when a requested block of 0 bytes, or when no blocks big enough. It will iterate until finding a big enough block (first fit), and will allocate the block if not worth splitting (or rather, not possible or resulting in 0-size block). Otherwise it will split and return the address past the header. The decision to allocate the first part of the block was from the lab document, as is arbitrary. We then maintain the linked list by changing pointers.

 The dfree function finds the address of the header of the block address provided, and frees the block within the constraints of the linked list. It addresses cases of NULL freepointer, freeing before the first block, and will then coalesce. It uses a while loop to find the closest free block previous to the block being freed. It uses this address as well as the block past (using header->size) to determine if it is applicable to coalesce. 

 This implementation is basically following the provided lab instructions directly. It allows for malloc-ing in linear time, and freeing in O(free blocks). The chices were made based on the simplicity of implementation, and that it provides enough space and efficiency to pass the tests in test_stress2 and test_stress3. We may have included a footer for access to previous blocks more efficiently, but did not need that for our addressed order, and saved memory.

 Compared to a singly linked list, this implementation makes it easier to find the previous block for freeing and coalescing. However, we still have linear times, and malloc especially is prone to slow and inefficient in space. We might have considered a compromise with best-fit, as well as perhaps a binning implementation, where we use a data structure to store ranges of different sized blocks. We would do this to avoid unnecessary fragmentation.

  Results of testing:

  test_stress2:
  Test case summary
  Loop count: 500, malloc successful: 487, malloc failed: 13, execution time: 0.001378 seconds
  Loop count: 5000, malloc successful: 4123, malloc failed: 877, execution time: 0.015858 seconds
  Loop count: 50000, malloc successful: 41366, malloc failed: 8634, execution time: 0.135914 seconds
  
  test_stress3:
  Test case summary
  Loop count: 500, malloc successful: 486, malloc failed: 14, execution time: 0.003716 seconds
  Loop count: 5000, malloc successful: 4109, malloc failed: 891, execution time: 0.041907 seconds
  Loop count: 50000, malloc successful: 41739, malloc failed: 8261, execution time: 1.86808 seconds


/************************
 * Feedback on the lab
 ************************/

/*
 * Any comments/questions/suggestions/experiences that you would help us to
 * improve the lab.
 * */

 Great challenge! It would've been nice to have more direction into more efficient implementations, and definately stricter tests to go off of.

/************************
 * References
 ************************/

/*
 * List of collaborators involved including any online references/citations.
 * */

 Princeton lecture slides: http://www.cs.princeton.edu/courses/archive/spr11/cos217/lectures/20DynamicMemory2.pdf

 Lab materials provided, including book and provided code.

 Asked TA in Link, several quesitons on Piazza
 Assistance on guidelines from Udit A., Ariba A.

# Source Code

buddy system and slab are two kinds of free memory allocation algorithm. The nginx makes use of slab memory allocation algorithm. The nginx is made of different modules. The module is a very important concept and represented as struct type. The define is one directive of preprocessor and provides macro variable definition. The module is composed of index, name, version, context, commands, type, process, thread. The core moduel is a special type and contains the fields which are different from regular modules. 

Some module prototype functions are defined and declared to meet the needs of module initialization, addition, search and cycle. The system header files are included in the nginx configuration header file. The system header files are included together into one header file. The pool concept is introduced in the nginx and put some data into the pool. The queue data structure is used to chain the data. 

The task is assigned to thread. The current thread task knows next thread task. The thread task has one handler to process the task and event for the task process. 
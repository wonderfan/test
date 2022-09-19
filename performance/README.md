Jmeter
======

## Source Codes

### Threads

The thread functionalities are grouped into threads package. The thread group is one kind of test elements. Its common attributes are described into abstract class. The threads have state, actione, quantity properties. The thread has main controller. The sampler also has its controller. The hash tree has relationship with thread group. The JmeterThread construction function is defined into class body. The controller is interface type and belongs to one kind of test elements. The next method is main contract of controller interface. The thread group uses controller to find next sampler. 

The start method is the primary method of thread group class. In the start method, the new thread is created and started according to thread number requirement. The Jmeter thread is virtual thread and it is passed into native thread and executed by the thread. The main algorithm of start new thread contains creating jmether thread, scheduling the thread, creating native thread, registering the thread for management and starting the new thread.

The main method of JmeterThread is its run method which utilizes while loop to run sampler. The sampler is extracted from hash tree by traversing the hash tree. The sampler is iteracted from controller object and processed in the run method. The transaction controller is special controller. The sampler is manipulated by do sampling method. The sampling result is returned and collected. The sample method of sampler is performed to get the sample result. The sampler is controlled by the transaction controller. The result processors are executed on the sample result. The life cycle of sampling process includes preprocessor, sampling, assertion and postprocessor.

The standard engine manages the thread group and start the thread group. The thread groups are searched from the tree and started by the standard engine. 
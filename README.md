# ThreadPoolPattern
A thread pool pattern based multi-threaded program that factorizes natural numbers concurrently.
We'll develop a multi-threaded program to factorize natural numbers concurrently.
Here are some more details about the requirements of the program… the main thread reads numbers and the corresponding tags from the standard input and place these into a queue. It also receives result from each worker and writes the results to the standard output.
…We also have to take care of when the program should exit. The program exits when the end of the standard input has been reached and there is no busy thread anymore.

Here are some design considerations: 
The queue will be modified by multiple threads concurrently. Therefore, access to the queue must be mutually exclusive. In other words, we need a mutex. A thread should wait if the queue is empty. So, we need a condition variable. Let’s name it full. A thread should wait if the queue is full. So, we need another condition variable. Let’s say empty. How a thread will know that the end of the standard input has been reached? We need a flag like BUFFER_CLOSED. Also, access to the flags buffer must be mutually exclusive. The factorize function uses the Sieve of Eratosthenes method to factorize numbers correctly.

The run() function is composed of five constituent functions. 
It creates the thread pool. Takes input from the command line and places on the program queue. Then it makes the main thread to wait for the worker threads. Next it writes results to the standard output. Finally, it releases the memory acquired by the program dynamically.

The function read_numbers_from_stdin() has three parts:
1.	Reads a list of (tag, natural number) pairs from the standard input and inserts each pair in the queue.
2.	Since all the data is inserted in one go, this function also sets the BUFFER_CLOSED flag. 
3.	A wake-up call is sent to all the threads waiting on queue’s condition variable full. 

The effect of this wake-up call is two-fold. 
First, threads waiting because the queue is empty and the BUFFER_CLOSED is unset, will wake up and …….. may notice that the BUFFER_CLOSED is set and will decide to stop execution. 
Second, threads waiting because the queue is empty and the BUFFER_CLOSED is unset, will wake up and …. may notice that there is at least one item in the queue and will proceed to grab it.

Also note that BUFFER_CLOSED may be accessed by multiple threads, so modifying it is guarded with mutex.

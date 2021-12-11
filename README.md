# DistributedOS-ClocksSync-MulticastOrdering-MutualExclusion

## Environments:

All tests are on Ubuntu 16.04 LTS, g++ (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609.

# 1. Proj2 Clocks Synchronization, Multicast Ordering, and Mutual Exclusion - Requirement

In this programming project, you will develop an n-node distributed system that provides a causally ordered multicasting service and a distributed locking scheme. The distributed system uses logical clock to timestamp messages sent/received between nodes. To start the distributed system, each node should synchronize their logical clocks to the same initial value, based on which the ordering of events can be determined among the machines. For causal ordered multicasting you can use the algorithm discussed in class.

Additionally, suppose the distributed nodes have read and write access to a shared file. The last task is to implement a distributed locking scheme that prevents concurrent accesses to the shared file (this is an extra credit bonus assignment). You can use the centralized, decentralized, or the distributed algorithm to realize mutual exclusive access to the file. To simplify the design and testing, the distributed system will be emulated using multiple processes on a single machine. Each process represents a machine and has a unique port number for communication.

## 1.1 Assignment-1 (60pts) 
Suppose the logical clock on each machine represents the number of messages have been sent and received by this machine. It is actually a counter used by the process (or the machine emulator) to count events. Randomly initialize the logical clock of individual processes and use Berkeley’s algorithm to synchronize these clocks to the average clock. You can select any process as the time daemon to initiate the clock synchronization. After the synchronization, each process prints out its logical clock to check the result of synchronization.

## 1.2 Assignment-2 (40pts) 
Implement the causal ordered multicasting for the distributed system. Create two threads for each process, one for sending the multicast message to other nodes and one for listening to its communication port. Use vector clocks to enforce the order of messages. Once a process delivers a received message to a user, it prints out the message on screen. You can assume that the number of processes (machines) is fixed (equal to or larger than 3) and processes will not fail, join, or leave the distributed system. Implement two versions of this program, one without causally ordered multicasting and one with this feature. Compare the results of the two programs.

## 1.3 Bonus assignment (20pts) 
Add the feature of distributed locking to your program to protect a shared file. The file only contains a counter that can be read and updated by processes. Implement two operations: acquire and release on a lock variable to protect the file. At the beginning, each process opens the file and tries to update the counter in the file and verifies the update. Thus, the critical section includes the following operations: (1) point the file offset to the counter; (2) update the counter; (3) read and print out the counter value. You can use any of the mutual exclusion algorithm learned in class to implement the locking. The expected result is that the read of the counter value always matches the updated value by a process if locking is enabled.

# 2. Assignment 1 Clock Synchronization, Berkeley Algorithm

The Background Knowledge Review and my Implementation for this part is at dir ["berkeley-algorithm-implementation"](berkeley-algorithm-implementation).



## 3.1 Steps for assignment 1 - clock synchronization, berkeley algorithm
1. Implement at least 3 process. Each has a random clock initial value.
2. Select one as time daemon arbitrarily.
3. Time daemon broadcast to ask all other processes for their local clock values.
4. All processes answer.
5. Time daemon calculaute the average value as final clock value and tell everyone how to adjust.


# 2. Background Knowledge Review


## 2.3 Broadcast, Multicast, Unicast

The 3 types of communication forms in DS are listed below. In this project all are needed.
- Broadcast, message sent to all processes (anywhere)
- Multicast, message sent to a group of processes
- Unicast, message sent from one sender process to one receiver process

For multicast, we care about the order issue. There arre 3 types multicast ordering approaches:
- **FIFO ordering**: If a correct process issues (sends) multicast(g,m) to group g and then multicast(g,m’), then every correct  process that receives m’ would already have received m
- **Causal ordering**: If multicast(g,m) -> multicast(g,m’)  then any correct process that delivers m’would already have delivered m. (is Lamport’s happens-before)
- **Total ordering**: If a correct process P delivers message m before m’ (independent of the senders), then any other correct process P’ that receives m’ would already have received m.


## 2.4 Mutual exclusion

- Mutual exclusion: is a concurrency control property which is introduced to prevent race conditions. 
- Critical section: code only one thread/process can execute at a time 
- Lock: mechanism for mutual exclusion. Lock entering critical section, accessing shared data. Unlock when complete. Wait if locked.

For single machine, we can use shared variable (like semaphores) to share the status of shared-resources and user, because these data are all in memory or shared memory.

For DS, no shared memory are avaiable, message passing is the sole means for implementing distributed mutual exclusion [[1]](https://www.cs.uic.edu/~ajayk/Chapter9.pdf).

Three basic approaches for distributed mutual exclusion:
1. Token based approach
2. Non-token based approach (like Lamport's algorithm)
3. Quorum based approach

More algorithm introduction about above three types of approaches refer to [[2]](https://www.geeksforgeeks.org/mutual-exclusion-in-distributed-system/?ref=lbp).

## 2.5 Lamport's algorithm

The algorithm discussed in requirement of bonus assignment and in prof's slide is Lamport's algorithm.

![](img/lamport1.png)
![](img/lamport2.png)
![](img/lamport3.png)
[(These images credit to Prof. Ajay Kshemkalyani.)](https://www.cs.uic.edu/~ajayk/Chapter9.pdf)


# 3. Assignment step details 



## 3.2 Steps for assignment 2 - Multicast ordering

This part asks for implementing two of FIFO ordering, Causal ordering and Total ordering.

- For FIFO ordering and Causal ordering:
1. Implement at least 3 process. Each has 2 threads, one for sending messages while another for receiving messages.
2. Use vector clocks for clock synchronization and a vector of pre-sender sequence number data structure for message ordering. 
3. Use different updating rules for delivering and buffering, for FIFO and Causal respectively (see below figure).

![](img/fifo_update.png)
![](img/causal_update.png)

- For Total ordering:
1. Select one process as sequencer (leader) arbitrarily. Use sequencer-based approach (see below figure).

![](img/total_order_update.png)

## 3.3 Steps for assignment 3 bounus assignment - Mutual exclusion in DS



Reference:

1. [multi-process fork()](https://www.geeksforgeeks.org/creating-multiple-process-using-fork/)
# DistributedOS-ClocksSync-Multicast-COMMIT

## Environments:

All tests are on Ubuntu 16.04 LTS, g++ (Ubuntu 5.4.0-6ubuntu1~16.04.12) 5.4.0 20160609.

# Proj2 Clocks, Multicast, and COMMIT - Requirement

In this programming project, you will develop an n-node distributed system that provides a causally ordered multicasting service and a distributed locking scheme. The distributed system uses logical clock to timestamp messages sent/received between nodes. To start the distributed system, each node should synchronize their logical clocks to the same initial value, based on which the ordering of events can be determined among the machines. For causal ordered multicasting you can use the algorithm discussed in class.

Additionally, suppose the distributed nodes have read and write access to a shared file. The last task is to implement a distributed locking scheme that prevents concurrent accesses to the shared file (this is an extra credit bonus assignment). You can use the centralized, decentralized, or the distributed algorithm to realize mutual exclusive access to the file. To simplify the design and testing, the distributed system will be emulated using multiple processes on a single machine. Each process represents a machine and has a unique port number for communication.

## Assignment-1 (60pts) 
Suppose the logical clock on each machine represents the number of messages have been sent and received by this machine. It is actually a counter used by the process (or the machine emulator) to count events. Randomly initialize the logical clock of individual processes and use Berkeley’s algorithm to synchronize these clocks to the average clock. You can select any process as the time daemon to initiate the clock synchronization. After the synchronization, each process prints out its logical clock to check the result of synchronization.

## Assignment-2 (40pts) 
Implement the causal ordered multicasting for the distributed system. Create two threads for each process, one for sending the multicast message to other nodes and one for listening to its communication port. Use vector clocks to enforce the order of messages. Once a process delivers a received message to a user, it prints out the message on screen. You can assume that the number of processes (machines) is fixed (equal to or larger than 3) and processes will not fail, join, or leave the distributed system. Implement two versions of this program, one without causally ordered multicasting and one with this feature. Compare the results of the two programs.

## Bonus assignment (20pts) 
Add the feature of distributed locking to your program to protect a shared file. The file only contains a counter that can be read and updated by processes. Implement two operations: acquire and release on a lock variable to protect the file. At the beginning, each process opens the file and tries to update the counter in the file and verifies the update. Thus, the critical section includes the following operations: (1) point the file offset to the counter; (2) update the counter; (3) read and print out the counter value. You can use any of the mutual exclusion algorithm learned in class to implement the locking. The expected result is that the read of the counter value always matches the updated value by a process if locking is enabled.

# Background Knowledge Review

## Synchronization, Clock Synchronization, Berkeley Algorithm
Synchronization is the core issue for distributed systems. Synchronization in DS is archieved via clocks. 

Two popular clock synchronization ways for DS are:
- Christian's algorithm, which is absolute synchronization via connecting to UTC (Universial Time Coordinator)
- Berkeley algorithm, which is relative synchronization without central time-server (like UTC). It uses arithmetic mean value as new time for everyone, so it is actually averaging algorithm.

In DS, absolute time is less important. Clock synchronazation doesn't need to be absolute. DS care more about orders. So Berkeley Algorithm is more useful in DS than Christian's.

In assignment 1, the Berkeley is also the one which is required.

## Steps for the Berkeley algorithm (Averaging algorithm)
1. The time daemon asks all the other machines for their clock values. 
2. The machines answer.
3. The Time daemon tells everyone how to adjust their clock.

![](img/berkeley.png)
[(This image credits to UPenn.)](https://www.cis.upenn.edu/~lee/07cis505/Lec/lec-ch6-synch1-PhysicalClock-v2.pdf)
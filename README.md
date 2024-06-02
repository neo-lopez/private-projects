# Private Projects
This file contains descriptions and links to my most notable private projects. Please note that each project link will lead to a 404 error unless I have granted you access. Please reach out to me at splopez@andrew.cmu.edu and I would be happy to grant access.
- [Dynamic Memory Allocator, malloc lab](https://github.com/neo-lopez/malloc-lab)
- [Web Proxy, proxy lab](https://github.com/neo-lopez/proxy-lab)

## Dynamic Memory Allocator
### Repository Access
The full repository can be found [here](https://github.com/neo-lopez/malloc-lab). To request access, please email splopez@andrew.cmu.edu
### Overview
In this project, I reimplemented the C memory allocation functions `malloc`, `calloc`, `realloc`, `free` by using rudimentary functions to manipulate and store data on the heap.

My project ranked [first](https://autolab.andrew.cmu.edu/courses/15213-f23/assessments/malloclab/scoreboard) in over 326 unique student submissions in the combination of throughput and utilization.
### Implementation
- Memory allocation takes place on the heap due to its expandable nature. Blocks on the heap are either allocated or freed, starting with one big free block. Each block contains a footer and/or a header to hold necessary information for maintenance of the blocks amongst one another on the heap.
- Utilization, how much of the heap we use for dynamic memory as opposed to overhead in maintaining the blocks, is optimized by the following design choices:
  - Coalescing: When a block is freed and its adjacent blocks are already free, contiguous free blocks are merged together in a process called coalescing. Information about previous and next block's allocation status is stored in the current block's header and footer.
  - Splitting blocks: When an allocated block is created from a free one, we split the free block into two, one that perfectly fits the to-be-allocated block and one free block to use for future allocations.
  - Removing footers in allocated blocks: Footers are necessary in free blocks as we splice free blocks in and out of their size specific doubly linked list as part of segregated linked lists. They are not necssary for allocated blocks, so removing them increases heap utilization.
  - Mini-blocks: The minimum block size can be decreased to 16 bytes after removing footers in allocated blocks. Mini-blocks allows this decrease by maintaining a singly linked free mini block list (all blocks are 16 bytes, only contain a header as opposed to a header and a footer) in addition to the other free block doubly linked lists.
  - Better-fit algorithm: After finding a free block of adequate size for a to-be-allocated block, the algorithm briefly looks for a better fit that wont split a free block and will perfectly fit a free block.
- Performance, measured in throughput, is optimized by:
  - Segregated free lists: Segregated free lists are attainable by associating a size with each block. The segregated lists ensure we do not search through every free block in search for a fit for a to-be-allocated block. The size boundaries on each segregated free list were carefully adjusted to maximize performance.
  - Better-fit algorithm: The algorithm avoids creating split blocks that are of size <= 16 bytes, since this will eventually join the mini block list, which is scanned through slowly.
  - Micro-optimizations: The code is written in a way that prevents using more time than necessary to check conditions. In addition, it features unique time-saving algorithms such as quicker rounding for power-of-2 numbers.

## Web Proxy
### Repository Access
The full repository can be found [here](https://github.com/neo-lopez/proxy-lab). To request access, please email splopez@andrew.cmu.edu
### Overview
In this project, I created a working web proxy, which serves as a middle man between clients and servers. To the client, it acts as a server, interpreting requests and providing responses. While to the server, it acts as a client, generating requests and reading responses. Proxies are useful for security, anonimity, and speed.
### High-level Process
1. Open a socket which listens for new clients.
2. Use a new thread for each new client.
3. Read requests from client.
4. Search the cache for the requested object. If present, write this response to the client and skip steps 5-7.
5. Write the client's request to the server.
6. Read the server's response and cache it.
7. Write the response back to the client.
### Implementation Details
- Get HTTP requests are handled by safe I/O functions that allow for reading and writing both binary and text data.
- HTTP requests are used as keys to software cache the most recent requests, enabling faster speeds for objects that are requested from the server multiple times.
- Multiple requests are handled concurrently with threads, and thread-safe access and updating of the cache is enabled by mutexes.
- The web proxy employs proper error checking and will not exit the program when errors arise.

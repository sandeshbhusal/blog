---
title: "Lru Cache :: Data Structure"
date: 2018-12-29T07:27:44+05:45
showDate: true
draft: false
tags: ["blog","Data Structures"]
---

LRU caches are one of the most interesting data structures I have encountered so far. LRU refers to Least Recently Updated, which gives the name to the data structure. Caching is a mechanism that is used to make frequent access of data quick. Caching encompasses two types of accessibility: Temporal and Spatial.

If data within a small range of memory addresses is to be frequently brought into the processor, then spatial caching is used. A fixed count of memory addresses around the requested address is fetched into the cache, and stored for quick access. The implementation depends upon the processor architecture. This is also referenced as locality of reference. This can be the case in places like accessing the elements of a linear array. Such programs that exhibit locality of reference are useful for RISC implementation, as they make pipelines extremely efficient to use.

Another type of caching is Temporal caching. The data that is most frequently accessed is fetched and contained into the cache. Temporal locality is a special case of Spatial locality. There are various other types of locality, that can be explored further. Most modern processors make use of hybrid systems of locality combined with branch locality and other forms to make processing efficient.

Now back onto the topic of LRU caches. From the definition of caches, we have come to understand that caches are extremely efficient data structures that support quick lookup and insertion. The fact that makes LRU caches different from any other type of cache is that the cache is sorted according to the update timing of the data inserted. As caches are fixed width memory structures (in computers, caches are extremely expensive so each processing unit has access to very trivial amount of memory as cache compared to main memory size. For more reference, check out L1, L2 and L3 caches), they tend to become full after some time. So, insertion of a new element means the deletion of an existing one from the cache.

From this, we came to know about two things -- LRU caches have quick lookups and insertions and the least recently updated element is kicked out in the next insertion.

### Implementation:

Okay. So we need quick lookups and insertions. Preferably in O(1) time. What data structure comes to mind? Yes! Hash maps! For checking the recent updates, we can implement a double ended queue and remove elements as we go on.

So far, so easy. Now how can we demonstrate the quick nature of LRU caches? Let us take a linked list, where we have 10 elements. We need to find the values of the nodes in the linked list. Now you must be thinking, why a *linked list* and not an *array*. This is due to the fact that disk lookups for processors are linear, and sometimes hybrid, employing efficient mechanisms to make them as quick as possible. Linked lists are the best linear structures that we can implement quickly.

Consider the following scenario:

There is a list of students, with their roll numbers, names and addresses. The data is structured in a linked list. There is a program that requests with us the data of a student based on his/her roll number, and we have to give the output based on it. Let's make the length of this list 10.

For a LRU cache, we will implement a hash table of size 5 (effectively simulating the limitation compared to the main memory, i.e. Linked List). When queried with a roll number, we will first check if the cache contains the roll number. If yes, we will retrieve the student from the cache itself. Otherwise, we will perform a lookup on the Linked List.




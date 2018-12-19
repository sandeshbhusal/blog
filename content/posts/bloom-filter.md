---
title: "Bloom Filters"
date: 2018-12-19T12:14:36+05:45
showDate: true
draft: false
tags: ["Data Structures","computing"]
titleblock: true

---

Bloom Filters are a great way of satisfying negatives.

Bloom filters are probabilistic data structures, that are used to find out if a certain given element lies within a set or not. Or more precisely, to find out if a particular given element definitely DOES NOT lie within a given set. This characteristic of bloom filters is exploited in various places, primarily, when implementing web crawlers. This article is meant to be an introduction to Bloom Filters. 

Bloom Filters (we will refer to it as BF from now onwards) is a probabilistic data structure. As BFs are primarily used to find set associations for elements and it being a probabilistic data structure, you might ask yourself: Why should I use it if it is inaccurate? The answer is this: Even though BFs are probabilistic, and can give a rough estimate if any given element lies in a set, they can give DEFINITE answers when it comes to giving negative answers, i.e. they can accurately predict if the element is not within a given set. "Hold up!", you must be thinking. "For any event happening, if the probability is *p(x)*, then the probability of it not happening will be *1-p(x)* ! Thus, how can BFs give accurate measures?!" If this question has popped into your mind, you will find your answer below.

#### Structure:

Bloom filters are essentialy limited bucket hashtable with multiple hash functions. Let's analyze the structure.

Let us consider a table with indices of size 10 as follows:

|  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |  9   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  -   |  -   |  -   |  -   |  -   |  -   |  -   |  -   |  -   |  -   |
|      |      |      |      |      |      |      |      |      |      |

This will be the bucket for our hash output storage. We require at least two hash functions for the BF to work. Again, BF means Bloom Filter. Let us take any two hashing functions, and output their modulo. For an example, we can take a sum hash and a multiply hash function (in real life these functions are not used).

Let, for a string 's', the sum function is defined as:

Sum(s) = s[0] + s[1] + s[2] + ... + s[n]; n<length(s)

\sum_{i=1}^{10} t_i

where s[i] represents the ith character in the string.

Similarly, the multiplication function will be defined by:

Mul(s) = s[0] x s[1] x s[2] x ... x s[n]; n<length(s)

Now that we have assigned two of our hash functions, we can proceed to fill the bloom table. For that, we need an algorithm.

Let, s be the input string. Then, we fill up positions 

**Sum(s) MOD 10 and Mul(s) MOD 10**

in the table. 

Consider the input string as **apple** . Then, the functional outputs will be:

Sum('apple') = 53**0**

Mul('apple') = 1327250534**4**

So, the bit positions 0 and 4 in the bucket will be filled. 

|  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |  9   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  1   |  -   |  -   |  -   |  1   |  -   |  -   |  -   |  -   |  -   |
|      |      |      |      |      |      |      |      |      |      |



Next time we need to query for the existence of 'apple' inside our structure, we will re-hash it and check if the respective bits have been set or not. If the bits have been set, then the element exists in the structure, otherwise not. Further analysis on the probability problem will be done in the later section.

### Design and Analysis:

For the selection of the hash function for our BF table, we need functions that are **independent**, i.e. there should be no possibility of guessing the output of one hash function from the other and **uniformly distributed**, which means that there should be equal probability of producing the items in the bucket from the hash function, provided a distributed input is fed into it.

In most of the examples on the internet regarding BFs, you will see two prominent hashing functions being used, namely, the **fnv** family of hashes and **murmur** family of hashing. The function choice depends upon your system requirement for the BF implementation.

Now in order to answer the probabilistic question. Let us consider, as in the above example, the word 'apple' fills up bucket positions 0 and 4 in the BF table. However, there can be another combination which can result in the same hash positions being occupied. Hence, we cannot say for sure that a given word exists in the hash table of BF. However, while searching for hash of 'apple' in the BF table, if the hash bits 0 or 4, either one of them is not set, then we can say for sure that the element does not exist in the set. So, we can observe that the BF table is useful for checking if a particular element does not exist in the set; not for checking the what elements are actually present in the set. Another question by now might be, "Can the two bits arbitrarily be set by another element of the set, such that when we check for the word 'apple', it returns a positive, even though it is not present?" For further understanding this question, let us consider the case when all of the buckets are filled in the table, which will return true for the existence of any element we choose.

Thus we can observe that the bloom filter will be suitable as long as a finite amount of elements are filled in it in a finite amount of space, both of which can be posed as an optimization problems.

> For more mathematical approaches and formulae on bloom filters, check out the wikipedia article. It contains extensive information and mathematical derivation of the optimization problem, and methods to choose the size of the filter; if you're into that sort of thing. For now, basic markdown won't let me post maths formulae. The Analysis section will be complete once I figure out how to do it :P

&nbsp;

​	--- END OF POST ---

&nbsp;

#### BIBILOGRAPHY:

- [llimllib's github blog](https://llimllib.github.io/bloomfilter-tutorial/)
- [Our handy Wikipedia](https://en.wikipedia.org/wiki/Bloom_filter)

&nbsp;

#### FURTHER READING:

- [Scalable Bloom Filters](http://gsd.di.uminho.pt/members/cbm/ps/dbloom.pdf)
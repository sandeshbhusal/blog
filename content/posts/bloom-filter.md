---
title: "Bloom Filters"
date: 2018-12-19T12:14:36+05:45
showDate: true
draft: false
tags: ["Data Structures","computing"]

---

## Bloom Filters

Bloom Filters are god's way of satisfying false negatives. 

ERM.. WHAT?!

Yup. Bloom filters are probabilistic data structures, that are used to find out if a certain given element lies within a set or not. Or more precisely, to find out if a particular given element definitely DOES NOT lie within a given set. This characteristic of bloom filters is exploited in various places, including implementing web crawlers. This article is meant to be an introduction to Bloom Filters. As I am kinda slow, I implore you to try out the math yourself. With that being said, let's begin!

As mentioned earlier, Bloom Filters (we will refer to it as BF from now onwards) is a probabilistic data structure, which means that you cannot be sure that it will be a hundred percent accurate when you implement it to do stuff. As BFs are primarily used to find set associations for elements, you might ask yourself: Why should I use it if it is inaccurate? The answer is this: BFs are probabilistic, and can give a rough estimate if any given element lies in a set. But it can give DEFINITE answers when it comes to giving negative answers, i.e. it can accurately predict if the element is not within a given set. "Hold up!", you must be thinking. "For any event happening, if the probability is *p(x)*, then the probability of it not happening will be *1-p(x)* ! Thus, how can BFs give accurate measures?!" Your question will be answered shortly.

#### Structure:

Bloom filters are essentialy hashtables on steroids. Well, not really. Let's analyze the structure.

Let us consider a table with indices of size 10 as follows:

|  0   |  1   |  2   |  3   |  4   |  5   |  6   |  7   |  8   |  9   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  -   |  -   |  -   |  -   |  -   |  -   |  -   |  -   |  -   |  -   |
|      |      |      |      |      |      |      |      |      |      |

This will be the bucket for our hash output storage. We require at least two hash functions for the BF to work. Again, BF means Bloom Filter. Let us take any two hashing functions, and output their modulo. For an example, we can take a sum hash and a multiply hash function (in real life these functions are not used).

Let, for a string 's', the sum function is defined as:

Sum(s) = s[0] + s[1] + s[2] + ... + s[n]; n<length(s)

where s[i] represents the ith character in the string.

Similarly, the multiplication function will be defined by:

Mul(s) = s[0] x s[1] x s[2] x ... x s[n]; n<length(s)

Now that we have assigned two of our hash functions, we can proceed to fill the bloom table. For that, we need an algorithm.

Let, s be the input string. Then, we fill up positions 

**Sum(s) MOD 10 and Mul(s) MOD 10**

in the table. 
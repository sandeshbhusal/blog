---
title: "Implementing Dictionary using a Trie"
date: 2018-12-19T08:11:50+05:45
draft: false
showDate: true
description: "A simple blog post explaining stuff."
tags: ['Data Structures']
---

## Implementation of Tries in C++

From Wikipedia: 

**In computer science, a Trie is a type of search tree used to represent the retrieval of data (thus the name Trie). It's an ordered data structure that is based on the prefix of a string.**

Thus, basically, a trie is a search tree that contains the information about a particular string. However, a trie can contain information also about the prefix and suffix informations on the string. In this case, it is particularly useful to store large amounts of information like dictionary words, which we are going to be doing today.

 *(Note: All of the examples in this blog are implemented using C++11.)*

#### Structure:

The trie data structure is a graph based data structure, implemented as a tree. It contains nodes, that contain many children (as opposed to binary trees) and each of those children fan out to further more children below them. In this way, a trie is constructed. Let us see the structure of a single node in a trie.

``` c++ 
struct node{
    bool complete;
    node *children[26];
    node(){
        for(int i=0; i<26; i++)
            this -> children[i] = nullptr;
        this -> complete = false;
    }
};
```

>  **Exercise:** Try changing the children to an array of node objects rather than pointers and see the effect.

As seen above, a node contains a boolean to indicate, if it is indeed the end of a particular word. Even if the node is an end, it has children too. So, an important point to consider is that the final nodes indicating the end of a word might have further children. For an example, *yell* and *yellow*, where the word **yell** is the prefix of the word **yellow**. Thus, all dictionary prefixes can also be detected for a word, when we traverse to the end of it. In this case, we encounter two ends in the subchildren "l" and "w" for the trie. Let us now see an example for insertion of a single word into a trie.

```c++
node *root = new node;	// Root element of our trie
std::string s;
std::cin >> s; // Input the required string.
node *currentNode = root;
for(char ch: s){	// This is a foreach array in C++11 standard
    if(currentNode[ch - 'a'] == nullptr){
        currentNode[ch - 'a'] = new node();
    }
    currentNode = currentNode[ch - 'a'];
}
// The word was inserted. Mark the node as complete
currentNode -> complete = true;
```

Now we can clearly see why the size of the children node array is 26. It is meant to store only the lowercase letters of the english alphabet, and hence, we decrement each character that we can observe from the string from 'a', effectively giving us the array index where the child element is stored.
---
layout: post
title: "[LeetCode] LRU Cache"
date: 2016-02-12 22:20
description: Solution of Leetcode Question 146
categories: [Programming]
tags: [leetcode, LRU cache]
---

Question Link:

<a href="https://leetcode.com/problems/lru-cache/" target="_blank">
https://leetcode.com/problems/lru-cache/</a>

---

I have a C++ template implementation of LRU cache on my [university web page](http://www.cs.uml.edu/~jlu1/doc/codes/lruCache.html)

## LRU

**Least Recently Used (LRU)** is a family of caching algorithms,
which discards the least recently used items first.
This algorithm requires keeping track of when the item was used,
which is expensive if one wants to make sure the algorithm always discards the least recently used item.
General implementations of this technique require keeping "age bits" for cache-lines and track the "Least Recently Used" cache-line based on age-bits.

## My Implementation

The advantage of LRU cache is that it can get and add key/value entry in O(1) time.
There are two key data structure in my implementation:

- Hash Map, containing the key-value-pairs to get the value in O(1) time.
- Double-linked List, storing all the key/value entries to add/remove entry in O(1) time.

Once the data with key K is queried, the function get(K) is first called.
If the data of key K is in the cache, the cache just returns the data and refresh the data in the linked list.
To refresh data T in the list, we move the item of data T to the head of the list.
Otherwise, if the data T of key K is not in the cache,
we need to insert the pair into the list.
If the cache is not full, we insert into the hash map, and add the item at the head of the list.
If the cache is already full, we get the tail of the list and update it with , then move this item to the head of the list.


## Python Implementation

<div class="code-title">
<span class="code-fold" id="fold-btn-python" onclick="$use('fold-body-python', 'fold-btn-python')">[-]</span>
Python code accepted by LeetCode OJ
</div>

~~~ python
class LRUCache(object):
    def __init__(self, capacity):
        """
        :type capacity: int
        """
        # Hash map of [key, entry_index]
        self.kvp = {}
        # Initialize the entries double-link list, each entry is of format [prev, next, key, value]
        self.entries = [[-1] * 4 for _ in xrange(capacity)]
        # Head and tail index
        self.head = -1
        self.tail = -1
        # Count indicating if there are free slots
        self.capacity = capacity
        self.free_index = 0

    def detach(self, i):
        """
        detach the node i from the double link list
        :param i: node index
        """
        prev_index = self.entries[i][0]
        next_index = self.entries[i][1]
        # If detached node is head, need to update head
        if prev_index != -1:
            self.entries[prev_index][1] = next_index
        else:
            self.head = next_index
        # If detached node is tail, need to update tail
        if next_index != -1:
            self.entries[next_index][0] = prev_index
        else:
            self.tail = prev_index

    def attach(self, i):
        """
        Attach the node i to the head of the double-link list
        :param i: node index to attach
        """
        self.entries[i][0] = -1
        self.entries[i][1] = self.head
        # Add the node i at the head
        if self.head != -1:
            self.entries[self.head][0] = i
        self.head = i
        # If there is no node in the double-link list, the attached node is tail too
        if self.tail == -1:
            self.tail = i

    def get(self, key):
        """
        :param key: entry key
        :rtype: int
        """
        # Find the key
        if key in self.kvp.keys():
            i = self.kvp[key]
            # Move the node to the head
            self.detach(i)
            self.attach(i)
            return self.entries[i][3]
        else:
            return -1

    def set(self, key, value):
        """
        :type key: int
        :type value: int
        :rtype: nothing
        """
        # Case 1: key already exists, set value and move it to the head
        # Case 2: key not exists and has free slot, set the free entry and move it to the head
        # Case 3: key not exists and not free slot, update the last entry and move it to the head
        if key in self.kvp.keys():
            i = self.kvp[key]
            # Update value
            self.entries[i][3] = value
            # Move the node to the head
            self.detach(i)
            self.attach(i)
        elif self.free_index < self.capacity:
            # Get the free slot
            self.entries[self.free_index][2] = key
            self.entries[self.free_index][3] = value
            # Move to the head
            self.attach(self.free_index)
            # Update the hash map
            self.kvp[key] = self.free_index
            # Reduce the free slot by 1
            self.free_index += 1
        else:
            # Find the tail
            i = self.tail
            # Remove the key from the hash map
            k = self.entries[i][2]
            self.kvp.pop(k)
            # Update the kev and value
            self.entries[i][2] = key
            self.entries[i][3] = value
            # Move the node to the head
            self.detach(i)
            self.attach(i)
            # Update the hash map
            self.kvp[key] = i
~~~
{: #fold-body-python}

## C++ Implementation

<div class="code-title">
<span class="code-fold" id="fold-btn-cpp" onclick="$use('fold-body-cpp', 'fold-btn-cpp')">[-]</span>
C++ code accepted by LeetCode OJ
</div>

~~~ cpp
#include <iostream>
#include <vector>
#include <unordered_map>

using namespace std;

class LRUCache{
public:
    struct LRUCacheEntry
    {
    	int key;
    	int data;
    	LRUCacheEntry* prev;
    	LRUCacheEntry* next;
    };

    unordered_map<int, LRUCacheEntry*>	_mapping;
	vector<LRUCacheEntry*>		_freeEntries;
	LRUCacheEntry*			head;
	LRUCacheEntry*			tail;
	LRUCacheEntry*			entries;

    LRUCache(int capacity) {
        entries = new LRUCacheEntry[capacity];
        for (int i=0; i<capacity; i++)
			_freeEntries.push_back(entries+i);
		head = new LRUCacheEntry();
		tail = new LRUCacheEntry();
		head->prev = NULL;
		head->next = tail;
		tail->next = NULL;
		tail->prev = head;
    }

    ~LRUCache()
    {
        delete head;
		delete tail;
		delete [] entries;
    }

    int get(int key) {
        LRUCacheEntry* node = _mapping[key];
		// If the key exists
		if(node)
		{
			detach(node);
			attach(node);
			return node->data;
		}
		else return -1;
    }

    void set(int key, int value) {
        LRUCacheEntry* node = _mapping[key];
		if(node)
		{
			// refresh the link list
			detach(node);
			node->data = value;
			attach(node);
		}
		else{
			if ( _freeEntries.empty() )
			{
				// Get the last node
				node = tail->prev;
				// Remove the key from the hash map
				_mapping.erase(node->key);
				// Update the node
				node->data = value;
				node->key = key;
				// Move the node to the head
				detach(node);
				attach(node);
				// Update the hash map
				_mapping[key] = node;
			}
			else{
				// Get the free entry
				node = _freeEntries.back();
				_freeEntries.pop_back();
				// Fill the node with key and value
				node->key = key;
				node->data = value;
				// Move the node to the head
				attach(node);
				// Update the hash map
				_mapping[key] = node;
			}
		}
    }

private:
    void detach(LRUCacheEntry* node)
	{
		node->prev->next = node->next;
		node->next->prev = node->prev;
	}
	void attach(LRUCacheEntry* node)
	{
		node->next = head->next;
		node->prev = head;
		head->next = node;
		node->next->prev = node;
	}
};
~~~
{: #fold-body-cpp}
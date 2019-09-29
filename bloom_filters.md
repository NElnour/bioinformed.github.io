---
layout: post
mathjax: true
title: "Bloom Filters"
date: 2019-09-29 18:00:00 -0500
author: Nada Elnour
---

Bloom filters are data structures that speed up set querying. To do so, a bloom filter makes querying a non-deterministic decision problem that returns if the item of interest is **not** in a set. While lossy, the approach allows efficient search and queying of large sets to test membership, because the two operations are independent of the size of the set.

Bloom filters are implemented as binary arrays. When the user adds an item $$i$$ to a set, it will first pass through $$k \in \mathbb{N}$$ hash functions, $$f:i \mapsto \{0,1, \cdots , m - 1\}$$, where $$m$$ is the size of the bloom filter. That is, each of the hash functions will map the item to one of the filter's indices. For every map, the bit pointed to by the index is changed to $$1$$. Hashing of an item given the different hash functions (here simply linear transformations) creates a profile or "fingerprint" of indexing. 

Here's a simple implementation in python:

``` python
from bitarray import bitarray
import random
import math


def hash_function_generator(bloom_filter_size: int, k: int) -> list:
	hash_functions = []

	for func in range(k):
		f = random.randint(0, bloom_filter_size)
		hash_functions.append(f)

	return hash_functions


def hash_item(item: object, hash_functions, bloom_filter_size: int) -> list:
	indices = []

	h = hash(item) % bloom_filter_size
	l = len(hash_functions)

	for i in range(l):
		idx = int(h + hash_functions[i])
		indices.append(idx % bloom_filter_size)

	return indices


def add(myset: set, item: object, my_hash_functions: list, bloom_filter:
list) -> None:

	indices = hash_item(item, my_hash_functions, bloom_filter.length())

	for idx in indices:
		bloom_filter[idx] = True
		myset.add(item)


def is_item_in_set(item: object, my_hash_functions: list, bloom_filter: int) -> bool:
	indices = hash_item(item, my_hash_functions, bloom_filter.length())  # total number of expected matches
	# for item
	counter = 0  # measures number of successful matches for item

	for idx in indices:
		if bloom_filter[idx]:
			counter += 1

	return counter == len(indices)
``` 

To check if an item is in the set, rather than traversing the whole set, it suffices to check if all filter positions returned by indexing with the $$k$$ hash functions return $$1$$. Because of this criterion and hashing collisions, a bloom filter approach may return **false positives** --- it can return that an item is in the set when it isn't. A bloom filter will not however assert the false absence of an item in the set. A bloom filter's size can be paramterized in terms of the expected number of items to be contained, $$n$$, and a permissible false positive rate defined by the user:

$$ m = \frac{n \ln P}{(\ln 2)^2}$$

For such a filter, one can also calculate the number of hash functions needed to keep the false positive rate low. This is given by 

$$ k = \frac{m \ln 2}{n}$$. Putting all of the code together, here's a sample workflow:

``` python
if __name__ == "__main__":
	m = 6000000  # size of bloom filter
	n = 100  # expected number of items to be inserted
	k = math.floor((m / n) * math.log(2))
	myset = set()  # an empty set to hold data

	my_hash_functions = hash_function_generator(m, k)
	bloom = bitarray(m)  # create an m-array of bits
	bloom.setall(False)  # initialize the m-bit array to 0s

	add(myset, "apple", my_hash_functions, bloom)
	add(myset, "banana", my_hash_functions, bloom)

	print(is_item_in_set("apple", my_hash_functions, bloom))
	print(is_item_in_set("banana", my_hash_functions, bloom))
	print(is_item_in_set("orange", my_hash_functions, bloom))
 ```

``` pseudocode 
True
True
False

Process finished with exit code 0
```

To conclude, bloom filters really are powerful data structures when working with large data. Their inherent simplicity allows them to be extended for searching multidimensional sets and be implemented as objects for object-oriented programming.
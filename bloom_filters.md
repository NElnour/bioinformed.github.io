---
layout: post
mathjax: true
title: "Bloom Filters"
date: 2019-09-29 18:00:00 -0500
author: Nada Elnour
---

Bloom filters are data structures that speed up searching a set [1]. To do so, a bloom filter makes searching or **querying** a set a non-deterministic decision problem that returns if the item of interest is **not** in a set. While lossy, this approach removes the dependency of searching on the size of the set. As a result, bloom filters allow us to quickly query a large set to test if it contains the desired item.

Bloom filters are implemented as binary arrays. When we add an item $$i$$ to a set, the item will first pass through $$k \in \mathbb{N}$$ hash functions, $$f:i \mapsto \{0,1, \cdots , m - 1\}$$, where $$m$$ is the size of the bloom filter. That is, each of the hash functions will map the item to one of the filter's indices. For every map, the bit pointed to by the index is changed to $$1$$ or **True**. Given the different hash functions, hashing an item creates a profile or "fingerprint" of indexing. 

Here's a simple implementation in python using linear transformations as hash functions:

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

To check if an item is in the set, rather than traversing the whole set, it suffices to check if all filter positions returned by indexing with the $$k$$ hash functions return $$1$$. Because of this criterion and hashing collisions, a bloom filter approach may return **false positives** --- it can return that an item is in the set when it isn't. A bloom filter will not however assert the false absence of an item in the set. 

What's left is then how can we choose $$m$$ and $$k$$ so that we don't skyrocket false positive errors? A bloom filter's size can be expressed in terms of the expected number of items to be contained, $$n$$, and a permissible false positive rate that we want:

$$ m = \frac{n \ln P}{(\ln 2)^2}$$

For such a filter, we can also calculate the number of hash functions needed to keep the false positive rate low [2]. This is given by 

$$ k = \frac{m \ln 2}{n}$$. 

Putting all of the code together, here's a sample workflow:

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

To conclude, bloom filters are really powerful data structures for searching large data sets. Their inherent simplicity allows us to extend them for searching multidimensional sets [3] and implement them as objects for object-oriented programming.

## References
1. Bloom, Burton H. (1970), "Space/Time Trade-offs in Hash Coding with Allowable Errors", *Communications of the ACM*, **13** (7): 422–426, doi:[10.1145/362686.362692](https://doi.org/10.1145%2F362686.362692)
2. Blustein, James; El-Maazawi, Amal (2002), "optimal case for general Bloom filters", *[Bloom Filters — A Tutorial, Analysis, and Survey](https://www.cs.dal.ca/research/techreports/cs-2002-10)*, Dalhousie University Faculty of Computer Science, pp. 1–31
3. Almeida, Paulo; Baquero, Carlos; Preguica, Nuno; Hutchison, David (2007), "Scalable Bloom Filters", *Information Processing Letters*, **101** (6): 255–261, doi:[10.1016/j.ipl.2006.10.007](https://doi.org/10.1016%2Fj.ipl.2006.10.007)

********

---
layout: post
mathjax: true
title: "Bloom Filters"
date: 2019-09-29 14:00:00 -0500
author: Nada Elnour
---

Bloom filters are data structures that speed up set querying. To do so, a bloom filter makes querying a non-deterministic decision problem that returns if the item of interest is **not** in a set. 

Bloom filters are implemented as binary arrays. When the user adds an item $$i$$ to a set, it will first pass through $$k \in \mathbb{N}$$ hash functions, $$f:i \mapsto \{0,1, \cdots , m\}$$, where $$m$$ is the size of the bloom filter.
``` pseudocode
require 'redcarpet'
markdown = Redcarpet.new("Hello World")
puts markdown.to_html
``` 

\\( 2 + 4 \\)
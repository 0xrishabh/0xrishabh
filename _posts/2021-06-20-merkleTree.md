---
layout: post
title: Implementing Merkle Tree
date: '2021-06-20'
summary: Implementing merkle tree with audit trail generation and verification abilities in python
---

Merkle Tree is basically a tree where each node respresents a hash, And the hash of the parent Node is calculated by concatenating the hashes of it's children
generally there are only two children but theres no need of it to be that way.

###### ALGORITHM

- Create leaves(Merkle Nodes) from chunks of data. 
- divide leaves into blocks of two if total count of leaf is odd duplicate the last leaf
- Now recursively create parents(Node) from these leaves(Node) untill there is only one parent(root) remaining.

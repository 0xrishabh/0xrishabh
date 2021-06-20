---
title: Implementing Merkle Tree
date: '2021-06-19'
summary: Implementing merkle tree with audit trail generation and verification abilities in python
---


Merkle Tree is basically a tree where each node respresents a hash, And the hash of the current Node is calculated by concatenating the hashes of it's children
generally there are only two children but theres no need of it to be.

###### ALGORITHM

- Take n chunks of data divide them into block of two if n is odd duplicate the last chunk
- Create leaves(Merkle Nodes) from these chunks 
- Now recursively create parents(Node) from these leaves(Node) untill there is only one parent(root) remaining.

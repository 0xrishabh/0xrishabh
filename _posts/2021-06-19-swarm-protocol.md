---
layout: post
date: 2019-01-26 19:22
title: Understanding swarm-whitepaper
summary: Summarizing swarm whitepaper
---


## Understanding swarm-whitepaper

##### TODO 
  - Understand merkle tree
  - kadmelia connectivity

### Mission Statement


Using blockchain to build a peer to peer storage and communication that is resilient to central authority , Dos and data deniability

### Disc

*Distributed Immutable storage chunks*

This is the storage *model* of the swarm protocol, It is maximised in a way that makes nodes in the network collaborate to read and store data. 

*The canonical unit of storage in Swarm is called a chunk.*

### Forwarding, privacy and caching

Every peer forwards the message to it's neighbouring peer until the file is found or every node is reached the response is forwarded through the same route back.

As their is no difference between forawding and intiating, no node can tell who intiated the request has the privacy maintains.

The node in between response routes may cache the response for further use.

### SWARM

The protocols ensures the stability of the network, peer which recieves the data from other peer after hitting threshold either needs to pay or wait untill provides the counter bandwith back to the network.



---
title: "General Things To know"
date: 2017-12-11T11:20:26+01:00
slug: "general-things"
tags: ["javascript","mssql","adonet","rabbitmq"]
categories: ["things to know"]
---

## Javascript

### Lexical environment

a specification type used to define the association of Identifiers to specific variables and functions based upon the lexical nesting structure of ECMAScript code. 
A Lexical Environment consists of an Environment Record and a possibly null reference to an outer Lexical Environment

## RabbitMQ

### Clustering and Network Partitions

RabbitMQ clusters do not tolerate network partitions well. If you are thinking of clustering across a WAN, don't. You should use federation or the shovel instead.

### Recovering from a network partition

To recover from a network partition, first choose one partition which you trust the most. This partition will become the authority for the state of
Mnesia to use; any changes which have occurred on other partitions will be lost.

Stop all nodes in the other partitions, then start them all up again. When they rejoin the cluster they will restore state from the trusted partition.

Finally, you should also restart all the nodes in the trusted partition to clear the warning.

It may be simpler to stop the whole cluster and start it again; if so make sure that the first node you start is from the trusted partition.

### Federation

The federation plugin allows you to make exchanges and queues federated. 
A federated exchange or queue can receive messages from one or more upstreams (remote exchanges and queues on other brokers).
A federated exchange can route messages published upstream to a local queue. 
A federated queue lets a local consumer receive messages from an upstream queue.
**When using a federation in a cluster, all the nodes of the cluster should have the federation plugin enabled.**

### Shovel

A shovel behaves like a well-written client application, which connects to its source and destination, reads and writes messages, and copes with connection failures

|Federation / Shovel|Clustering|
|--------------------|-----------|
|Brokers are logically separate and may have different owners.|A cluster forms a single logical broker.|
|Brokers can run different versions of RabbitMQ and Erlang.|Nodes must run the same version of RabbitMQ, and frequently Erlang.|
|Brokers can be connected via unreliable WAN links. Communication is via AMQP (optionally secured by SSL), requiring appropriate users and permissions to be set up.|Brokers must be connected via reliable LAN links. Communication is via Erlang internode messaging, requiring a shared Erlang cookie.|
|Brokers can be connected in whatever topology you arrange. Links can be one- or two-way.|All nodes connect to all other nodes in both directions.|
|Chooses Availability and Partition Tolerance (AP) from the CAP theorem.|Chooses Consistency and Partition Tolerance (CP) from the CAP theorem.|
|Some exchanges in a broker may be federated while some may be local.|Clustering is all-or-nothing.|
|A client connecting to any broker can only see queues in that broker.|A client connecting to any node can see queues on all nodes.|

## MSSQL

### Transactions

ACID properties (atomic, consistent, isolated, and durable)
Transactions can also span multiple data resources
the transactions are coordinated by the Microsoft Distributed Transaction Coordinator (MSDTC) that resides in each system.
`Enlist=false`

|IsolationLevel|Des|
|--------------|---|
|Chaos | The pending changes from more highly isolated transactions cannot be overwritten.|
|ReadCommitted | Volatile data cannot be read during the transaction, but can be modified.|
|ReadUncommitted | Volatile data can be read and modified during the transaction.|
|RepeatableRead | Volatile data can be read but not modified during the transaction. New data can be added during the transaction.|
|Serializable | Volatile data can be read but not modified, and no new data can be added during the transaction.|
|Snapshot | Volatile data can be read. Before a transaction modifies data, it verifies if another transaction has changed the data after it was initially read. If the data has been updated, an error is raised. This allows a transaction to get to the previously committed value of the data.|
|Unspecified | A different isolation level than the one specified is being used, but the level cannot be determined. An exception is thrown if this value is set.|

> The TransactionScope class creates a transaction with a IsolationLevel of Serializable by default
> Depending on your application, you might want to consider lowering the isolation level to avoid high contention in your application.

#### Optimistic and Pessimistic Concurrency

Pessimistic concurrency involves locking rows at the data source to prevent other users from modifying data in a way that affects the current user.

#### SQL Client Async operation

`Asynchronous Processing=true` in connectionstring
> Asynchronous calls are not supported if an application also uses the Context Connection connection string keyword.

---
title: "Rabbit Mq Details"
date: 2018-02-22T09:36:45+01:00
slug: "rabbit-mq-details"
tags: ["rabbitmq"]
categories: ["rabbitmq"]
---

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
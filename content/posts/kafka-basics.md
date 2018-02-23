---
title: "Kafka Basics"
date: 2018-02-22T09:36:32+01:00
slug: "apache-kafka-basics"
tags: ["kafka"]
categories: ["kafka"]
---

## Kafka Notes

> Unfortunately, queues aren't multi-subscriber—once one process reads the data it's gone. Publish-subscribe allows you broadcast data to multiple processes, but has no way of scaling processing since every message goes to every subscriber.

> Data written to Kafka is written to disk and replicated for fault-tolerance. Kafka allows producers to wait on acknowledgement so that a write isn't considered complete until it is fully replicated and guaranteed to persist even if the server written to fails.
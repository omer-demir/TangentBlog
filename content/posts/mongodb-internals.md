---
title: "Mongodb Internals"
date: 2017-12-14T17:28:34+01:00
---

# Mongo DB Storage Engines

## WiredTiger Storage Engine

WiredTiger is the default storage engine starting in MongoDB 3.2. It is well-suited for most workloads and is recommended for new deployments. WiredTiger provides a document-level concurrency model, checkpointing, and compression, among other features. In MongoDB Enterprise, WiredTiger also supports Encryption at Rest.

WiredTiger uses MultiVersion Concurrency Control (MVCC). At the start of an operation, WiredTiger provides a point-in-time snapshot of the data to the transaction. A snapshot presents a consistent view of the in-memory data.

When writing to disk, WiredTiger writes all the data in a snapshot to disk in a consistent way across all data files. The now-durable data act as a checkpoint in the data files. The checkpoint ensures that the data files are consistent up to and including the last checkpoint; i.e. checkpoints can act as recovery points.

It uses **snappy**(from google) compressing library for compression. Snappy aims for speed not compression rate at heart

## MMAPv1 Storage Engine

MMAPv1 is the original MongoDB storage engine and is the default storage engine for MongoDB versions before 3.2. It performs well on workloads with high volumes of reads and writes, as well as in-place updates.

> MMAPv1 is not supported on big-endian architectures such as s390x. MongoDB returns an error if you set MMAPv1 as the storage engine on a big-endian system.

## In Memory Storage Engine

The In-Memory Storage Engine is available in MongoDB Enterprise. Rather than storing documents on-disk, it retains them in-memory for more predictable data latencies.
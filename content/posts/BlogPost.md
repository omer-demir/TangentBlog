---
title: "General Things To know"
date: 2017-12-11T11:20:26+01:00
slug: "general-things"
tags: ["javascript","mssql","adonet"]
categories: ["things to know"]
---

## Javascript

### Lexical environment

a specification type used to define the association of Identifiers to specific variables and functions based upon the lexical nesting structure of ECMAScript code. 
A Lexical Environment consists of an Environment Record and a possibly null reference to an outer Lexical Environment

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

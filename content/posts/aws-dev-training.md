---
title: "Aws Dev Training"
date: 2018-02-22T09:38:58+01:00
slug: "aws-dev"
tags: ["AWS"]
categories: ["AWS"]
---

# Info

EFS(Elastic file storage) - > NAS  
EBS(Elastic block storage) - > SAN  
Glacier - > Long term file storage  
Amazon Neptune - > Graph DB

## S3

S3 standart - > good for request price bad for size
S3 standart infrequent access - > good for size bad for bad request

OLTP should have <10ms and data<16KB and IO is the important metric
OLAP throughput is the important metric - use columnar storage tech to compress the same type data 

S3 name 3-63 with hyphen without dot and all lowercase
S3 object has UTF-8 key encoding

Share files for Alfafrekans musics with S3 pre-auth url
`s3 presign s3://bucketname/file`

Use alphanumeric folder or key names in S3 to store object. In order to avoid `Hot Partition` problem which is occured when you have sharded data and all request coming to one shard.

## DynamoDb

You will pay for  **Read capacity and write capacity units**
**Partition key** is must **Sort key** is optional

### Global Secondary Index

has own set Read and Write capacity
partition key can be differen

### Local Secondary Index

will use main talbe Read/Write capacity
you have to use the same partition key
5 indexes you can create max!

## Lambda

with 64 till 300
only tcp ip
max 5 min can run a single task

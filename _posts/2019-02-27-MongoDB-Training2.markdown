---
layout:     post
title:      "MongoDB training2"
subtitle:   ""
date:       2019-02-24 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    - meetup
    
---

## MongoDB training2

### mongoimport

> _id -> primary index (default)


> lookup -> index

> scan -> search through every single documents

Leverage the best use of _id for optimal performance 


all operations/queries should less than *~120 ms*
MongoDB is online DB datastore, not for data processing.



REplicaSet Cluster

* Primary (All writes, all reads, for consistency)
* Secondary
* Arbiter

When primary is down, the note who has latest heartbeat will be next primary.


MongoDB is distributed but not decentralized.

 
chunk size is 64MB



Please find the dropbox link with all materials,

https://www.dropbox.com/sh/qv2k8k81h9cr9or/AABGaTSBRJNg4tuL2SjRjS7ta?dl=0

Also PFA 

pincodes.csv
014.2_AggregationPipeline.pdf
014.1_Aggregations_Intro.pdf
013.1_Causal_Consistency.pdf
012.2_Transactions_HandsOn.pdf
012.1_Transactions_Introduction.pdf
011.2_ChangeStreams_MongoShell_HandsOn.pdf
011.1_ChangeStreams_Introduction.pdf
010.4_MongoSharding_Explain_SINGLE_SHARD_query.pdf
chapter_2_mock_data.csv
010.3_MongoSharding_ShardedCollection_Key_Ruby_Stress_Shards.txt
010.2_Mongo_sharded_cluster_enableSharding.txt
010.1_Mongo_sharded_cluster_setup_3Shards.txt
tmp_sharded_cluster.txt
009.1_MongoDB_Java_Maven.html
009.2_MongoDB_Java_Create.html
009.3_MongoDB_Java_Update.html
009.3_MongoDB_Java_Update.pdf
009.2_MongoDB_Java_Create.pdf
009.1_MongoDB_Java_Maven.pdf
tmp_notes.txt
007_mongo_replicasets_intro.txt
008_mongo_replicasets_STEPDOWN.txt
006.5_CRUD_READ.pdf
006.4_CRUD_DELETE.pdf
006.3_CRUD_UPDATE.pdf
006.2_CRUD_CREATE.pdf
006.1_CRUD_intro.pdf
005_index_explain_executionStats.txt
004_mongoimport_demo.txt
003_mongo_js_shell.txt
002_mongo_find.txt
001_mongo_install.txt

High Availability with Replication_4.pdf
High Scalability with Sharding_5.pdf
1_MongoDB Introduction_1.pdf
2_Command-line Operations and Indexes_2.pdf




sqldeveloper import data model diagram

Click File → Data Modeler → Import → Data Dictionary.


---

### END


---
layout:     post
title:      "MongoDB training1"
subtitle:   ""
date:       2019-02-26 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - training
    - Tech
    - mongoDB
    
---

## Course Description:
MongoDB is a high-performance and feature-rich NoSQL database that forms the backbone of the systems that power many different organizations

This Course starts with how to initialize the server in three different modes with various configurations. A new feature in MongoDB 3 is that you can connect to a single node using Python, set to make MongoDB even more popular with anyone working with Python.

You will then learn a range of further topics including advanced query operations, monitoring and backup using MMS, as well as some very useful administration recipes including SCRAM-SHA-1 Authentication. 

This MongoDB Course will help you handle everything from administration to automation with MongoDB more effectively than ever before.

## Duration: 4 Days

## Pre-requisites:
⬥	The participants should have knowledge on either Java or Python and DataBase Concepts

Software & Hardware requirements:
Cloud Infrastructure:

Lab Requirements:
Min 4GB RAM
Windows OS
Arch: 64-Bit OS is preferred
Access to (10 Mbps+) Hi Speed Internet.

Os: Any Windows
Every participant must have the PuTTY[1] tool to connect to the remote 22 port (SSH Access) and Web Ports of external Cloud Servers (provided by Admatic for the Hands On Training).
 
[1] https://the.earth.li/~sgtatham/putty/latest/x86/putty.exe


High Level Course Contents

Install, configure, and administer MongoDB sharded clusters and replica sets

Begin writing applications using MongoDB in Java

Initialize the server in three different modes with various configurations

Discover frameworks and products built to improve developer productivity using Mongo

Take an in-depth look at the Mongo Java driver APIs

Set up enterprise class MultiNode MongoDB and monitoring and backups of MongoDB
Building a Web Application with MongoDB as backend

Day wise coverage:
Day 1

MongoDB Introduction  - SQL/NoSQL
Datastore design considerations
Relational v/s NoSQL stores
Entities, Relationships and Database modeling
When to use Relational/NoSQL?
Categories of NoSQL stores
Examples of NoSQL stores

Data Formats
What are Data Formats?
Difference between Data Formats and Data Structures
Serializing and de-serializing data
The JSON Data Format
BSON Data Format
Advantages of BSON

MongoDB Concepts
Servers
Connections
Databases
Collections
Documents
CRUD
Indexes

Querying MongoDB
Query Expression Objects
Query Options
Cursors
Mongo Query Language
Dot Notation
Full Text Search

Data Types
Basic Data Types
Dates
Arrays
Embedded Documents
_id and ObjectIds

Using the MongoDB Shell
Tips for Using the Shell
Running Scripts with the Shell
Creating a .mongorc.js
Customizing Your Prompt
Editing Complex Variables
Inconvenient Collection Names

Creating, Updating, and Deleting Documents
Inserting Documents
Removing Documents

Updating Documents
Document Replacement
Using Update Operators
Upserts
Updating Multiple Documents
Returning Updated Documents
 
Day 2
Querying
Introduction to find
Query Criteria

Type-Specific Queries
null
Regular Expressions
Querying Arrays
Querying on Embedded Documents
$where Queries

Cursors
Limits, Skips, and Sorts
Avoiding Large Skips
Advanced Query Options
Immortal Cursors
Database Commands

Indexes
Introduction to Indexes
Creating an Index
Introduction to Compound Indexes
How MongoDB Selects an Index
Using Compound Indexes
How $-Operators Use Indexes
Indexing Objects and Arrays
Index Cardinality

Using explain()
When Not to Index
Types of Indexes
Index Administration

Advanced querying
Joins
Server-side v/s Client-side querying
Retrieving a subset of fields
Conditional operators
Aggregation
Grouping
Projections
Cursor Methods
MapReduce introduction

Installing and Starting the Server
Installing single node MongoDB
Starting a single node instance using command-line options
Single node installation of MongoDB with options from the config file
Connecting to a single node in the Mongo shell with JavaScript
Connecting to a single node using a Java client
Starting multiple instances as part of a replica set
Connecting to the replica set in the shell to query and insert data
Connecting to the replica set to query and insert data from a Java client
Connecting to the replica set to query and insert data using a Python client
Starting a simple sharded environment of two shards
Connecting to a shard in the shell and performing operations

Command-line Operations and Indexes
Introduction
Creating test data
Performing simple querying, projections, and pagination from Mongo shell
Updating and deleting data from the shell

Creating index and viewing plans of queries
Analyzing the plan
Improving the query execution time
Improvement using indexes
Improvement using covered indexes
Some caveats of index creations

Creating a background and foreground index in the shell
Creating and understanding sparse indexes
Expiring documents after a fixed interval using the TTL index
Expiring documents at a given time using the TTL index

Day 3
MongoDB Setup & Configuration
Basic configuration options
Replication
Master-Slave Replication
Adding and Removing Sources
Replica Sets
Nodes in a Replica Set
Using Slaves for Data Processing
How It Works
The Oplog
Syncing
Replication State and the Local Database
Blocking for Replication
Administration
Diagnostics
Changing the Oplog Size
Replication with Authentication

Administration Basics
Renaming a collection
Viewing collection stats
Viewing database stats
Manually padding a document
The mongostat and mongotop utilities
Getting current executing operations and killing them
Using profiler to profile operations
Setting up users in Mongo
Interprocess security in Mongo
Modifying collection behavior using the collMod command
Setting up MongoDB as a windows service

Replica set configurations

MongoDB through the JavaScript shell
Diving into the MongoDB shell
Creating and querying with indexes

Sharding
Introduction to Sharding
Autosharding in MongoDB
When to Shard
The Key to Sharding: Shard Keys
Sharding an Existing Collection
Incrementing Shard Keys Versus Random Shard Keys
How Shard Keys Affect Operations
Setting Up Sharding
Starting the Servers
Sharding Data
Production Configuration
A Robust Config
Many mongos
A Sturdy Shard
Physical Servers
Sharding Administration
config Collections Sharding Commands
Manage Sharding with Java API

Monitoring
Using the Admin Interface
serverStatus
mongostat
Third-Party Plug-Ins

Security and Authentication
Authentication Basics
How Authentication Works
Other Security Considerations

Backup and Repair
Data File Backup
mongodump and mongorestore
fsync and Lock
Slave Backups
Repair
Document-oriented data
Principles of schema design
Designing an e-commerce data model
Nuts and bolts: on databases, collections, and documents

Queries and aggregation
E-commerce queries
MongoDB’s query language
Aggregating orders
Aggregation in detail

Updates, atomic operations, and deletes
A brief tour of document updates
E-commerce updates
Atomic document processing
Nuts and bolts: MongoDB updates and deletes
Manage Aggregations with Java API

Day 4
MongoDB Java Driver
Synchronous and asynchronous Operations

Getting Started with Java Driver for MongoDB
Getting the Mongo JDBC driver
Creating your first project
Inserting a document
Querying data
Updating documents
Deleting documents
Performing operations on collections
Listing collections
Using the MongoDB Java driver version 3
Running the HelloWorld class with driver v.3
Managing collections
Inserting data into the database
Querying documents
Updating documents
Deleting documents

MongoDB CRUD Beyond the Basics
Seeing MongoDB through the Java lens
Extending the MongoDB core classes
Using the Gson API with MongoDB
Downloading the Gson API
Using Gson to map a MongoDB document
Inserting Java objects as a document
Mapping embedded documents
Custom field names in your Java classes
Mapping complex BSON types
Using indexes in your applications
Defining an index in your Java classes
Advanced Operations
Introduction
Atomic find and modify operations
Implementing atomic counters in Mongo
Implementing server-side scripts
Creating and tailing a capped collection cursors in MongoDB
Converting a normal collection to a capped collection
Storing binary data in Mongo
 Mongo GridFS
Storing large data in Mongo using GridFS
Storing data to GridFS from Java client
Implementing triggers in Mongo using oplog
Implementing full text search in Mongo

Coding bulk operations

Managing Data Persistence with MongoDB and JPA

MongoDB Java Driver API 
Connecting to Mongos from Java API
Mongos vs Mongo vs mongod Java API
ReplicaSets vs Sharding API

MongoDB UseCases:
Building a Spring Application with MongoDB as Backend
MongoDB Big Data Integration with Spark Stack

## Notes

MongoDB -> NOSQL 

Data exposion cause further degradation of performance for RDBMS. Hence we introduce NOSQL. 

RDBMS | MongoDB
------ | ------------------------------------
strict Schema - displine  | Schema free/Dynamic schema
ACID | CAP theorem
Not natively distributed | Natively distributed 
Single point of failure | No SPOF
Not scalable | Scalable
SQL | No SQL


### NOSQL category 

* Key-value store
    * key -> value
    * redis, memcache
    * caching/ lookup
    * in memery DB
* Document oriented
    * record/row 
    * atomic unit will be document
    * MongoDB, couchDB, dynomDB
    * Heavey reads than writes
* Column oriented 
    * Heavey writes than read
    * atomic unit is column
    * casandra/Hbase/Big table  
* Graph oriented 
    * For connected data
    * GiantDB
    * NeoleJ
    * Big table
* Inverted index
    * enterprise search
    * solr, elastic search
    * Traditional RDBMS are always forward search
    
    
    
    CAP theorem
    -----------------------
    * consistency | * cheap
    * Availability | * good quality
    * partition tolerance | * quick delivery
    
    We can not enjoy all those three. 
    
    consistency vs. partition
    Eventual consistency
    Tunable consistency
    
## Features
Belongs to CP model


1. mongod (daemon, mongo db server instance)    
2. mongo (CLI client to mongodb instance)
3. mongos (query & router)

mongod --datapath /data/db


partition is better to hosted in same geograpical localtion for consistency. 

High scalability with sharding 



    
---

### END


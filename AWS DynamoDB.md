# AWS DynamoDB<!-- omit in toc -->

## Contents <!-- omit in toc -->

- [1. Traditional Architecture](#1-traditional-architecture)
- [2. NoSQL databases](#2-nosql-databases)
- [3. Amazon DynamoDB](#3-amazon-dynamodb)
- [4. DynamoDB - Basics](#4-dynamodb---basics)
- [5. Primary Keys](#5-primary-keys)
- [6. Read/Write Capacity Modes](#6-readwrite-capacity-modes)
  - [6.1. R/W Capacity Modes - Provisioned](#61-rw-capacity-modes---provisioned)
    - [6.1.1. Write Capacity Units (WCU)](#611-write-capacity-units-wcu)
    - [6.1.2. Strongly Consistent Read vs. Eventually Consistent Read](#612-strongly-consistent-read-vs-eventually-consistent-read)
    - [6.1.3. Read Capacity Units (RCU)](#613-read-capacity-units-rcu)
    - [6.1.4. Partitions Internal](#614-partitions-internal)
    - [6.1.5. Throttling](#615-throttling)
  - [6.2. R/W Capacity Modes - On-Demand](#62-rw-capacity-modes---on-demand)
- [7. DynamoDB Basic APIs](#7-dynamodb-basic-apis)
  - [7.1. Writing Data](#71-writing-data)
  - [7.2. Reading Data](#72-reading-data)
  - [7.3. Reading Data (Query)](#73-reading-data-query)
  - [7.4. Reading Data (Scan)](#74-reading-data-scan)
  - [7.5. Deleting Data](#75-deleting-data)
  - [7.6. Batch Operations](#76-batch-operations)
- [8. Indexes](#8-indexes)
  - [8.1. Local Secondary Index (LSI)](#81-local-secondary-index-lsi)
  - [8.2. Global Secondary Index (GSI)](#82-global-secondary-index-gsi)
  - [8.3. Indexes and Throttling](#83-indexes-and-throttling)
- [9. PartiQL](#9-partiql)
- [10. DynamoDB - Optimistic Locking](#10-dynamodb---optimistic-locking)
- [11. Accelerator - DAX](#11-accelerator---dax)
- [12. DynamoDB Streams](#12-dynamodb-streams)
  - [12.1. DynamoDB Streams and AWS Lambda](#121-dynamodb-streams-and-aws-lambda)
- [13. Time To Live (TTL)](#13-time-to-live-ttl)
- [14. DynamoDB CLI – Good to Know](#14-dynamodb-cli--good-to-know)
- [15. Global Tables](#15-global-tables)

# 1. Traditional Architecture

- Traditional applications leverage RDBMS databases.
- These databases have the SQL query language.
- Strong requirements about how the data should be modeled.
- Ability to do query joins, aggregations, complex computations.
- Vertical scaling (getting a more powerful CPU / RAM / IO).
- Horizontal scaling (increasing reading capability by adding EC2 / RDS Read Replicas).

# 2. NoSQL databases

- NoSQL databases are non-relational databases and are distributed.
- NoSQL databases include MongoDB, DynamoDB, ...
- NoSQL databases do not support query joins (or just limited support).
- All the data that is needed for a query is present in one row.
- NoSQL databases don't perform aggregations such as "SUM", "AVG", ...
- **NoSQL databases scale horizontally.**
- There's no "right or wrong" for NoSQL vs SQL, they just require to model the data differently and think about user queries differently

# 3. Amazon DynamoDB

- **DynamoDB is a fast and flexible non-relational database service for any scale. It can scale with no downtime, it can process millions of requests per second, and is fast and consistent in performance.**
- Fully managed, highly available with replication across multiple AZs.
- **NoSQL database - not a relational database.**
- Scales to massive workloads, distributed **serverless** database.
- Millions of requests per seconds, trillions of row, 100s of TB of storage.
- Fast and consistent in performance (low latency on retrieval).
- Integrated with IAM for security, authorization and administration.
- Enables event driven programming with DynamoDB Streams.
- Low cost and auto-scaling capabilities.
- Standard and Infrequent Access (IA) Table Class.

# 4. DynamoDB - Basics

- DynamoDB is made of **Tables**.
- Each table has a **Primary Key** (must be decided at creation time).
- Each table can have an infinite number of items (= rows).
- Each item has **attributes** (can be added over time - can be null).
- Maximum size of an item is **400KB**.
- Data types supported are:
  - **Scalar Types:** String, Number, Binary, Boolean, Null.
  - **Document Types:** List, Map.
  - **Set Types:** String Set, Number Set, Binary Set.

# 5. Primary Keys

- **Option 1: Partition Key (HASH)**
  - Partition key must be unique for each item.
  - Partition key must be "diverse" so that the data is distributed.
  - Example: "User_ID" for a users table.
- **Option 2: Partition Key + Sort Key (HASH + RANGE)**
  - The combination must be unique for each item.
  - Data is grouped by partition key.
  - Example: users-games table, "User_ID" for Partition Key and "Game_ID" for Sort Key.
- What is the best Partition Key to maximize data distribution?
  - movie_id
  - producer_name
  - leader_actor_name
  - movie_language
- "movie_id" has the highest cardinality so it's a good candidate.
- "movie_language" doesn't take many values and may be skewed towards English so it's not a great choice for the Partition Key.

# 6. Read/Write Capacity Modes

- Control how you manage your table's capacity (read/write throughput).
- **Provisioned Mode (default):**
  - You specify the number of reads/writes per second.
  - You need to plan capacity beforehand.
  - Pay for provisioned read and write capacity units.
- **On-Demand Mode:**
  - Read/writes automatically scale up/down with your workloads.
  - No capacity planning needed.
  - Pay for what you use, more expensive ($$$).
- You can switch between different modes once every 24 hours.

## 6.1. R/W Capacity Modes - Provisioned

- Table must have provisioned read and write capacity units.
- **Read Capacity Units (RCU)** - throughput for reads.
- **Write Capacity Units (WCU)** - throughput for writes.
- Option to setup auto-scaling of throughput to meet demand.
- Throughput can be exceeded temporarily using **"Burst Capacity"**.
- If Burst Capacity has been consumed, you'll get a **"ProvisionedThroughputExceededException"**.
- It's then advised to do an **exponential backoff** retry.

### 6.1.1. Write Capacity Units (WCU)

- One Write Capacity Unit (WCU) represents one write per second for an item up to 1 KB in size.
- If the items are larger than 1 KB, more WCUs are consumed.

### 6.1.2. Strongly Consistent Read vs. Eventually Consistent Read

- **Eventually Consistent Read (default)**
  - If we read just after a write, it's possible we'll get some stale data because of replication.
- **Strongly Consistent Read**
  - If we read just after a write, we will get the correct data.
  - Set "ConsistentRead" parameter to True in API calls (GetItem, BatchGetItem, Query, Scan).
  - Consumes twice the RCU.

### 6.1.3. Read Capacity Units (RCU)

- One Read Capacity Unit (RCU) represents:
  - **One (1) Strongly Consistent Read** per second
  - **Two (2) Eventually Consistent Reads** per second
- For an item up to 4 **KB** in size.
- If the items are larger than 4 KB, more RCUs are consumed.

### 6.1.4. Partitions Internal

- Data is stored in partitions.
- Partition Keys go through a hashing algorithm to know to which partition they go to.

### 6.1.5. Throttling

- If we exceed provisioned RCUs or WCUs, we get **"ProvisionedThroughputExceededException"**.
- Reasons:
  - **Hot Keys** - one partition key is being read too many times (e.g., popular item).
  - **Hot Partitions.**
  - **Very large items**, RCU and WCU depends on size of items.
- Solutions:
  - **Exponential backoff** when exception is encountered (already in SDK).
  - **Distribute partition keys** as much as possible.
  - If RCU issue, we can **use DynamoDB Accelerator (DAX)**.

## 6.2. R/W Capacity Modes - On-Demand

- Read/writes automatically scale up/down with your workloads.
- No capacity planning needed (WCU / RCU).
- Unlimited WCU and RCU, no throttle, more expensive.
- You're charged for reads/writes that you use in terms of **RRU** and **WRU**.
- **Read Request Units (RRU)** - throughput for reads (same as RCU).
- **Write Request Units (WRU)** - throughput for writes (same as WCU).
- 2.5x more expensive than provisioned capacity (use with care).
- Use cases: unknown workloads, unpredictable application traffic, ...

# 7. DynamoDB Basic APIs

## 7.1. Writing Data

- **PutItem:**
  - Creates a new item or fully replace an old item (same Primary Key).
  - Consumes WCUs.
- **UpdateItem:**
  - Edits an existing item's attributes or adds a new item if it doesn't exist.
  - Can be used to implement Atomic Counters - a numeric attribute that's unconditionally incremented.
- **Conditional Writes:**
  - Accept a write/update/delete only if conditions are met, otherwise returns an error.
  - Helps with concurrent access to items.
  - No performance impact.

## 7.2. Reading Data

- **GetItem:**
  - Read based on Primary key.
  - Primary Key can be HASH or HASH+RANGE.
  - Eventually Consistent Read (default).
  - Option to use Strongly Consistent Reads (more RCU - might take longer).
  - **ProjectionExpression** can be specified to retrieve only certain attributes.

## 7.3. Reading Data (Query)

- **Query** returns items based on:
  - **KeyConditionExpression:**
    - Partition Key value (must be = operator) - required.
    - Sort Key value (=, <, <=, >, >=, Between, Begins with) - optional.
  - **FilterExpression:**
    - Additional filtering after the Query operation (before data returned to you).
    - Use only with non-key attributes (does not allow HASH or RANGE attributes).
- Returns:
  - The number of items specified in **Limit**.
  - Or up to 1 MB of data.
- Ability to do pagination on the results.
- Can query table, a Local Secondary Index, or a Global Secondary Index.

## 7.4. Reading Data (Scan)

- **Scan** the entire table and then filter out data (inefficient).
- Returns up to 1 MB of data - use pagination to keep on reading.
- Consumes a lot of RCU.
- Limit impact using **Limit** or reduce the size of the result and pause.
- For faster performance, use **Parallel Scan**.
  - Multiple workers scan multiple data segments at the same time.
  - Increases the throughput and RCU consumed.
  - Limit the impact of parallel scans just like you would for Scans.
- Can use **ProjectionExpression and FilterExpression** (no changes to RCU).

## 7.5. Deleting Data

- **DeleteItem:**
  - Delete an individual item.
  - Ability to perform a conditional delete.
- **DeleteTable:**
  - Delete a whole table and all its items.
  - Much quicker deletion than calling DeleteItem on all items.

## 7.6. Batch Operations

- Allows you to save in latency by reducing the number of API calls.
- Operations are done in parallel for better efficiency.
- Part of a batch can fail; in which case we need to try again for the failed items.
- **BatchWriteItem**
  - Up to 25 **PutItem** and/or **DeleteItem** in one call.
  - Up to 16 MB of data written, up to 400 KB of data per item.
  - Can't update items (use **UpdateItem**).
- **BatchGetItem**
  - Return items from one or more tables.
  - Up to 100 items, up to 16 MB of data.
  - Items are retrieved in parallel to minimize latency.

# 8. Indexes

## 8.1. Local Secondary Index (LSI)

- **Alternative Sort Key** for your table (same **Partition Key** as that of base table).
- The Sort Key consists of one scalar attribute (String, Number, or Binary).
- Up to 5 Local Secondary Indexes per table.
- **Must be defined at table creation time.**
- **Attribute Projections** - can contain some or all the attributes of the base table **(KEYS_ONLY, INCLUDE, ALL)**.

## 8.2. Global Secondary Index (GSI)

- **Alternative Primary Key (HASH or HASH+RANGE)** from the base table.
- Speed up queries on non-key attributes.
- The Index Key consists of scalar attributes (String, Number, or Binary).
- **Attribute Projections** - some or all the attributes of the base table **(KEYS_ONLY, INCLUDE, ALL)**.
- Must provision RCUs and WCUs for the index.
- **Can be added/modified after table creation.**

## 8.3. Indexes and Throttling

- Global Secondary Index (GSI):
  - **If the writes are throttled on the GSI, then the main table will be throttled!**
  - Even if the WCU on the main tables are fine.
  - Choose your GSI partition key carefully!
  - Assign your WCU capacity carefully!
- Local Secondary Index (LSI):
  - Uses the WCUs and RCUs of the main table.
  - No special throttling considerations.

# 9. PartiQL

- Use a SQL-like syntax to manipulate DynamoDB tables.
- Supports some (but not all) statements:
  - INSERT
  - UPDATE
  - SELECT
  - DELETE
- It supports Batch operations.

# 10. DynamoDB - Optimistic Locking

- DynamoDB has a feature called "Conditional Writes".
- A strategy to ensure an item hasn't changed before you update/delete it.
- Each item has an attribute that acts as a version number.

# 11. Accelerator - DAX

- Fully-managed, highly available, seamless in-memory cache for DynamoDB.
- Microseconds latency for cached reads and queries.
- Doesn't require application logic modification (compatible with existing DynamoDB APIs).
- Solves the "Hot Key" problem (too many reads).
- 5 minutes TTL for cache (default).
- Up to 10 nodes in the cluster.
- Multi-AZ (3 nodes minimum recommended for production).
- Secure (Encryption at rest with KMS, VPC, IAM, CloudTrail, ...)

# 12. DynamoDB Streams

- Ordered stream of item-level modifications (create/update/delete) in a table.
- Stream records can be:
  - Sent to **Kinesis Data Streams**.
  - Read by **AWS Lambda**.
  - Read by **Kinesis Client Library applications**.
- Data Retention for up to 24 hours.
- Use cases:

  - react to changes in real-time (welcome email to users).
  - Analytics.
  - Insert into derivative tables.
  - Insert into ElasticSearch.
  - Implement cross-region replication.

- Ability to choose the information that will be written to the stream:
  - **KEYS_ONLY** - only the key attributes of the modified item.
  - **NEW_IMAGE** - the entire item, as it appears after it was modified.
  - **OLD_IMAGE** - the entire item, as it appeared before it was modified.
  - **NEW_AND_OLD_IMAGES** - both the new and the old images of the item.
- DynamoDB Streams are made of shards, just like Kinesis Data Streams.
- You don't provision shards, this is automated by AWS.
- **Records are not retroactively populated in a stream after enabling it.**

## 12.1. DynamoDB Streams and AWS Lambda

- You need to define an **Event Source Mapping** to read from a DynamoDB Streams.
- You need to ensure the Lambda function has the appropriate permissions.
- **Your Lambda function is invoked synchronously.**

  [AWS Lambda](AWS%20Lambda.md)

# 13. Time To Live (TTL)

- Automatically delete items after an expiry timestamp.
- Doesn't consume any WCUs (i.e., no extra cost).
- The TTL attribute must be a "Number" data type with "Unix Epoch timestamp" value.
- Expired items deleted within 48 hours of expiration.
- Expired items, that haven't been deleted, appears in reads/queries/scans (if you don't want them, filter them out).
- Expired items are deleted from both LSIs and GSIs.
- A delete operation for each expired item enters the DynamoDB Streams (can help recover expired items).
- Use cases: reduce stored data by keeping only current items, adhere to regulatory obligations, ...

# 14. DynamoDB CLI – Good to Know

- **--projection-expression:** one or more attributes to retrieve.
- **--filter-expression:** filter items before returned to you.
- General AWS CLI Pagination options (e.g., DynamoDB, S3, ...):
  - **--page-size:** specify that AWS CLI retrieves the full list of items but with a larger number of API calls instead of one API call (default: 1000 items).
  - **--max-items:** max. number of items to show in the CLI (returns NextToken).
  - **--starting-token:** specify the last NextToken to retrieve the next set of items.

# 15. Global Tables

- Make a DynamoDB table accessible with low latency in multiple-regions.
- Active-Active replication (read/write to any AWS Region).

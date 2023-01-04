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
- [8. Accelerator - DAX](#8-accelerator---dax)
- [9. Global Tables](#9-global-tables)

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
- Standard & Infrequent Access (IA) Table Class.

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
  - Pay for provisioned read & write capacity units.
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
- Unlimited WCU & RCU, no throttle, more expensive.
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
- Can use **ProjectionExpression & FilterExpression** (no changes to RCU).

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

# 8. Accelerator - DAX

- **Amazon DynamoDB Accelerator (DAX) is a fully managed, highly available, in-memory cache for Amazon DynamoDB that delivers up to a 10 times performance improvement—from milliseconds to microseconds—even at millions of requests per second.**
- Fully Managed **in-memory** cache for DynamoDB.
- **10x performance improvement** - single- digit millisecond latency to microseconds latency - when accessing your DynamoDB tables.
- Secure, highly scalable & highly available.
- Difference with ElastiCache at the CCP level: DAX is only used for and is integrated with DynamoDB, while ElastiCache can be used for other databases.

# 9. Global Tables

- Make a DynamoDB table accessible with low latency in multiple-regions.
- Active-Active replication (read/write to any AWS Region).

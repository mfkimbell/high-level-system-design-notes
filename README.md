# high-level-system-design-notes

### PostgreSQL
* `psycopg2` is what you can use to connect with python
* `sqlalchemy` uses psycopg2 under the hood

Locally:
`brew services start postgresql@14`
`psql postgres`

```sql
\dt               -- List all tables
\du               -- List all users (roles)
\conninfo         -- Show current database connection info
\c dbname         -- Connect to a specific database
\dn               -- List all schemas
\l                -- List all databases
\dt schema.*      -- List tables in a specific schema
\q                -- Quit psql
SELECT current_user;    -- Show current user
SELECT version();       -- Show PostgreSQL version
\d tablename      -- Describe table structure
\d+ tablename     -- Describe table with additional details
\password username -- Change password for a user
```
create db

```sql
CREATE DATABASE testdb;
\c testdb;
```
create table

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
insert user data

```sql
INSERT INTO users (username, email)
VALUES 
('alice', 'alice@example.com'),
('bob', 'bob@example.com');
```


### REST API vs NON REST

* REST APIs are **STATELESS**, idempotent
Definition: Every request from the client must contain all the information the server needs to process it.
The server does not store any information about the client between requests.
Implement a Go-based API app utilizing DynamoDB storage.
* uses standard CRUD operations mapped to HTTP methods:
  
<img width="541" alt="Screenshot 2025-02-20 at 6 25 40 PM" src="https://github.com/user-attachments/assets/a4ff1dac-c1d3-496b-b172-94fce443d92b" />

Resource based URLs

<img width="539" alt="Screenshot 2025-02-20 at 6 27 56 PM" src="https://github.com/user-attachments/assets/8842352b-4a42-494f-8554-6c0aa3620c14" />
<img width="532" alt="Screenshot 2025-02-20 at 6 28 40 PM" src="https://github.com/user-attachments/assets/4719c301-4ee7-42ab-8a25-416e9209c30c" />

Client-Server separation

<img width="532" alt="Screenshot 2025-02-20 at 6 29 27 PM" src="https://github.com/user-attachments/assets/cdfeaaba-1fff-4fe7-80c2-ec372b5bfb1e" />

#### What's a non-rest api? GRAPHQL
* not REST because it breaks the uniform interface rule (no distinct endpoints for each resource).

### Load Balancer v.s Reverse Proxy

Your confusion is reasonable - they are often the same thing. But not always. When you refer to a load balancer you are referring to a very specific thing - a server or device that balances inbound requests across two or more web servers to spread the load. A reverse proxy, however, typically has any number of features:

* load balancing: as discussed above

* caching: it can cache content from the web server(s) behind it and thereby reduce the load on the web server(s) and return some static content back to the requester without having to get the data from the web server(s)

* security: it can protect the web server(s) by preventing direct access from the internet; it might do this through simple means by just obfuscating the web server(s) or it may have some more active components that actually review inbound requests looking for malicious code

* SSL acceleration: when SSL is used; it may serve as a termination point for those SSL sessions so that the workload of dealing with the encryption is offloaded from the web server(s)

### Notes
- **Package Managers**: NuGet (C#), PDM/Poetry (Python)  
- **ORM**: Entity Framework Core (C#), SQLAlchemy (Python)  
- **Migrations**: Entity Framework Core (C#), Alembic (Python)  


Good source for drawing diagrams:
* https://excalidraw.com/



Courses I've found:

Crowdsourced a TON of good information:
* https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#study-guide
* free

NeetCode.io
* https://neetcode.io/practice
* courses about system design, beginner and intermediate
* might end up doing the ObjectOriented course too for practice
* Has some good practice problems in his "intermediate" course
* $120 bucks for a year

HelloInterview: 
* https://www.hellointerview.com/learn/system-design/in-a-hurry/how-to-prepare
* has some notes and more importantly, practice questions
* 30/month 40/year
  
DesignGuru:
* https://www.designgurus.io/path/system-design-interview-playbook?pathSlug=System-Design-Interview-Playbook
* bunch of organized notes for system design
* $16/month

## Read some real life examples of architechture
* https://www.hiredintech.com/system-design/scalability/examples/
* https://github.com/donnemartin/system-design-primer/blob/master/README.md#real-world-architectures
* https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#company-architectures

## System Design Practice
If you can’t find a friend/coworker, go to a site like HighScalability that contains tons of examples of real-life architectures and compare what you conjured up to what these companies actually did. You’d be surprised how many of the top websites’ architectures are widely available. Keep in mind that these systems were designed by multiple people over multiple months, so don’t expect to be able to come up with every low-level decision they made. Focus on the high-level stuff.
* https://www.hiredintech.com/system-design/the-twitter-problem/
* HelloInterview has practice problems, could be worth the 30 bucks
* https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#system-design-interview-questions-with-solutions
* https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#additional-system-design-interview-questions

## Object Oriented Design Practice
* https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#object-oriented-design-interview-questions-with-solutions

##  Back of the envelop calculations
* there are about 86k seconds in a day, but you can round to 100k for approximate calculations
* KB = 1000 bytes, MB = 1,000,000, GB = 1billion bytes, TB = 1trillion bytes, PB = 1*10^15 bytes

## Mock interviews
possibly a free service
* https://www.tryexponent.com/practice?ref=pramp&utm_source=pramp&utm_campaign=pramp_banner
hello interview: $170
* https://www.hellointerview.com/learn/system-design/in-a-hurry/how-to-prepare

# Step 1: establish use-cases and Functional Requirements

* Who is going to use it?
* How are they going to use it?
* How many users are there?
* What does the system do?
* What are the inputs and outputs of the system?
* How much data do we expect to handle?
* How many requests per second do we expect?
* What is the expected read to write ratio?

# Step 2: Establish Constraints and  Non-Functional Requirements
* Scalability Constraints: The system should handle millions of requests per day.
* Latency Constraints: The system must redirect users in under 200 milliseconds.
* Storage Constraints: The database must support terabytes of data.
* Cost Constraints: The solution should be cost-effective to maintain.
* Security Constraints: Must prevent malicious URLs and protect user data.

## API Keys vs JWT
* JWT are for frontend authenticaiotn, API is for backend/automation or server-to-server authentication
* JWTs expire by default, API keys are used for peristent connection
  

## SQL vs NOSQL

* SQL = relational        can do joins in one query in the database
* NoSQL = non-relational  needs to do multiple queries sometimes to get data

**Why is NoSQL more scalable?**
* They do sharding (horizontal paritioning) "out of the box" by choosing a "shard key" with consistent (circular) hashing AUTOMATICALLY (this is manual for relational databases)
* The data is denormalized so you don't have to do joins (but this makes it so you have to handle it in the app logic if you need it) and you don't have to worry about schema syncronization.
* It uses BASE instead of ACID (eventually consistency)

**SQL Join**:
```sql
SELECT orders.id, orders.date, customers.name, customers.email 
FROM orders 
JOIN customers ON orders.customer_id = customers.id;
```

**Application-Level Join in a NoSQL Database**:
1. Query the `orders` collection:
   ```javascript
   const orders = db.orders.find({}); // Get all orders
   ```

2. Use the `customer_id` from each order to query the `customers` collection:
   ```javascript
   for (let order of orders) {
     const customer = db.customers.findOne({ id: order.customer_id });
     order.customer = customer; // Merge customer data into the order
   }
   ```

* Trade-offs in NoSQL: NoSQL databases prioritize scalability, availability, and flexibility, often at the cost of certain relational capabilities like joins.
* This is why denormalization (storing related data together) is often recommended to avoid needing joins altogether.

### Why This Change Matters:
- **Performance**: SQL databases are optimized for joins and can handle them efficiently when properly indexed. Moving joins to the application code can introduce performance bottlenecks, especially with large data sets or multiple queries.
- **Complexity**: Doing joins in code means more manual work and increased code complexity. It requires extra handling for combining datasets, error checking, and ensuring performance does not degrade.
- **Trade-offs in NoSQL**: NoSQL databases prioritize scalability, availability, and flexibility, often at the cost of certain relational capabilities like joins. This is why denormalization (storing related data together) is often recommended to avoid needing joins altogether.

In summary, SQL databases simplify querying related data with native joins, while in a NoSQL or denormalized SQL setup, you have to handle joins manually in your application code. This can impact performance and code complexity but is sometimes necessary for scaling horizontally and simplifying database structure for distributed systems.

SQL/ACID systems
* **ACID** (atomicity, consistency, isolation, and durability)
##### **It becomes difficult to SCALE HORIZONTALLY for Relational databases because of maintaining ACID properties**
* ACID systems are called **Transactional Systems**
**Transaction** = { begin, call, call, call, call, ..., end}

* Atomicity - Each transaction is all or nothing
* Consistency - Any transaction will bring the database from one valid state to another
* Isolation - Executing transactions concurrently has the same results as if the transactions were executed serially
* Durability - Once a transaction has been committed, it will remain so

* Atomicity: Transactions are treated as a single, indivisible unit. Either all operations within the transaction succeed, or none of them do.
Example: Transferring money between bank accounts. If the withdrawal from one account succeeds but the deposit into another fails, the entire transaction is rolled back to maintain consistency.

* Consistency: Transactions must maintain the consistency of the database. Data should meet all rules and constraints before and after the transaction.
Example: In a reservation system, booking a seat only if it's available ensures that no two customers book the same seat simultaneously.

* Isolation: Transactions are isolated from each other to prevent interference. Concurrent transactions should not impact each other's outcomes.
* If we remove 500 from Bob's account, if bob checks his bank account right after that starts, it wouldn't change (dirty read), we do them in SEQUENCE to prevent this
Example: Simultaneous bank transfers from the same account should not interfere with each other, ensuring each transaction sees a consistent view of the data.
EXAMPLE: User A is transferring $100 from Account A to Account B, and at the same time, User B is querying Account A's balance. Isolation ensures that User B’s query does not see the intermediate state (e.g., where $100 has been debited from Account A but not yet credited to Account B).

* Durability: Committed transactions are permanently saved and survive system failures. Once a transaction is committed, its changes remain even after a power outage or crash.
Example: After confirming a purchase online, ensuring that the transaction is saved and the goods are allocated, even if there's a temporary network failure.
(As opposed to RAM)

## NoSQL
* Follows **BaSE** (eventually consistency) instead of ACID 
* To scale, we can divide our data and store them on different databases
* ???
#### Key Value Store (hash table)
* key value pairs like redis and memcached technically qualify
* Since they offer only a limited set of operations, complexity is shifted to the application layer if additional operations are needed.
#### Document store
* Ex: MongoDB, DynamoDB
* key-value store with documents stored as values
* Json instead of a bare value
* **Table = Collection, Row = Document**
* key value store that can be nested
* good for **occasionally changing data** so you don't have to continue to change database columns to amtchdata
* 
#### Wide Column
<img width="953" alt="Screenshot 2024-11-08 at 1 11 09 PM" src="https://github.com/user-attachments/assets/2d850649-2039-45b2-95b5-df961eef1ead">

* each item (identified by a row key) can indeed have data that spans multiple column families
* Wide-column databases like Cassandra are specifically designed for write-heavy workloads. They use a log-structured merge-tree (LSM tree) storage model, which allows for fast, sequential writes to disk. 
* Ex: Cassandra, Google Big Table
* Optimal for writes
* Good for low read/update requirements
* Wide column stores often use an append-only model where new data is written sequentially to disk without altering existing data structures. This makes write operations extremely efficient and minimizes disk I/O.
#### Graph Databases
* Ex: Neo4j
* all about graph based relations, IE Facebook

### Considerations:
#### Choose SQL:
* For transactional systems,
* secure and consistent data needs,
* complex queries
* strong data integrity requirements.

#### Choose NoSQL:
* For high scalability,
* flexibility in data modeling,
* handling big data, high availability,
* agile development with evolving data structures.
* storing many TB (PB) of data
#### Example of NoSQL use cases:
* Rapid ingest of clickstream and log data (noSQL are optimized for high write throughput)
* Leaderboard or scoring data
* Temporary data, such as a shopping cart (lots of writing, data constantly changes, good for session based data that is add and removed quickly)
* (SQL IS BETTER FOR LONG TERM LOW WRITES DATA)
* Frequently accessed ('hot') tables (WHEN WE USE CACHING OR EVENTUAL CONSISTENCY)
* Metadata/lookup tables

##### **SQL databases can use eventual consistency**
* but it's less common, and breaks ACID
* for instanc you can use read replicaas that might have stale data for SQL
* Using **event-driven architectures or CQRS** (Command Query Responsibility Segregation), SQL databases can be part of a system that supports eventual consistency. In this pattern, the system separates read and write operations, and updates are propagated to read models asynchronously.

## Scaling Relational Databases (SQL)
### Database Replication
#### Leader -> Follower Replication
* Followers can also act as leaders so they don't all need to directly talk to the leader, they can do a chain reaction
* Great for **Scaling Reads**
#### Leader <-> Leader Replication
* Great for **Scaling Writes**
* ex: Lets say we have a leader in each country where languages differ, the data MIGHT not be relevant for people in the other country, so the consistency might not matter as much
* suffers with **Consistency** when async
* suffers with **Latency** when syncronouse

### Leader-Follower replication
* One Leader node handles all the writes
* There are many followers than can be used for reads
#### Disadvantages
* Complexity
* Can't scale writes as well

### Leader-Leader Replication
* Multiple leaders can accept read and writes, they coordinate with each other
#### Disadvantages
* You'll need a load balancer or you'll need to make changes to your application logic to determine where to write
* Most master-master systems are either loosely consistent (violating ACID) or have increased write latency due to synchronization.
* Conflict resolution comes more into play as more write nodes are added and as latency increases.

### Replication Disadvantages for Leader-Leader and Leader-Follower
* There is a potential for loss of data if the leader fails before any newly written data can be replicated to other nodes.
* Writes are replayed to the read replicas. If there are a lot of writes, the read replicas can get bogged down with replaying writes and can't do as many reads.

### Federation (functional partioning) (SQL)
* (or functional partitioning) splits up databases by function
* For example, instead of a single, monolithic database, you could have three databases: forums, users, and products, resulting in less read and write traffic to each database and therefore less replication lag.
* Joining data from two databases is more complex with a server link.
Federation adds more hardware and additional complexity.

### Database Sharding (SQL)
* allows for less replication
* allows for multiple sources for writing, and no latency for syncronization)
* smaller database = faster queries

DynamoDB and similar NoSQL databases (like Cassandra or MongoDB) handle sharding automatically based on partition keys, ideal for distributed architectures.

**Range Based Sharding**
* A-L goes to one databse
* K-Z goes to the other database

**Hash Based Sharding**
* spreads evenly
* cosistent hashing
* Usually just chooses a primary key

##### NoSQL Generally does this by Default. Relational databases CAN but not by default. 

When to Use Sharding:

* High Data Volume: When data size exceeds the storage capacity or performance limits of a single database.
* High Traffic: When a single database cannot handle the number of read/write operations efficiently.
* Scalability Needs: When horizontal scaling is required for future growth.
* Geographic Distribution: When data needs to be distributed across regions for faster local access.
  
When Not to Use Sharding:

* Low to Moderate Data Size: When a single database can handle the data volume and traffic efficiently.
* Complexity Concerns: When adding complexity would outweigh the benefits (e.g., small teams or limited expertise).
* Cost Sensitivity: When the added infrastructure and management costs are prohibitive.
* Strong Consistency Requirements: When cross-shard consistency is crucial and difficult to maintain.
* Cross-shard Queries: When frequent multi-shard queries could degrade performance or complicate application logic.

Summary of Trade-offs (sharding vs replication):
It's important to note however, many times they are BOTH used at the same time, but leads to additional complexity
* Sharding:
* Pros: Scalability, reduced load per node, efficient caching.
* Cons: Complex data management, rebalancing issues, potential hotspots.
* Replication:
* Pros: High availability, improved read performance, better cache hits.
* Cons: Increased storage and cost, write performance impact, consistency challenges.

### Denormalization (SQL)
* Denormalization attempts to improve read performance at the expense of some write performance.
* Redundant copies of the data are written in multiple tables to avoid expensive joins.
* **Imporant when there are very complex join reads** and they outweigh the writes
#### Disadvantages
* Data is duplicated.
* Redundant copies of information MUST stay in sync (unless it doesn't matter and can be eventual), which increases complexity of the database design.
* A denormalized database under heavy write load might perform worse than its normalized counterpart.

## Caching
* A cache is a simple key-value store and it should reside as a buffering layer between your application and your data storage.
* Whenever your application has to read data it should at first try to retrieve the data from your cache. Only if it’s not in the cache should it then try to get the data from the main data source.
* cache is lightning-fast. It holds every dataset in RAM and requests are handled as fast as technically possible.

## Caching Overview

### Types of Caching
* cache-aside (lazy loading):  Only requested data is cached, which avoids filling up the cache with data that isn't requested.
* write-through: all writes go to cache
* Write-behind (write-back): Add/update entry in cache, Asynchronously write entry to the data store, improving write performance
* ^ aka you add the write operation to a async queue
* Refresh-ahead: You can configure the cache to automatically refresh any recently accessed cache entry prior to its expiration.


- **Client Caching**: Located on the client side (OS or browser) to reduce server load and improve response times.

- **CDN (content delivery network) Caching**: A type of cache that stores content closer to users for faster delivery.

- **Web Server Caching**:
  - Reverse proxies (e.g., Varnish) can serve static and dynamic content directly.
  - Web servers cache requests and serve responses without contacting the application server.

- **Database Caching**:
  - Built-in caching configurations are often included by default.
  - Custom tuning can optimize for specific usage patterns.

- **Application Caching**:
  - In-memory caches (e.g., Memcached, Redis) act as key-value stores between the app and data storage.
  - Faster than disk-based databases due to RAM storage.
  - Uses cache invalidation algorithms (e.g., LRU) to manage limited RAM.

### Redis Additional Features
- **Persistence options**
- **Built-in data structures**: Sorted sets, lists, etc.

### Caching Levels
- **Row-level**: Cache individual rows.
- **Query-level**: Cache full query results.
- **Objects**: Cache fully-formed serializable objects.
- **HTML**: Cache fully-rendered HTML for quick delivery.

### Best Practices
- Avoid file-based caching for easier cloning and auto-scaling.


Two types
### 1 - Cached Database Queries
* That’s still the most commonly used caching pattern. Whenever you do a query to your database, you store the result dataset in cache. A hashed version of your query is the cache key.
* The main issue is the expiration. It is hard to delete a cached result when you cache a complex query (who has not?). When one piece of data changes (for example a table cell) you need to delete all cached queries who may include that table cell. You get the point?
* Challenge with Invalidation: When data changes in the database (e.g., a product's price is updated), it can be difficult to know which cached queries are affected. If a table cell changes, multiple cached queries might include that cell, and tracking down and invalidating all those specific cache entries can be complex. This is the main issue with using query caching in applications with complex or related data structures.

### 2 - Cached Objects
That’s my strong recommendation and I always prefer this pattern. In general, see your data as an object like you already do in your code (classes, instances, etc.)
* you cache objects like a "Class Product {}" gets instanitated, and a bunch of database calls occur filling in the data
* if the database sees a field has chnaged, it invalidates that cache entry, preventing stale data
* Reduced Latency: Fetching a fully assembled object from the cache is much faster than making multiple database queries to gather different parts of the product data.
* It's a **single** cache call instead of many
* Offloading Database Load: By caching full objects, your database handles fewer read requests because the application can fetch data from the cache instead. This allows the database to focus on write operations and other queries that are not cached.
* Asynchronous Assembly with Worker Servers: You can set up worker servers that periodically fetch fresh data from the database, assemble complete objects, and store them in the cache.
* ^ this can help with scalability, it can fetch "popular cache entries" and go ahead and update them during non-peak times to ensure faster responses during peak times

## Throughput vs Latency


* Throughput is the amount of operartions per second

* Latency is the time it takes for an individual operation

* Latency can be affected by things like "how far the server is from where i am"
* Having servers in multiple availability zones can decrease latency by being CLOSER to the request

Single vs. Batch Processing:

* Low Latency: Single processing handles each request immediately, ensuring fast response times but may limit total throughput.
* High Throughput: Batch processing groups requests, improving overall system capacity but increasing latency for individual operations.

## Availability vs Consistency

#### PACELC
* Given P (network partition/outage) choose A (availability) or C (consistency)
* E (else) choose between L (latency) or C (consistency)
* This is talking about the differency between L (eventual/weak consitency) vs C (strong consistency)

first part is CAP theorem, second part is eventual vs strong consistency


#### CAP theorem
* Only really applies to replicated data

states that in a **distributed system**, you can only achieve **two out of three** guarantees: **consistency** (all nodes see the same data), **availability** (every request gets a response), and **partition tolerance** (the system works despite network failures).

* Normal systems will always at LEAST maintain partition tolerance (people can continue to use the service)
* Availability means every signle database node can respond to requests
* Therefore we can either choose for partitioned nodes to be **Availible and inconsitent** or **Unavailable for application consistency**

##### Real-Life Handling of Partition Tolerance

Partitioning = losing connection to other parts of the database, or replicated databases in other areas that are able to recieve writes (wouldn't be an issue in a read only database)

##### AP Systems (Availability and Partition Tolerance):
* Example: NoSQL databases like Cassandra, DynamoDB, and other distributed systems often prioritize availability and partition tolerance. These systems continue to respond to requests even when parts of the network are partitioned, accepting that the data might not be the most up-to-date (eventual consistency).
* Basically, when parts of the database can't communicate with one anohter, we just use the data we have available

##### CP Systems (Consistency and Partition Tolerance):
* Example: Databases like HBase or systems that use distributed consensus algorithms (e.g., Paxos, Raft) prioritize consistency and partition tolerance. During a partition, these systems ensure that data is consistent, even if that means some parts of the system might become unavailable until the partition is fixed.
* when we have connection issues, we disable access teh database with faulty network, leaving customers in that area without access to the program

##### Key Mechanisms for Partition Tolerance
* Replication: Data is replicated across multiple nodes so that if one part of the network is unreachable, other nodes can still respond to user requests.
* Eventual Consistency: In AP systems, data changes propagate to all nodes over time. Users might get slightly outdated data during a partition, but the system reconciles and ensures consistency once the partition is resolved.
* Quorum-Based Voting: Some systems use quorum mechanisms to maintain partition tolerance. For a write or read to succeed, a majority of nodes (a quorum) must agree. This balances availability and consistency while maintaining partition tolerance.

###### Eventual consistencey in AP systems
* During a Partition: When parts of the system cannot communicate with each other, each node or shard continues to serve requests with the data it has. Writes are accepted locally, but they don’t propagate immediately to other nodes due to the partition.
* After the Partition: Once the partition is resolved and network connectivity is restored, nodes synchronize by exchanging updates to ensure all copies of the data are consistent. This process is often handled by a background reconciliation mechanism or conflict resolution strategy.

Much of this is automatically done in high AP systems

**Simple takeaway**: If you need the system to **always respond**, even if the data might not be **up-to-date**, prioritize making it **highly available**. If your priority is that data should always be **accurate and consistent**, design the system to ensure **data consistency** but be prepared for it to potentially become **unavailable during network issues**.


## Consistency Patterns

True Consistency = Every read receives the most recent write or an error.


#### Weak consistency
* **Asyncronous**
* After a write, reads may or may not see it. A best effort approach is taken.
* This approach is seen in systems such as memcached. Weak consistency works well in real time use cases such as VoIP, video chat, and realtime multiplayer games. For example, if you are on a phone call and lose reception for a few seconds, when you regain connection you do not hear what was spoken during connection loss.

#### Eventual consistency
* **Asyncronous**
* Generally have one master database, and other follower databases. 
Eventual consistency means there is a window of time after a write operation where the data might not be the same across all copies of the database. During this period, different nodes or replicas may show different versions of the data, leading to potential inconsistencies.
* usually a few milliseconds between write and data replication/sychronization

#### Strong consistency
* **Syncronous**
* data is replicated synchronously, but **increases latency**
* Leader-Based Replication: Distributed databases with a primary-replica (leader-follower) architecture often enforce strong consistency by requiring all writes to go through the leader.
* reads may be delayed until all writes are completed 



https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#availability-patterns\

This is where I left off on that google page

#### Neetcode

## SSD/HHD (TB) vs RAM (GB)
* RAM (random access memory) is directly connected to the CPU, it's much much much faster than Disk, 1/10^6 seconds.
## Cache (MB)
* Cache is even faster than ram 1/10^9 of a second

## Networking REview

* HTTP = Applicatin Layer (in green in picture, it's all in one packet here)
* TCP = Transport Layer (makes sure data sent by IP is in the right order and reformed, put in the TCP header)
* IP = Network Layer (routing and addressing of sent packets, put in the IP header)
<img width="733" alt="Screenshot 2024-11-04 at 4 28 21 PM" src="https://github.com/user-attachments/assets/dc886861-2c7a-48ca-bd6d-5cff10dd0490">

### UDP vs TCP
* TCP has a 3 way handshake
* TCP takes more time, and establishes a connection before sending
* sends in order, and resends missing packets

* UDP is connectionless
* Fast
* No guarentee that every packet of data will arrive (or maybe not in order)

## DNS

<img width="724" alt="Screenshot 2024-11-04 at 6 11 34 PM" src="https://github.com/user-attachments/assets/dd28e6ca-e58c-4153-a197-220dfd2e9beb">

#### A Records
"Google.com" -> IP Address

## APIs

* APIs have optional parameters remember
* **pagination** is very imporant in API design, so you don't accidentaly grab way too much data
* **GETS** are **Idempotent**, meaning we ALWAYS get the same response back from the same query

#### Representational state transfer (REST) API
* Follows the CRUD verbs for data
* needs to say what the response looks like (uses CORRECT status codes)
* Representation through headers in REST means using HTTP headers like Accept and Content-Type to specify and negotiate the format of data exchanged between the client and server. 
* stateless (doesn't need previous information, all that is in the request)
* So like if we need 10 videos and then the next 10 videos, we would PASS PAGINATION into the query parameters
* as opposed to clicking "next 10" twice with no query parameters
* (HTML interface for HTTP) - your web service should be fully accessible in a browser.

* Resource-Based: REST APIs are built around resources (e.g., users, products), which are represented by URIs (Uniform Resource Identifiers).
* Standard HTTP Methods: Uses standard HTTP methods such as GET, POST, PUT, DELETE to perform CRUD (Create, Read, Update, Delete) operations.
* Uniform Interface: REST adheres to a consistent and uniform set of rules, making it easy to understand and interact with.

#### HTTP
* request response protocol
* HTTP is built on top of TCP
* slower since we have a send a request every time we make a call

#### Web Socket
* Full-Duplex Communication: WebSocket provides a persistent, two-way connection between the client and server. Both parties can send and receive data at any time after the connection is established.
* Persistent Connection: The connection remains open, allowing for real-time, continuous communication.

#### HTTP2
* http with multiple data streams
* neetcode.io says it makes web sockets "obsolete"

<img width="512" alt="Screenshot 2024-11-04 at 6 38 37 PM" src="https://github.com/user-attachments/assets/597dc5ac-328c-46f9-8662-9fd6b7f64224">

#### GraphQL
* built ontop of HTTP
* fixes overfetching
* aka maybe ONLY the user's name and not the ENTIRE user
* fixes underfetching
* allows more data to be grabbed per call, group requests into one request

#### gRPC
* built ontop of HTTP2
* must be queried from a proxy, gRPC needs fine grained control which you can't do browser
* generally server to server
* It's objectively fster than REST
* it sends info in protocol buffers, sends it as binary
* protocol buffers are then converted to objects in your language of choice
* has a specified schema for request and response

## Cache
* descreases network cost
* We store **static** content in the local disk/cache
* either "hits" and "miss"
* Cache ration is (hit / hit + miss), we want a high cache ratio 
#### Broswer Cache
* Chrome stores cache data on the client's local disk
* This can be cleared by the client
#### Server Cache
* Regional storage of data in RAM or Memory of servers
* This cannot be cleared by the client

  
#### CDN (Content Delivery Network)
* Globally distributed network of proxy servers
Serving content from CDNs can significantly improve performance in two ways:
1. Users receive content from data centers close to them
2. Your servers do not have to serve requests that the CDN fulfills

* CDN can only store **Static** content. (images, videos, html, js)
* CDNs are best utliized when your application has a lot of static content to deliver
* We WOULD do this for all the data in the Origin Server, but some data will be different for each user: we wouldn't store dynamic content, like a twitter feed, or very secure content, like passwords
* Some requests are marked "prviate" meaning they aren't allowed on CDN (passwords)
* Fouses on Global delivery of content
* Removes strain on the **Origin Server** and increases availability
* **AWS Cloudfront** edge locations, howver, CAN do computation/dynamic content. (Ex: Lambda at Edge)


#### CDN Pull vs Push
* Push CDNs means when the origin server recieves a request, it pushes to **all** CDN nodes
* Pull CDNs acts more like a traditional cache, only updating the CDN where a request is made with the data it requested
* Pull makes a reqeust to CDN, has a miss, and then acts as a proxy, sending the response to the Origin Server
* sites with **heavy traffic** favor **Pull CDNS**
* **TTL (Time to Live)** Decides when the cached content gets removed. Could be every hour for exmaple. 

#### Disadvantage(s)
* CDN costs could be significant depending on traffic, although this should be weighed with additional costs you would incur not using a CDN.
* Content might be stale if it is updated before the TTL expires it.
* CDNs require changing URLs for static content to point to the CDN.

## Redit / Memcached
* regardless of where its stored, (the main server's memory/RAM or a separate server's memory/RAM), it's always stored in **RAM / MEMORY**
* Getting data off of RAM is always faster than a disk
* In-memory caches like Redis or Memcached can typically handle hundreds of thousands to millions of reads per second
* Traditional disk-based databases (e.g., MySQL, PostgreSQL) generally handle thousands to tens of thousands of reads per second
* about 100x faster


### **Write-Around Cache**
- **Process**: Data is written directly to the **database**, not the cache.
- **Reading**: Data is read from the **cache**. If the data is not present (a cache miss), it is fetched from the **database** and then added to the cache for future reads.
- **Pros**: Reduces the load on the cache for infrequently accessed data.
- **Cons**: High **cache miss rate** initially, which can result in slower reads until the cache is populated.
* better for majority writes

### **Write-Through Cache**
- **Process**: Data is written to the **cache** first and then **immediately** written to the **database** as well.
- **Reading**: Data is read from the **cache**.
- **Pros**: Ensures that the **cache and database** are always in sync, reducing cache misses.
- **Cons**: Slightly slower write operations due to dual writes to both the cache and the database.
* better for majority reads

### **Write-Back Cache (Write-Behind Cache)**
- **Process**: Data is written to the **cache** first, and the write to the **database** is deferred and done asynchronously at a later time.
- **Reading**: Data is read from the **cache**.
- **Pros**: Faster write performance because the write operation completes once the cache is updated. Reduces database load by batching writes.
- **Cons**: Risk of data loss if the cache fails before the data is written to the database. Data inconsistency can occur if the database is not updated in time.
* This would work for things like "liking" a post on twitter, since it's not imporant if it's lost, bad for "posting" a tweet though

### Eviction Policies
* FIFO: good for data that's less accessed as time goes on
* LRU (least recently used): if something gets read, it goes to top of FIFO
* LFU (least frequently used): # of times its used is tracked, we remove the least "popular" tweet
* problem with LFU is that the value doesn't reset automatically, so we might keep caching old popular stuff

## Proxies

## Reverse Proxy


* A reverse proxy is a web server that centralizes internal services and provides unified interfaces to the public.
* NGINX, Load Balancers
* Directs traffic for the backend, middleman on behalf of the servers
#### Benefits
* Increased security - Hide information about backend servers, blacklist IPs, limit number of connections per client
* SSL termination - Decrypt incoming requests and encrypt server responses so backend servers do not have to perform these potentially expensive operations. Removes the need to install X.509 certificates on each server
* Increased scalability and flexibility - Clients only see the reverse proxy's IP, allowing you to scale servers or change their configuration
* Reverse proxies can be useful even with just one web server or application server, opening up the benefits described in the previous section. **(AS OPPOSED TO LOADBALANCER)**
## Forward Proxy
* Regions Bank Forward Proxy, VPNs
* Can be used to make the user anonymous (the server acts as the user), or can be used to hide/monitor requests (like a business blacklisting certian websites)
* VPNs act aas a forward proxy, hiding the user (but it also encrypts data)

## Load Balancer
<img width="600" alt="Screenshot 2024-11-06 at 2 44 27 PM" src="https://github.com/user-attachments/assets/90da81db-c16b-4556-86d8-53835350ea3c">
* Load balancers can be implemented with hardware (expensive) or with software such as HAProxy.
* To protect against failures, it's common to set up multiple load balancers, either in active-passive or active-active mode.

#### Horizontal scaling
* Obviously helps with Horizontal Scaling of servers and services
* Sessions can be stored in a centralized data store such as a database (SQL, NoSQL) or a persistent cache (Redis, Memcached)
* Stateless Servers: For true horizontal scaling, servers should be stateless, meaning no server should hold session data locally. This allows any server to handle any user request without needing to stick to a specific server.
#### DrawbacksHash-Based Session Distribution
Scalability Limitations:
* Non-stateful, can lead to complications when individual servers go down (could cause shuffling)
* If a server fails, any sessions stored locally on that server are lost unless there is a backup mechanism.
* This can lead to challenges in high availability and fault tolerance.

If one server becomes overloaded with too many user sessions, distributing based on a hash can lead to imbalanced traffic.
Adding or removing servers can disrupt the hash distribution and cause a "reshuffling" effect, where users are redirected to different servers, potentially losing their session data.
#### Traffic Distribution
* Round Robin Approach: Send it to all servers before sending it to the same one again
* Weighted Round Robin: Certain servers get a larger percentage
* Least Connections: least people connected to that server (connected = request being processed)
* Location Based
* Random
* Based on Session Cookies

#### Layer 4 Network Load Balancer
* Layer 4 = TCP
* Network Load Balancer
* Can use IP for location or round robin
* NO ACCESS TO APPLICATION OR HTTP

#### Layer 7 Application Load Balancer
* Uses HTTP
* Does have access to the data, and can route based on reqeusts
* Could send requests to different kinds of servers, not just duplicates
* For example, a layer 7 load balancer can direct video traffic to servers that host videos while directing more sensitive user billing traffic to security-hardened servers.

#### Consistent Hashing
Maps a `cirular hash store` so that removals don't cause shuffling

* this way, we can send the same user to the same server every time, and each user can access the same cache every visit
<img width="707" alt="Screenshot 2024-11-05 at 4 37 50 PM" src="https://github.com/user-attachments/assets/e3b534cf-79a8-461e-b8ac-810bfe7a74be">


#### Object Storage
* AWS S3, Azure Blob Storage
* Illusion of a file system
* You can write or read, but **can't update*
* **No Duplicates**, Ex: AWS S3 buckets must be **Global**
* Meant to store large viles ex: photo and video

## Message/Event Queues
* Ex: RabbitMQ, Kafka
* decouples parts of the application (producers/consumers)
* Bulk processing
* Asyncronous processing
* Allows for pub/sub systems, queue publishes to many subscribers
* Example: A new user signup event is sent to the queue, with multiple subscribers handling different tasks: Database Service saves user data, Email Service sends a welcome email, Analytics Service logs the signup event.
* "Fanning out" to multiple topics

## Data Processing

#### Batch

#### Streaming
* Apache Flink

## Map Reduce
* Modern times we use a "Spark cluster" for batch processing

## Availability patterns




### Failover
###### Advantages:
* when you server goes down, another server can pick up the work and clients don't see any downtime
###### Disadvantages:
* Fail-over adds more hardware and additional complexity.
* There is a potential for loss of data if the active system fails before any newly written data can be replicated to the passive.

#### Active-passive Failover
* With active-passive fail-over, heartbeats are sent between the active and the passive server on standby. If the heartbeat is interrupted, the passive server takes over the active's IP address and resumes service.
* Also known as master-slave failover
#### Active-Active Failover
* If the servers are public-facing, the DNS would need to know about the public IPs of both servers.
* In this case, DNS health checks determine which server to send traffic to
* If the servers are internal-facing, application logic would need to know about both servers.
* In cloud environments, internal-facing services often use private IP addresses or internal load balancers, which means that only services within the same network or virtual private cloud (VPC) can communicate with them.
* Active-active failover can also be referred to as master-master failover.

### Replication
discussed in database section 

### Availability in parallel vs in sequence
* If a service consists of multiple components prone to failure, the service's overall availability depends on whether the components are in sequence or in parallel.

## DNS
<img width="900" alt="Screenshot 2024-11-06 at 2 13 02 PM" src="https://github.com/user-attachments/assets/a7c94235-c600-4e27-ad6d-9c489626da8a">

* 1. Initial Request: The user device (e.g., computer or smartphone) requests the IP address for www.google.com, which goes to the ISP DNS server (or any configured recursive resolver).
* 2. ISP DNS Server Cache Check: If the ISP DNS server does not have the IP address in its cache, it needs to find the answer by querying further up the chain.
* 3. Recursive Query to the Root DNS Server: The ISP DNS server contacts a root DNS server, which responds with a referral to the Top-Level Domain (TLD) DNS server (e.g., .com DNS server).
* 4. Query to the TLD DNS Server: The ISP DNS server then contacts the TLD DNS server, which provides a referral to the authoritative name server for google.com.
* 5. Authoritative Name Server Response: The authoritative name server for google.com holds the actual DNS records, such as A records, CNAME records, etc. It responds with the IP address for www.google.com (e.g., 173.194.115.96).
* 6. ISP DNS Server Caches the Response: The ISP DNS server caches the response to serve future requests more quickly.
* 7. Final Response to the User: The ISP DNS server sends the IP address back to the user's device, allowing it to connect to www.google.com.
* NS record: example.com -> ns1.dnsprovider.com, ns2.dnsprovider.com (specifies the DNS servers for the domain) (DNS Servers hold A Records)
* MX record: example.com -> mail.example.com (specifies the mail server for accepting messages) (SMTP = mail protocol)
* A record: example.com -> 192.168.1.1 (points a domain to an IP address)
* CNAME record: www.example.com -> example.com (points one domain name to another domain name) (Authoritative DNS Servers keep track of CNAME records)

A DNS server for a domain is a server that translates domain names (e.g., example.com) into IP addresses (e.g., 192.168.1.1), allowing users to access websites using human-readable names instead of numerical IP addresses.

## DNS Traffic Distribution
* weighted round robin
* geolocation based
* latency based

## Application Layer
* Separating out the web layer from the application layer (also known as platform layer) allows you to scale and configure both layers independently.


## Microservices
* Related to this discussion are microservices, which can be described as a suite of independently deployable, small, modular services. Each service runs a unique process and communicates through a well-defined, lightweight mechanism to serve a business goal.
* 

## Service Discovery
* Systems such as Consul, Etcd, and **Zookeeper** can help services find each other by keeping track of registered names, addresses, and ports.

## Async

## Message Queues
* Back Pressure: setting a max to queue sizes, RAM gets overwhelmed and we end up doing more disks reads. They send a 503 service unavailable message

## Remote procedure call (RPC)
* In an RPC, a client causes a procedure to execute on a different address space, usually a remote server.
* The procedure is coded as if it were a local procedure call, abstracting away the details of how to communicate with the server from the client program.
* Remote calls are usually slower and less reliable than local calls so it is helpful to distinguish RPC calls from local calls.
* Popular RPC frameworks include Protobuf, Thrift, and Avro.

* 
RPC frameworks like gRPC offer several advantages over traditional REST APIs for certain use cases, especially when it comes to performance, efficiency, and ease of use in distributed systems. Here’s why RPC (specifically gRPC) might be preferred over a typical API:

1. Efficiency and Performance:
* Binary Protocols: RPC frameworks like gRPC use Protocol Buffers (Protobuf) for data serialization, which is a compact binary format. * This makes data transmission much faster and more efficient compared to JSON used in REST, which is text-based and less space-efficient.
2. Lower Latency:
* Because gRPC uses a binary format and HTTP/2, it can achieve lower latency and faster communication compared to traditional REST APIs using HTTP/1.1.
3. HTTP/2 Support:
* Multiplexing: gRPC leverages HTTP/2, which allows multiple requests to be sent simultaneously over a single connection.
* This reduces the overhead of establishing multiple connections and improves the performance of real-time and highly interactive applications.

### Cons
* HTTP APIs following REST tend to be used more often for public APIs.
* RPC clients become tightly coupled to the service implementation.
* A new API must be defined for every new operation or use case.
* It can be difficult to debug RPC.

## Security
* Encrypt in transit and at rest.
* Sanitize all user inputs or any input parameters exposed to user to prevent XSS and SQL injection.
* Use parameterized queries to prevent SQL injection.
* Use the principle of least privilege.

# system-design-notes

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

## System Design Practice
If you can’t find a friend/coworker, go to a site like HighScalability that contains tons of examples of real-life architectures and compare what you conjured up to what these companies actually did. You’d be surprised how many of the top websites’ architectures are widely available. Keep in mind that these systems were designed by multiple people over multiple months, so don’t expect to be able to come up with every low-level decision they made. Focus on the high-level stuff.
* https://www.hiredintech.com/system-design/the-twitter-problem/
* HelloInterview has practice problems, could be worth the 30 bucks
* https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#system-design-interview-questions-with-solutions

## Object Oriented Design Practice
* https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#object-oriented-design-interview-questions-with-solutions
* 

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


## SQL vs NOSQL

* SQL = relational        can do joins in one query in the database
* NoSQL = non-relational  needs to do multiple queries sometimes to get data

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
* ACID systems are called **Transactional Systems**
* Atomicity: Transactions are treated as a single, indivisible unit. Either all operations within the transaction succeed, or none of them do.
Example: Transferring money between bank accounts. If the withdrawal from one account succeeds but the deposit into another fails, the entire transaction is rolled back to maintain consistency.
* Consistency: Transactions must maintain the consistency of the database. Data should meet all rules and constraints before and after the transaction.
Example: In a reservation system, booking a seat only if it's available ensures that no two customers book the same seat simultaneously.
* Isolation: Transactions are isolated from each other to prevent interference. Concurrent transactions should not impact each other's outcomes.
Example: Simultaneous bank transfers from the same account should not interfere with each other, ensuring each transaction sees a consistent view of the data.
* Durability: Committed transactions are permanently saved and survive system failures. Once a transaction is committed, its changes remain even after a power outage or crash.
Example: After confirming a purchase online, ensuring that the transaction is saved and the goods are allocated, even if there's a temporary network failure.


## Considerations:
* Choose SQL: For transactional systems, secure and consistent data needs, complex queries, and strong data integrity requirements.

* Choose NoSQL: For high scalability, flexibility in data modeling, handling big data, high availability, and agile development with evolving data structures.

## Database Sharding

DynamoDB and similar NoSQL databases (like Cassandra or MongoDB) handle sharding automatically based on partition keys, ideal for distributed architectures.

Relational databases generally do not do this by default. 

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

## Caching
* A cache is a simple key-value store and it should reside as a buffering layer between your application and your data storage.
* Whenever your application has to read data it should at first try to retrieve the data from your cache. Only if it’s not in the cache should it then try to get the data from the main data source.
* cache is lightning-fast. It holds every dataset in RAM and requests are handled as fast as technically possible.

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

**CAP theorem** states that in a **distributed system**, you can only achieve **two out of three** guarantees: **consistency** (all nodes see the same data), **availability** (every request gets a response), and **partition tolerance** (the system works despite network failures).

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
* After a write, reads may or may not see it. A best effort approach is taken.
* This approach is seen in systems such as memcached. Weak consistency works well in real time use cases such as VoIP, video chat, and realtime multiplayer games. For example, if you are on a phone call and lose reception for a few seconds, when you regain connection you do not hear what was spoken during connection loss.

#### Eventual consistency

Eventual consistency means there is a window of time after a write operation where the data might not be the same across all copies of the database. During this period, different nodes or replicas may show different versions of the data, leading to potential inconsistencies.
* usually a few milliseconds between write and data replication/sychronization

#### Strong consistency
* data is replicated synchronously
* Leader-Based Replication: Distributed databases with a primary-replica (leader-follower) architecture often enforce strong consistency by requiring all writes to go through the leader.
* reads may be delayed until all writes are completed 



https://github.com/donnemartin/system-design-primer?tab=readme-ov-file#availability-patterns\

This is where I left off on that google page

#### Neetcode

## SSD/HHD (TB) vs RAM (GB)
* RAM (random access memory) is directly connected to the CPU, it's much much much faster than Disk, 1/10^6 seconds.
## Cache (MB)
* Cache is even faster than ram 1/10^9 of a second

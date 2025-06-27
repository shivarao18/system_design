# System Design: Module 1 - The Building Blocks

This module covers the foundational concepts and principles that govern the design of scalable, distributed systems.

---

## 1. Introduction to System Design

* **What is it?** The process of defining the architecture, components, interfaces, and data for a software system to satisfy specified requirements. It's the architectural blueprint for a software product.
* **Why is it important?** It's used in interviews to assess a candidate's thought process, technical breadth, communication skills, and ability to handle large, ambiguous problems.
* **High-Level Design (HLD):** The "bird's-eye view" of the system. It describes the major components and their interactions (e.g., Load Balancers, Web Servers, Databases, Caches). This is the focus of most system design interviews.
* **Low-Level Design (LLD):** The "zoomed-in view" of a single component. It describes the internal logic, such as class diagrams, database schemas, and API contracts.

---

## 2. Core Concepts in Distributed Systems

These are the fundamental "-ilities" or non-functional requirements that guide design choices.

### a. Scalability

The ability of a system to handle a growing amount of work.

| Type                  | Vertical Scaling (Scaling Up)                               | Horizontal Scaling (Scaling Out)                            |
| :-------------------- | :---------------------------------------------------------- | :---------------------------------------------------------- |
| **Method** | Add more resources (CPU, RAM, SSD) to a **single server**.  | Add **more servers** to a pool of resources.                |
| **Analogy** | A stronger superhero.                                       | An army of soldiers.                                        |
| **Pros** | Simple to manage; no distributed complexity.                | Elastic, resilient (no single point of failure), cost-effective. |
| **Cons** | Very expensive, has a hard physical limit, creates a **Single Point of Failure (SPOF)**. | Introduces complexity (requires load balancing, data is distributed). |
| **Primary Use Case** | Simpler applications, stateful systems like some databases. | Most large-scale web applications (Google, Netflix, etc.).   |

### b. Availability

The measure of a system's operational uptime, often expressed in "nines." High availability is achieved by eliminating Single Points of Failure (SPOFs).

* **99.9%:** ~43 minutes of downtime/month.
* **99.99%:** ~4 minutes of downtime/month.
* **99.999%:** ~26 seconds of downtime/month.

**How Horizontal Scaling improves Availability:** With multiple servers behind a load balancer, if one server fails, the load balancer's **health checks** will detect the failure and redirect traffic to the healthy servers, keeping the application online.

### c. Reliability

Ensures the system performs its intended function **correctly**. A system can be available but unreliable.

* **Availability:** Is the ATM online and responsive?
* **Reliability:** When I withdraw $100, does it give me exactly $100 and debit my account correctly?

Reliability is achieved through data replication, atomic transactions (ACID), and robust testing.

### d. Latency vs. Throughput

These are the two primary metrics for system performance.

* **Latency:** The time it takes for a single request to complete (a round trip). It's a measure of **speed**. The goal is **low latency**. (e.g., a stock trade must execute instantly).
* **Throughput:** The number of requests a system can handle in a given time period (e.g., Requests Per Second). It's a measure of **capacity**. The goal is **high throughput**. (e.g., a data processing system analyzing millions of records).

**Analogy:** For a highway, latency is the time for one car to travel from A to B. Throughput is the total number of cars that can pass point A in an hour.

### e. Consistency

Ensures that all clients see the same data at the same time, especially when data is replicated across multiple servers.

* **Strong Consistency:**
    * **Guarantee:** After a write is complete, every subsequent read will see that new value.
    * **Analogy:** A bank account balance. It must be 100% up-to-date for every transaction.
    * **Trade-off:** Higher safety, but can result in higher latency and lower availability.
    * **Use Case:** Shopping carts, inventory systems, financial transactions.

* **Eventual Consistency:**
    * **Guarantee:** If no new writes occur, all replicas will *eventually* converge to the same value. A brief period of staleness is possible.
    * **Analogy:** A social media "like" count. It's acceptable if it takes a few seconds to be visible to everyone globally.
    * **Trade-off:** Lower latency and higher availability, but more complex for developers to handle stale data.
    * **Use Case:** Social media feeds, view counts, user profile updates—anywhere a slight delay is acceptable.
 
# System Design: Module 2.1 - Data Storage (SQL vs. NoSQL)

This section covers the fundamental choice of data storage, breaking down the differences between relational (SQL) and non-relational (NoSQL) databases.

---

## 1. SQL (Relational Databases)

Relational databases store data in a highly structured format using tables with rows and columns. They rely on a predefined schema and are known for reliability.

* **Analogy:** A well-organized library card catalog or an Excel spreadsheet.
* **Schema:** **Predefined Schema (Schema-on-Write)**. The table structure (columns, data types) must be defined before data is inserted.
* **Key Guarantee (ACID):** This is the main reason to choose SQL for critical systems.
    * **A**tomicity: All parts of a transaction succeed, or the entire transaction is rolled back.
    * **C**onsistency: Data is always in a valid state.
    * **I**solation: Concurrent transactions don't interfere with each other.
    * **D**urability: Once saved, data is permanent, even after a system crash.
* **Scaling:** Traditionally scaled **vertically** (by increasing server power).
* **Examples:** `MySQL`, `PostgreSQL`, `Microsoft SQL Server`.
* **Use When:** You have structured data, a stable schema, and require absolute transactional integrity (e.g., financial systems, e-commerce orders, inventory).

---

## 2. NoSQL (Non-Relational Databases)

NoSQL is an umbrella term for databases that do not use the traditional relational model. They are prized for their flexibility and horizontal scalability.

* **Analogy:** A folder that can hold any type of file (Word docs, PDFs, images).
* **Schema:** **Dynamic Schema (Schema-on-Read)**. The structure is flexible and can vary from entry to entry.
* **Key Principle (BASE):** Prioritizes availability and scale over strict consistency.
    * **B**asically **A**vailable
    * **S**oft state
    * **E**ventually consistent
* **Scaling:** Designed to scale **horizontally** (by adding more servers).

### Types of NoSQL Databases

| Database Type      | Data Model                               | Primary Strength                        | Use Case Example                        |
| :----------------- | :--------------------------------------- | :-------------------------------------- | :-------------------------------------- |
| **Document** | JSON-like Documents                      | Flexible schema, intuitive for developers | Blog posts, product catalogs            |
| **Key-Value** | `(Key, Value)` pairs                     | Extreme speed for simple lookups        | Caching (Redis), user sessions          |
| **Column-Family** | Rows with varied, grouped columns        | Massive write throughput, analytics     | Logging, time-series data (Cassandra)   |
| **Graph** | Nodes (entities) and Edges (relationships) | Traversing complex relationships        | Social networks, fraud detection (Neo4j)|

### Detailed Breakdown:

* **Document Databases (e.g., MongoDB):**
    * Stores data in self-contained documents (like JSON). All data for an object (e.g., a blog post and its comments) can be in one document, making reads fast. Great general-purpose NoSQL DB.

* **Key-Value Stores (e.g., Redis):**
    * The simplest model. A unique key maps to a value. Incredibly fast (`O(1)` lookup) but you can only query by the key. Perfect for caching.

* **Column-Family Stores (e.g., Cassandra):**
    * Stores data in columns rather than rows. Optimized for fast writes and analytical queries that aggregate data from specific columns over millions of rows. Ideal for logging and time-series data.

* **Graph Databases (e.g., Neo4j):**
    * Purpose-built to store and traverse relationships. The connections between data are as important as the data itself. Avoids slow, recursive `JOINs` needed in SQL for similar tasks.


# System Design: Module 2.2 - Communication Protocols & APIs

This section covers the primary methods and architectural styles that services use to communicate with each other and with clients.

---

## 1. REST (REpresentational State Transfer)

A set of architectural principles for designing networked applications, using the standard HTTP protocol. It is the most common style for web APIs.

* **Analogy:** Ordering a specific combo meal from a restaurant menu. The options are fixed.
* **Architecture:** Client-server model where clients interact with **Resources** via unique URLs (endpoints).
* **Core Components:**
    * **Endpoint (URL):** The address of a resource (e.g., `/users/123`).
    * **HTTP Method:** The action to be performed.
        * `GET`: Read a resource.
        * `POST`: Create a new resource.
        * `PUT`/`PATCH`: Update an existing resource.
        * `DELETE`: Remove a resource.
    * **Headers:** Metadata about the request (e.g., `Authorization`, `Content-Type`).
    * **Body:** The data payload for `POST` or `PUT` requests (usually JSON).
* **Strength:** Standardized, stateless, and leverages the existing web infrastructure.
* **Weakness:** Can lead to **over-fetching** (getting more data than needed) or **under-fetching** (needing multiple requests to get all required data).

## 2. GraphQL

A query language for APIs that allows clients to request exactly the data they need.

* **Analogy:** A buffet where you pick exactly which items you want on your plate.
* **Architecture:** Typically uses a single endpoint (e.g., `/graphql`). The client sends a `POST` request with a query detailing its data requirements.
* **Core Concepts:**
    * **Schema:** A strong type system defined on the server that describes all available data.
    * **Query:** Sent by the client to read data. The response JSON mirrors the query structure.
    * **Mutation:** Sent by the client to write (create, update, delete) data.
* **Strength:** Solves over/under-fetching, reducing the number of network round trips. Highly efficient for complex views and mobile clients.

## 3. Real-Time Communication & WebSockets

Used when the server needs to push data to the client without waiting for a request.

* **Polling (The Old Way):** The client repeatedly asks the server "is there anything new?" Inefficient.
* **WebSockets (The Modern Way):** Establishes a persistent, stateful, two-way communication channel between the client and server.
    * **Analogy:** An open phone line versus sending letters back and forth.
    * **How it works:** Starts with an HTTP "Upgrade" request and keeps the TCP connection alive for full-duplex communication.
    * **Strength:** Very low latency, efficient, ideal for "live" features.
    * **Use Cases:** Chat applications, live stock tickers, collaborative editing tools, real-time notifications.
 
# System Design: Module 2.3 - Messaging and Queuing Systems

This section covers asynchronous communication between services using message queues, a pattern crucial for building scalable and resilient applications.

---

## 1. Synchronous vs. Asynchronous

* **Synchronous:** The client makes a request and is **blocked**, waiting for the server to finish its work and send a response. (e.g., a phone call).
* **Asynchronous:** The client sends a message to a service and can **immediately move on** to other tasks. It trusts the message will be processed eventually. (e.g., sending a text message).

## 2. What is a Message Queue?

A message queue is a reliable intermediary component (a buffer) that allows services to communicate asynchronously.

* **Producer (Publisher):** A service that writes a message (describing a task) to the queue.
* **Message Queue (Broker):** The "post office" that reliably stores the message until it's processed.
* **Consumer (Subscriber):** A service that reads messages from the queue and performs the specified task.

![Message Queue Diagram](https://i.imgur.com/gBw1gN1.png)

## 3. Key Benefits

* **Decoupling & Resilience:** The producer and consumer services are independent. If the consumer service fails, the messages are held safely in the queue, preventing data loss. The producer doesn't even need to know if the consumer is online.
* **Improved Responsiveness:** User-facing services can offload slow tasks to a queue in milliseconds and provide an immediate response to the user, making the application feel fast. The heavy work happens in the background.
* **Load Leveling / Buffering:** A queue can absorb sudden spikes in traffic (e.g., from a flash sale), preventing consumer services from being overwhelmed. The consumers can process the backlog at their own steady pace.

## 4. Common Patterns & Use Cases

* **Use Cases:** E-commerce order processing, video encoding, sending bulk emails, ingesting large volumes of log data.
* **Parallel Processing ("Fan-Out"):** A single event message is delivered to multiple queues so several distinct services can work on the task in parallel.
* **Sequential Processing ("Chaining"):** One service consumes a message, performs a task, and then produces a new message for the next service in the workflow, creating a reliable, multi-step process.
* **Technologies:** `RabbitMQ`, `Amazon SQS`, `Apache Kafka`.

# System Design: Module 2.4 - The CAP Theorem

This section covers the CAP Theorem, a fundamental principle that describes the trade-offs inherent in any distributed data store.

---

## 1. The Three Guarantees

The CAP Theorem states that a distributed system can only simultaneously guarantee **two** of the following three properties:

* **C - Consistency (Strong):** Every read request receives the most recent write or an error. All nodes in the system see the exact same data at the same time.

* **A - Availability:** Every request receives a (non-error) response, without the guarantee that it contains the most recent write. The system is always online and responsive.

* **P - Partition Tolerance:** The system continues to operate despite a network partition (i.e., dropped or delayed messages between nodes).

## 2. The Core Trade-off

For any real-world distributed system, network partitions **are a fact of life**. Therefore, **Partition Tolerance (P) is mandatory**.

This forces a choice between the remaining two guarantees: when a network partition happens, a system must choose to sacrifice either Consistency or Availability.

## 3. The Two System Types

* ### CP (Consistent & Partition-Tolerant)
    * **Choice:** When a partition occurs, the system chooses **Consistency** over Availability.
    * **Behavior:** To avoid returning stale data, some nodes may become unavailable. They might stop responding or return an error until the partition heals and consistency can be guaranteed.
    * **Use Cases:** Systems where correctness is paramount.
    * **Examples:** Financial systems, inventory management, relational databases.

* ### AP (Available & Partition-Tolerant)
    * **Choice:** When a partition occurs, the system chooses **Availability** over Consistency.
    * **Behavior:** All nodes remain online and respond to requests, even if it means returning stale data. The system relies on **eventual consistency** to sync up after the partition heals.
    * **Use Cases:** Systems where being online is more critical than having perfectly up-to-date data.
    * **Examples:** Social media feeds, product reviews, NoSQL databases like Cassandra.

# System Design Case Study: A URL Shortener (like TinyURL)

This document walks through the process of designing a URL shortening service, following a standard 5-step system design interview framework.

---

## The Problem

Design a service like TinyURL. At its core, this service must do two things:
1.  **Shorten:** Accept a long URL and return a much shorter, unique URL.
2.  **Redirect:** Accept a short URL and redirect the user to the original long URL.

---

### Step 1: Requirements & Scope Clarification

First, we must understand the exact requirements. After asking clarifying questions, we've agreed on the following scope for our system.

#### Functional Requirements:
* Users can input a long URL and receive a short URL.
* Users who visit a short URL are redirected to the original long URL.
* The creation process must be synchronous and feel fast to the user.
* Analytics (like click tracking) are out of scope for this initial design.

#### Non-Functional Requirements:
* **Traffic (Writes):** The system will handle **100 million** new URL creations per month.
* **Traffic (Reads):** The system will have a **10:1** read-to-write ratio.
* **Availability:** The redirection service must be **highly available**. Broken links are unacceptable.
* **Latency:** The redirection must happen with **very low latency**.

---

### Step 2: Back-of-the-Envelope Estimation

Based on the requirements, we can estimate the load and storage needs to guide our design.

* **Write Rate (Requests Per Second):**
    * `100,000,000 writes / (30 days * 24 hours * 3600 seconds)` ≈ **40 writes/sec**

* **Read Rate (Requests Per Second):**
    * Based on the 10:1 ratio: `40 writes/sec * 10` = **400 reads/sec**
    * **Key Insight:** The system is **read-heavy**. The read path (redirection) must be highly optimized.

* **Storage (for 5 years):**
    1.  **Total Records:** `100M URLs/month * 12 months/year * 5 years` = **6 Billion URLs**.
    2.  **Size per Record:**
        * `long_url`: Assume a generous average of **500 bytes**.
        * `short_alias`: We need a unique key for 6 billion items. Using Base62 (`a-z, A-Z, 0-9`), 7 characters gives us `62^7` ≈ 3.5 trillion unique combinations, which is more than enough. So, **7 bytes**.
        * Other metadata (`UserID`, `CreationDate`, etc.): ~**13 bytes**.
        * **Total per record:** `500 + 7 + 13` ≈ **520 bytes**.
    3.  **Total Storage:** `6 Billion records * 520 bytes/record` ≈ **3.12 TB**.

---

### Step 3: High-Level Design (HLD)

The architecture is split into a **Write Path** and a **Read Path** to optimize for our read-heavy workload.

!https://medium.com/@sandeep4.verma/system-design-scalable-url-shortener-service-like-tinyurl-106f30f23a82(https://i.imgur.com/uGZ8w2a.png)

#### Write Path (URL Shortening)
* **Flow:** `Client -> POST Request -> Load Balancer -> Web Server -> Primary Database`
* **Process:**
    1.  A user sends a `POST` request with the long URL.
    2.  The web server generates a unique short alias.
    3.  It writes the `(short_alias, long_url)` pair to the **Primary Database**. All writes go here to maintain a single source of truth.
    4.  The server returns the new short URL to the user.

#### Read Path (URL Redirection)
* **Flow:** `Client -> GET Request -> Load Balancer -> Web Server -> Cache -> Read Replica Database`
* **Process:**
    1.  A user clicks a short link, sending a `GET` request.
    2.  The web server first checks a **Cache** (e.g., Redis) using the short alias as the key.
    3.  **If Cache Hit:** The long URL is found in the cache. This is the fastest path and will serve the majority of requests.
    4.  **If Cache Miss:** The server queries a **Read Replica Database** to find the long URL. It then stores the result in the cache for future requests.
    5.  The server issues a `301 Permanent Redirect` to the user's browser.

#### Database Model: Primary-Replica
To handle the read-heavy load and provide high availability, we use a primary-replica setup.
* The **Primary** DB handles all writes.
* The Primary automatically copies data (**replication**) to multiple **Read Replicas**.
* The high volume of read requests is distributed across the replicas, preventing the Primary from getting overloaded.
* If the Primary fails, a replica can be promoted to become the new Primary, ensuring high availability.

---

### Step 4: Deep Dive - Alias Generation Strategy

The core logic is creating a unique 7-character alias. We considered two approaches.

1.  **Hashing (Initial Idea):**
    * **Method:** Hash the long URL (e.g., with SHA-256) and truncate the result to 7 characters.
    * **Problem:** At a scale of billions, truncating hashes leads to a very high rate of **collisions**. The logic to check for and handle these collisions (by re-hashing) would be complex and slow down the write process.

2.  **Counter + Base-62 Conversion (Chosen Design):**
    * **Method:** This approach guarantees uniqueness.
        1.  Use the database's auto-incrementing `ID` feature. Each new URL gets a unique integer (e.g., 1, 2, ..., 6,000,000,000).
        2.  Take that unique integer ID and convert it from Base-10 to **Base-62** (`a-z, A-Z, 0-9`). This gives us our short, non-colliding alias.
    * **Example:** ID `6000000000` becomes `6mZeWq` in Base-62.
    * **Result:** This method is faster, simpler, and guarantees a unique alias every time.

---

### Step 5: Identifying Bottlenecks and Scaling

As the system grows to handle 10x or 100x traffic, we must address bottlenecks.

* **Primary Bottleneck:** The database. A single server (even with replicas) cannot store tens of terabytes of data or handle millions of requests per second efficiently.

* **Solution: Database Sharding (Partitioning):**
    * **Concept:** We break our massive database into smaller, independent databases called **shards**.
    * **Strategy:** We can use **Range-Based Sharding**. The sharding key is our unique numeric `ID`.
        * **Shard 1:** Stores IDs 1 to 1 Billion.
        * **Shard 2:** Stores IDs 1B+1 to 2 Billion.
        * ...and so on.
    * **Benefit:** This distributes the data and the read/write traffic across many servers, allowing for near-infinite horizontal scaling. An application layer service is needed to route requests to the correct shard.

* **Handling "Hotspots":**
    * **Problem:** What if one URL goes viral? Its shard will get overwhelmed with traffic.
    * **Solution:** The **caching layer** is our primary defense. A viral link will be served almost entirely from the cache, protecting the database. We must ensure our cache is robust and scalable.


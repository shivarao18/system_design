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

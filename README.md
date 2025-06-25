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

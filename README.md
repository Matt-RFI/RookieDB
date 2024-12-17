# RookieDB: A Lightweight Database System Implementation 🚀

This repository showcases my implementation of key database system functionalities as part of a semester-long project. It began as a bare-bones database that supports simple transactions, and through several assignments, I extended it to include indexing, efficient join algorithms, cost-based query optimization, multi-granularity locking for concurrency control, and ARIES-based recovery.

---

## Overview

**RookieDB** started as a minimal DBMS that processes transactions serially. Over the course of the project, I integrated critical database concepts and features, reflecting a hands-on understanding of database internals. The result is a simplified yet instructive codebase that highlights important principles used in real-world relational database systems.

**Key achievements include:**
- Implementing **B+ Tree indices** for efficient data lookups.
- Building **efficient join algorithms** (e.g., nested-loop, sort-merge, and hash joins).
- Introducing a **cost-based query optimizer** to automatically select efficient query plans.
- Adding **multi-granularity locking** to handle concurrent transactions safely.
- Ensuring **durability and consistency** with an **ARIES-inspired recovery** system.

---

## Features & Highlights

1. **Indexing (B+ Trees)**  
   - Implemented a B+ tree index layer to allow logarithmic-time lookups, insertions, and deletions.
   - Enables fast range queries and significantly improves query performance over large datasets.

2. **Efficient Join Algorithms**  
   - Integrated multiple join strategies and learned how to pick the right join for different scenarios.
   - This experience highlights the importance of algorithm selection in query performance.

3. **Query Optimization**  
   - Developed a cost-based query optimizer that uses table statistics to estimate the cost of different query plans.
   - The system can reorder joins and choose indexes to minimize query execution time, reflecting the real-world importance of query planning.

4. **Concurrency Control (Multi-Granularity Locking)**  
   - Implemented a lock manager supporting multiple lock modes (S, X, IS, IX, SIX).
   - Ensured serializability, consistency, and reduced lock contention, paving the way for concurrent transaction execution.

5. **Recovery (ARIES Protocol)**  
   - Integrated logging and crash recovery mechanisms inspired by ARIES.
   - Guaranteed that the database can recover from system failures without losing committed data.

---

## Architecture Overview

- **Storage Layer**:  
  - Disk Space Management and Buffer Management to handle reads/writes from/to disk.
  - Page structures and a flexible heap file format.

- **Index Layer**:  
  - B+ tree indexes for accelerating data retrieval.

- **Query Execution Layer**:  
  - Iterators and operators for scans, joins, selections, and projections.
  - Query optimizer that chooses efficient physical plans.

- **Concurrency Layer**:  
  - Lock manager enforcing multi-granularity locking.
  - Ensures isolation among concurrent transactions.

- **Recovery Layer**:  
  - Write-ahead logging (WAL) and ARIES-based protocols for crash recovery.
  - Redo/Undo phases to maintain transactional consistency after failures.

---

## Technologies Used

- **Java 8**: Primary programming language.
- **Maven**: Build and dependency management.
- **JUnit**: Comprehensive test suite to ensure correctness.
- **IntelliJ IDE**: Development environment with debugging tools.

---

## Running the Project

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/Matt-RFI/RookieDB.git
   cd RookieDB
2. **Setup Environment**:

- Install Java 8 and Maven.
- Open the project in IntelliJ (or your preferred IDE) as a Maven project.

3. **Run Tests**:
   ```bash
   mvn clean test
   
4. **Command Line Interface**:
   - The CLI lets you interact with the DB directly:
      ```bash
      mvn exec:java -Dexec.mainClass="edu.berkeley.cs186.database.cli.CommandLineInterface"

    From here, you can run SQL-like queries against your database.

---

## Example Usage
```bash
Database db = new Database("database-dir");

try (Transaction t1 = db.beginTransaction()) {
    Schema s = new Schema()
            .add("id", Type.intType())
            .add("firstName", Type.stringType(10))
            .add("lastName", Type.stringType(10));

    t1.createTable(s, "people");
    t1.insert("people", 1, "Alice", "Smith");
    t1.insert("people", 2, "Bob", "Jones");
    t1.commit();
}

try (Transaction t2 = db.beginTransaction()) {
    Iterator<Record> iter = t2.query("people").execute();
    while (iter.hasNext()) {
        System.out.println(iter.next());
    }
    t2.commit();
}

db.close();


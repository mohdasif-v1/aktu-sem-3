# 📘 Database Management Systems — Complete Sessional Handbook

> A single, self-contained reference covering file systems, database fundamentals, models, keys, and ER diagrams — polished and expanded for revision.

---

## 🗺️ Table of Contents

1. [Physical File vs Logical File](#part-1)
2. [Types of Files](#part-2)
3. [Properties of a Database](#part-3)
4. [Classification of Databases](#part-4)
5. [Database Languages](#part-5)
6. [Database Schema](#part-6)
7. [Advantages and Disadvantages of a Database](#part-7)
8. [Difference between DBMS and File System](#part-8)
9. [Database Models](#part-9)
10. [Bank ER Diagram — Full Worked Example](#part-10)
11. [Super Key, Candidate Key, Primary Key, Composite Key](#part-11)
12. [Generalization vs Specialization](#part-12)
13. [Important Exam Questions (Ranked)](#part-13)
14. [One-Page Revision Table](#part-14)

---

<a name="part-1"></a>
# PART 1 — DIFFERENCE BETWEEN PHYSICAL FILE AND LOGICAL FILE

## 1.1 Logical File

A **logical file** is the way data is **conceptually organized and viewed by the user or application program** — it represents *what* data looks like from the outside, independent of how it is actually stored on disk.

- Defined in terms of **records and fields** (e.g., a "Student" file with fields `RollNo`, `Name`, `Marks`)
- Represents the **user's/programmer's view** of data
- Deals with the **logical structure**: record format, field types, relationships between records
- Independent of physical storage device, block size, or storage medium

## 1.2 Physical File

A **physical file** is the way data is **actually stored on the physical storage medium** (hard disk, SSD, tape) — it represents *how* the bits and bytes are laid out.

- Defined in terms of **blocks, sectors, and tracks**
- Represents the **operating system's/storage device's view** of data
- Deals with **physical structure**: block size, storage address, file allocation method (contiguous, linked, indexed)
- Concerned with actual read/write operations, disk seek time, and storage medium characteristics

## 1.3 Worked Example

Consider a "Student" file with 1,000 records, each 100 bytes long.

| View | Description |
|------|-------------|
| **Logical view** | "The Student file has 1,000 records, each with fields RollNo (int), Name (string), Marks (float)." |
| **Physical view** | "The Student file occupies 25 disk blocks of 4 KB each, stored starting at block address 5000, using contiguous allocation." |

The **same data** is described two different ways depending on which layer you're looking at — this separation is what allows a programmer to work with data without worrying about disk mechanics, and what allows the OS/DBMS to reorganize physical storage without breaking the application's logic. This separation is called **physical data independence**.

## 1.4 Comparison Table

| Feature | Logical File | Physical File |
|---------|---------------|-----------------|
| Definition | User's/program's view of data | Actual storage layout on disk |
| Organized in terms of | Records and fields | Blocks, sectors, tracks |
| Concerned with | Data structure and meaning | Storage location and access method |
| Independence | Independent of storage device | Tied to a specific storage device |
| Who deals with it | Application programmer / DBMS user | Operating system / storage manager |
| Changeable without affecting the other? | Yes — logical structure can stay the same even if physical storage changes (data independence) | Yes — physical layout can be reorganized without changing the logical view |
| Example | "Employee file with Name, ID, Salary" | "Employee file spans blocks 100–150 on disk sector 4" |

> **Exam-ready one-liner:** *"A logical file represents how data appears to the user in terms of records and fields, while a physical file represents how that same data is actually stored on disk in terms of blocks and addresses. The mapping between the two is maintained by the DBMS/OS, enabling physical data independence."*

---

<a name="part-2"></a>
# PART 2 — TYPES OF FILES

Files can be classified based on **organization method** (how records are arranged for storage and access) and based on **content/purpose**.

## 2.1 Classification by File Organization

### (a) Sequential File Organization
Records are stored **one after another**, typically in the order they were inserted, or sorted by a key field.

- **Access:** Must read records in order — to reach the 100th record, you generally traverse the first 99 first (unless using an index)
- **Best for:** Batch processing where most/all records need to be processed (e.g., payroll processing)
- **Drawback:** Slow for random access and slow for insertions/deletions in the middle (may require reorganizing the whole file)

### (b) Heap (Pile) File Organization
Records are inserted **wherever there is free space**, with no particular order.

- **Access:** Requires a full linear scan to find a specific record
- **Best for:** Fast insertions (just append), bulk-loading data
- **Drawback:** Very slow searches — O(n) in the worst case

### (c) Hash File Organization
A **hash function** is applied to a key field to directly compute the storage address (block) of a record.

- **Access:** Very fast — O(1) average case for lookups
- **Best for:** Applications needing fast exact-match retrieval (e.g., lookup tables)
- **Drawback:** Poor for range queries (e.g., "find all records between X and Y"); collisions must be handled

### (d) Indexed Sequential File Organization (ISAM)
Records are stored sequentially, **plus an index** is maintained that maps key values to block addresses, enabling faster direct access.

- **Access:** Uses the index to jump close to the target record, then does a small local scan
- **Best for:** Applications needing both sequential *and* random access (e.g., banking systems)
- **Drawback:** Index must be maintained/updated as records are inserted or deleted — added overhead

### (e) Clustered File Organization
Two or more related tables/records that are frequently accessed together are stored **physically close together** on disk (often on the same block).

- **Best for:** Speeding up joins between frequently co-accessed tables

## 2.2 Classification by Content/Purpose (OS-level file types)

| File Type | Purpose | Example |
|-----------|---------|---------|
| **Master File** | Contains relatively permanent/reference data, updated infrequently | Employee master file with ID, Name, Department |
| **Transaction File** | Contains records of ongoing transactions/events, used to update master files | Daily sales transactions |
| **Report File** | Contains formatted data intended for printing/display, generated as output | Monthly sales report |
| **Work (Scratch/Temporary) File** | Temporary file created during processing and discarded afterward | Sort/merge intermediate file |
| **Program File** | Contains source code, object code, or executable instructions | `.exe`, `.class`, `.py` files |
| **Text File** | Stores data as human-readable characters | `.txt`, `.csv` |
| **Binary File** | Stores data in machine-readable binary format, not directly human-readable | `.bin`, `.dat`, image/audio files |

## 2.3 Comparison Table — File Organization Methods

| Organization | Insertion Speed | Search Speed | Range Query Support | Best Use Case |
|--------------|:---------------:|:------------:|:--------------------:|----------------|
| Sequential | Slow (mid-insert) | Slow (O(n)) | ✅ Excellent | Batch processing |
| Heap | ✅ Fast (append) | Slow (O(n)) | ❌ Poor | Bulk loading, logs |
| Hash | Fast | ✅ Very Fast (O(1) avg) | ❌ Poor | Exact-match lookups |
| Indexed Sequential | Moderate | Fast (via index) | ✅ Good | Banking, mixed access patterns |
| Clustered | Moderate | Fast for joins | ✅ Good (for joined data) | Frequently joined tables |

---

<a name="part-3"></a>
# PART 3 — PROPERTIES OF A DATABASE

A well-designed database exhibits the following core properties — a frequently tested "list and explain" question.

## 3.1 Self-Describing Nature
A database contains not just the data itself but also a description of the data (metadata) — this metadata is stored in the **system catalog/data dictionary**, describing table structures, field names, types, and constraints.

## 3.2 Insulation Between Programs and Data (Data Independence)
The structure of data is stored separately from the application programs that access it. Changing the internal storage structure does not require changing the application programs (this is the DBMS analog of the logical/physical file separation from Part 1).

- **Logical Data Independence:** Ability to change the logical schema (e.g., add a new field) without affecting application programs
- **Physical Data Independence:** Ability to change the physical storage structure without affecting the logical schema

## 3.3 Support for Multiple Views of Data
Different users can have different, tailored views of the same underlying database — e.g., a bank teller sees only account balances, while a bank manager sees loan details too, both drawn from the same underlying data.

## 3.4 Sharing of Data and Multi-User Transaction Processing
A database allows **multiple users to access the same data concurrently**, with the DBMS ensuring **concurrency control** so simultaneous transactions don't corrupt data (e.g., two people booking the last train seat simultaneously).

## 3.5 Data Integrity
The database enforces **rules/constraints** (e.g., primary key uniqueness, foreign key references, data type checks) to ensure data remains accurate and consistent.

## 3.6 Data Security
Access to data is restricted through **authentication and authorization** — different users are granted different permission levels (read/write/execute) on different parts of the database.

## 3.7 Minimal Redundancy / Controlled Data Redundancy
Through **normalization**, a database minimizes duplicate data storage, reducing the risk of inconsistency (though *some* controlled redundancy may be intentionally kept for performance).

## 3.8 Persistent Storage
Data in a database, once inserted, persists beyond the lifetime of the program that created it — it survives program termination, and even system crashes (via backup/recovery mechanisms).

### ⭐ Model Exam Answer
> "A database is self-describing (it stores metadata alongside data), provides data independence between programs and storage, supports multiple simultaneous user views, allows controlled multi-user sharing with concurrency control, enforces data integrity and security, minimizes redundancy through normalization, and guarantees persistence of data across program executions."

---

<a name="part-4"></a>
# PART 4 — CLASSIFICATION OF DATABASES

Databases can be classified along several different dimensions.

## 4.1 By Data Model

| Type | Description | Example |
|------|-------------|---------|
| **Relational Database (RDBMS)** | Data stored in tables (rows/columns) with relationships via keys | MySQL, PostgreSQL, Oracle |
| **Hierarchical Database** | Data organized in a tree-like parent-child structure | IBM IMS |
| **Network Database** | Data organized as a graph, allowing many-to-many relationships via pointers | IDMS |
| **Object-Oriented Database** | Data stored as objects, as in object-oriented programming | ObjectDB, db4o |
| **NoSQL Database** | Non-relational, flexible schema, built for scale | MongoDB (document), Cassandra (column), Redis (key-value), Neo4j (graph) |

## 4.2 By Number of Sites (Distribution)

| Type | Description |
|------|-------------|
| **Centralized Database** | All data stored and managed at a single physical location/server |
| **Distributed Database** | Data spread across multiple physical locations/servers, appearing as a single logical database |

## 4.3 By Number of Users

| Type | Description |
|------|-------------|
| **Single-user Database** | Supports only one user accessing the database at a time |
| **Multi-user Database** | Supports multiple concurrent users; further divided into: |
| — Client-Server Database | Central server handles requests from multiple client machines |
| — Distributed Database | Data spread across multiple sites (see above) |

## 4.4 By Purpose/Use Case

| Type | Description | Example |
|------|-------------|---------|
| **Operational Database (OLTP)** | Handles day-to-day transactional operations, frequent reads/writes | Banking transaction systems |
| **Data Warehouse (OLAP)** | Stores historical, aggregated data for analysis and reporting | Business intelligence systems |
| **Data Mart** | A focused subset of a data warehouse for a specific department | Sales data mart |

## 4.5 By Content Type

| Type | Description |
|------|-------------|
| **General-purpose Database** | Stores structured business data of various kinds |
| **Multimedia Database** | Stores images, audio, video |
| **Spatial Database** | Stores geographic/spatial data (maps, coordinates) |
| **Temporal Database** | Stores time-based/historical data with time-stamped versions |

---

<a name="part-5"></a>
# PART 5 — DATABASE LANGUAGES

DBMS languages are broadly divided into the following categories — this is a **very frequently tested** classification.

## 5.1 DDL — Data Definition Language

Used to **define, modify, or delete the structure** of database objects (tables, schemas, indexes) — deals with the *schema*, not the data itself.

**Commands:** `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`

```sql
CREATE TABLE Student (RollNo INT PRIMARY KEY, Name VARCHAR(50));
ALTER TABLE Student ADD Marks INT;
DROP TABLE Student;
```

## 5.2 DML — Data Manipulation Language

Used to **insert, update, delete, and retrieve** data stored within the database structure.

**Commands:** `SELECT`, `INSERT`, `UPDATE`, `DELETE`

```sql
INSERT INTO Student VALUES (1, 'Asif', 90);
UPDATE Student SET Marks = 95 WHERE RollNo = 1;
DELETE FROM Student WHERE RollNo = 1;
SELECT * FROM Student;
```

> **Sub-classification:** DML is sometimes further split into:
> - **Procedural DML** — user specifies *what* data is needed and *how* to get it (step-by-step)
> - **Non-procedural (Declarative) DML** — user specifies *what* data is needed without specifying *how* to retrieve it (SQL is primarily this kind)

## 5.3 DCL — Data Control Language

Used to **control access/permissions** to data within the database.

**Commands:** `GRANT`, `REVOKE`

```sql
GRANT SELECT, INSERT ON Student TO user1;
REVOKE INSERT ON Student FROM user1;
```

## 5.4 TCL — Transaction Control Language

Used to **manage transactions** within the database, ensuring the ACID properties.

**Commands:** `COMMIT`, `ROLLBACK`, `SAVEPOINT`, `SET TRANSACTION`

```sql
BEGIN TRANSACTION;
UPDATE Account SET Balance = Balance - 500 WHERE AccNo = 1;
UPDATE Account SET Balance = Balance + 500 WHERE AccNo = 2;
COMMIT;
```

## 5.5 DQL — Data Query Language (sometimes treated as a subset of DML)

Used **exclusively for querying/retrieving** data.

**Command:** `SELECT`

Some textbooks classify `SELECT` as its own category (DQL) rather than lumping it into DML, since it doesn't *modify* data — mention both classifications if asked, as syllabi vary.

## ⭐ Summary Table

| Language | Full Form | Purpose | Key Commands |
|----------|-----------|---------|----------------|
| DDL | Data Definition Language | Define/modify schema structure | CREATE, ALTER, DROP, TRUNCATE |
| DML | Data Manipulation Language | Manipulate data within tables | INSERT, UPDATE, DELETE, (SELECT) |
| DQL | Data Query Language | Retrieve/query data | SELECT |
| DCL | Data Control Language | Control access/permissions | GRANT, REVOKE |
| TCL | Transaction Control Language | Manage transactions | COMMIT, ROLLBACK, SAVEPOINT |

---

<a name="part-6"></a>
# PART 6 — DATABASE SCHEMA

## 6.1 What Is a Schema?

A **database schema** is the **overall logical design/structure** of a database — it describes the tables, fields, data types, relationships, and constraints, **without containing the actual data itself**. Think of it as the *blueprint*, while the actual stored data is the *instance*.

## 6.2 Schema vs Instance

| Aspect | Schema | Instance |
|--------|--------|----------|
| Definition | The structure/design of the database | The actual data stored at a given moment |
| Changes | Changes rarely (only during redesign) | Changes constantly (every insert/update/delete) |
| Analogy | The blueprint of a house | The furniture and people currently inside the house |

## 6.3 Three-Schema Architecture (ANSI/SPARC Architecture)

This is the standard model describing a database at **three levels of abstraction** — a classic, very frequently examined diagram-based topic.

```
        ┌─────────────────────────────────────┐
        │         EXTERNAL LEVEL                │
        │   (Multiple User Views / Sub-schemas) │
        │   View 1    View 2    View 3          │
        └──────────────┬────────────────────────┘
                        │  External/Conceptual Mapping
        ┌──────────────▼────────────────────────┐
        │        CONCEPTUAL LEVEL                │
        │  (Community/Global logical schema —    │
        │   describes entities, relationships,   │
        │   constraints for the ENTIRE database) │
        └──────────────┬────────────────────────┘
                        │  Conceptual/Internal Mapping
        ┌──────────────▼────────────────────────┐
        │         INTERNAL LEVEL                 │
        │  (Physical schema — describes how data │
        │   is actually stored: files, indexes,  │
        │   access paths, block structure)        │
        └─────────────────────────────────────────┘
```

### (a) External Schema (View Level)
Describes **how individual users see** the database — different users/applications can have different, customized views showing only the data relevant to them, hiding the rest.

### (b) Conceptual Schema (Logical Level)
Describes the **overall logical structure** of the *entire* database for the community of users — all entities, attributes, relationships, and constraints — independent of physical storage details and independent of any single user's view.

### (c) Internal Schema (Physical Level)
Describes **how data is physically stored** — file organization, indexing methods, access paths, and storage allocation.

## 6.4 Why Three Levels? — Data Independence

The three-schema architecture is the foundation for two types of data independence:

| Independence Type | Definition |
|---------------------|------------|
| **Logical Data Independence** | Ability to change the *conceptual schema* (e.g., add a new entity/attribute) without needing to change *external schemas* or application programs |
| **Physical Data Independence** | Ability to change the *internal schema* (e.g., switch storage structure, reorganize files) without needing to change the *conceptual schema* |

> **Exam tip:** Logical data independence is considered "harder to achieve" than physical data independence, because changes to the logical structure are more likely to ripple outward and affect how applications interact with the data.

---

<a name="part-7"></a>
# PART 7 — ADVANTAGES AND DISADVANTAGES OF A DATABASE

## 7.1 Advantages

| # | Advantage | Explanation |
|---|-----------|--------------|
| 1 | **Reduced Data Redundancy** | Normalization minimizes duplicate storage of the same data |
| 2 | **Data Consistency** | Since redundancy is reduced, updates need to happen in fewer places, keeping data consistent |
| 3 | **Data Sharing** | Multiple users/applications can access the same data concurrently |
| 4 | **Data Integrity** | Constraints (primary key, foreign key, check constraints) ensure data accuracy |
| 5 | **Data Security** | Authentication and authorization mechanisms restrict access to sensitive data |
| 6 | **Backup and Recovery** | DBMS provides built-in mechanisms to recover data after a crash/failure |
| 7 | **Data Independence** | Applications are insulated from changes to physical storage structure |
| 8 | **Concurrent Access Control** | DBMS manages simultaneous transactions safely, preventing conflicts |
| 9 | **Efficient Query Processing** | Query optimizers and indexing enable fast data retrieval even on huge datasets |
| 10 | **Reduced Application Development Time** | Built-in features (querying, transactions, security) mean developers don't have to reimplement these from scratch |

## 7.2 Disadvantages

| # | Disadvantage | Explanation |
|---|----------------|--------------|
| 1 | **High Cost** | DBMS software, hardware, and skilled personnel (DBAs) can be expensive |
| 2 | **Complexity** | Designing, implementing, and maintaining a database requires specialized expertise |
| 3 | **Performance Overhead** | The additional layers (concurrency control, security checks, integrity constraints) can slow down simple operations compared to direct file access |
| 4 | **Single Point of Failure Risk** | If the DBMS or central server fails, all dependent applications can be affected (mitigated via backups/replication, but adds more complexity) |
| 5 | **Storage/Hardware Requirements** | DBMS software itself, plus indexes and logs, requires significant additional storage beyond just the raw data |
| 6 | **Migration Difficulty** | Once a large system is built on a specific DBMS, switching to a different one can be costly and time-consuming |

> **Exam-ready summary:** *"While a DBMS offers major advantages in reducing redundancy, ensuring consistency, and enabling secure multi-user access, these benefits come at the cost of higher complexity, licensing/hardware expense, and some performance overhead compared to simpler file-based systems — a tradeoff generally well worth it for any application beyond trivial scale."*

---

<a name="part-8"></a>
# PART 8 — DIFFERENCE BETWEEN DBMS AND FILE SYSTEM

One of the **highest-frequency comparison questions** in any DBMS sessional.

| Feature | File System | DBMS |
|---------|--------------|------|
| **Data Redundancy** | High — same data often duplicated across multiple files | Low — normalization minimizes duplication |
| **Data Consistency** | Difficult to maintain — updates to duplicated data can go out of sync | Enforced automatically through constraints and normalized design |
| **Data Sharing** | Difficult — files are typically tied to specific applications | Easy — multiple applications/users can share the same data |
| **Data Security** | Minimal — often just OS-level file permissions | Strong — fine-grained authentication and authorization at table/row/column level |
| **Data Integrity** | Must be manually enforced by each application | Enforced by the DBMS via constraints (primary key, foreign key, check, etc.) |
| **Concurrent Access** | Poor — simultaneous access can easily corrupt data | Managed via concurrency control mechanisms (locking, timestamps) |
| **Backup and Recovery** | Manual, application-specific, often unreliable | Built-in, automated backup and crash recovery mechanisms |
| **Data Independence** | Little to none — application code is tightly coupled to file structure | Strong — logical and physical data independence supported |
| **Query Capability** | Limited — usually requires custom code to search/filter data | Powerful — supports declarative query languages (SQL) |
| **Atomicity of Operations** | Not guaranteed — a crash mid-update can leave data in an inconsistent state | Guaranteed via transactions (ACID properties) |
| **Cost & Complexity** | Simple, low-cost, easy to set up | More complex and costly, but scales far better |
| **Examples** | Flat files, `.txt`, `.csv`, storing data in plain OS files | MySQL, Oracle, PostgreSQL, SQL Server |

### ⭐ Model Exam Answer
> "A file system stores data as a collection of independent files, typically tied to specific applications, offering little in the way of data sharing, integrity, or security — data redundancy and inconsistency are common problems. A DBMS, in contrast, provides a centralized, structured approach to storing data, enforcing integrity constraints, supporting concurrent multi-user access, providing powerful query languages, and guaranteeing transactional consistency through ACID properties — at the cost of greater complexity and overhead."

---

<a name="part-9"></a>
# PART 9 — DATABASE MODELS

A **database model** (or data model) defines the logical structure of a database — how data is organized, stored, and manipulated. This is a core conceptual topic.

## 9.1 Hierarchical Model

Data is organized as a **tree structure**, with each record having exactly **one parent** (except the root) and **potentially many children** — a strict one-to-many relationship at every level.

```
                Company
                   │
        ┌──────────┴──────────┐
     Department A         Department B
        │                      │
   ┌────┴────┐            ┌────┴────┐
Employee1  Employee2   Employee3  Employee4
```

- **Pros:** Simple and fast for one-to-many relationships; efficient traversal
- **Cons:** Difficult to represent many-to-many relationships; rigid structure; deleting a parent can force deletion of children
- **Example system:** IBM's Information Management System (IMS)

## 9.2 Network Model

Generalizes the hierarchical model by allowing a record to have **multiple parents**, represented as a **graph** rather than a strict tree — relationships are implemented via pointers.

```
   Student ────┐          ┌──── Course
       │        \        /         │
       │         Enrollment        │
       │        /        \         │
   Student2 ───┘          └─── Course2
```

- **Pros:** Can represent many-to-many relationships naturally; more flexible than hierarchical
- **Cons:** Complex structure; navigation requires knowing the pointer paths, making it hard to use/maintain
- **Example system:** Integrated Database Management System (IDMS)

## 9.3 Relational Model ⭐ (Most Important)

Data is organized into **tables (relations)** consisting of **rows (tuples)** and **columns (attributes)**. Relationships between tables are established through **keys** (primary key / foreign key), not physical pointers.

```
┌────────────────────────────┐        ┌───────────────────────────┐
│         Student            │        │         Enrollment          │
├─────────┬─────────┬────────┤        ├─────────┬──────────────────┤
│ RollNo  │  Name   │ Branch │        │ RollNo  │   CourseCode     │
│  (PK)   │         │        │◄───────┤  (FK)   │                  │
├─────────┼─────────┼────────┤        ├─────────┼──────────────────┤
│   1     │  Asif   │  CSE   │        │   1     │     CS101        │
│   2     │  Riya   │  ECE   │        │   1     │     CS102        │
└─────────┴─────────┴────────┘        └─────────┴──────────────────┘
```

- **Pros:** Simple, intuitive, mathematically grounded (based on set theory/relational algebra), supports powerful declarative querying (SQL), strong data independence
- **Cons:** Can be less efficient than specialized models for certain complex relationship-heavy data (e.g., deeply hierarchical or graph-like data)
- **Example systems:** MySQL, PostgreSQL, Oracle, SQL Server

## 9.4 Entity-Relationship (ER) Model

A **conceptual/design-level model** (not directly implemented for storage) used to represent real-world entities and their relationships graphically, typically as a step before converting to the relational model.

**Core components:** Entities, Attributes, Relationships (see Part 10 for a full worked ER diagram)

## 9.5 Object-Oriented Model

Data is represented as **objects**, similar to object-oriented programming, encapsulating both data (attributes) and behavior (methods) together, and supporting concepts like inheritance and polymorphism directly within the database.

- **Pros:** Natural fit for applications already built with OOP; supports complex data types directly
- **Cons:** Less mature tooling/standardization compared to relational databases; smaller ecosystem
- **Example systems:** ObjectDB, db4o

## 9.6 NoSQL Models (Modern, Non-Relational)

| Sub-type | Structure | Example |
|----------|-----------|---------|
| **Document-based** | Data stored as JSON/BSON-like documents | MongoDB |
| **Key-Value** | Data stored as simple key-value pairs | Redis |
| **Column-based** | Data stored by columns rather than rows, for fast analytical queries | Cassandra |
| **Graph-based** | Data stored as nodes and edges, ideal for highly connected data | Neo4j |

## 9.7 Comparison Table

| Model | Structure | Relationship Handling | Flexibility | Example |
|-------|-----------|--------------------------|-------------|---------|
| Hierarchical | Tree | One-to-many only | Low | IBM IMS |
| Network | Graph | Many-to-many (via pointers) | Medium | IDMS |
| Relational | Tables | Via keys (PK/FK) | High | MySQL, Oracle |
| ER Model | Conceptual diagram | Entities + relationships | N/A (design tool) | — |
| Object-Oriented | Objects | Via object references | High (for OOP apps) | ObjectDB |
| NoSQL | Varies (doc/key-value/column/graph) | Varies | Very High (schema-less) | MongoDB, Redis |

---

<a name="part-10"></a>
# PART 10 — BANK ER DIAGRAM (Full Worked Example) ⭐⭐⭐

A classic textbook ER modeling example — designing a simplified **Bank Database**.

## 10.1 Requirements (Problem Statement)

- A **Branch** has a unique branch code, name, and city.
- A **Customer** has a unique customer ID, name, address, and phone number.
- A **Account** has a unique account number, account type (savings/current), and balance.
- A **Loan** has a unique loan number, loan type, and amount.
- A **Customer** can open **many Accounts**, and each **Account** belongs to exactly **one Customer** (simplified — real banks allow joint accounts, a many-to-many case).
- A **Customer** can take **many Loans**, and each **Loan** is taken by exactly **one Customer**.
- A **Branch** manages **many Accounts** and **many Loans**, but each Account/Loan belongs to exactly **one Branch**.
- An **Employee** works at exactly **one Branch**, and a **Branch** has **many Employees**.

## 10.2 Identifying Entities and Attributes

| Entity | Attributes | Primary Key |
|--------|-----------|--------------|
| **Branch** | BranchCode, BranchName, City | BranchCode |
| **Customer** | CustomerID, Name, Address, Phone | CustomerID |
| **Account** | AccountNo, AccountType, Balance | AccountNo |
| **Loan** | LoanNo, LoanType, Amount | LoanNo |
| **Employee** | EmployeeID, Name, Salary | EmployeeID |

## 10.3 Identifying Relationships and Cardinalities

| Relationship | Between | Cardinality |
|--------------|---------|--------------|
| **Opens** | Customer — Account | One-to-Many (1 Customer : M Accounts) |
| **Takes** | Customer — Loan | One-to-Many (1 Customer : M Loans) |
| **Maintains** | Branch — Account | One-to-Many (1 Branch : M Accounts) |
| **Grants** | Branch — Loan | One-to-Many (1 Branch : M Loans) |
| **Works_At** | Employee — Branch | Many-to-One (M Employees : 1 Branch) |

## 10.4 Full ER Diagram (Text Representation)

```
                                  ┌───────────────┐
                                  │    BRANCH     │
                                  │───────────────│
                                  │ BranchCode(PK)│
                                  │ BranchName    │
                                  │ City          │
                                  └───┬───┬───┬───┘
                                      │   │   │
                       ┌──────────────┘   │   └───────────────┐
                       │ 1                │ 1                 │ 1
                  Maintains            Grants              Works_At
                       │ M                │ M                 │ M
                       ▼                  ▼                   ▼
              ┌────────────────┐  ┌────────────────┐  ┌────────────────┐
              │    ACCOUNT     │  │      LOAN      │  │    EMPLOYEE    │
              │────────────────│  │────────────────│  │────────────────│
              │ AccountNo (PK) │  │ LoanNo (PK)    │  │ EmployeeID(PK) │
              │ AccountType    │  │ LoanType       │  │ Name           │
              │ Balance        │  │ Amount         │  │ Salary         │
              └───────┬────────┘  └───────┬────────┘  └────────────────┘
                      │ M                 │ M
                   Opens                Takes
                      │ 1                 │ 1
                      ▼                   ▼
              ┌─────────────────────────────────────┐
              │              CUSTOMER                 │
              │────────────────────────────────────── │
              │ CustomerID (PK)                       │
              │ Name                                   │
              │ Address                                │
              │ Phone                                  │
              └─────────────────────────────────────────┘
```

**Reading the diagram:**
- **Rectangles** represent entities (Branch, Account, Loan, Employee, Customer)
- **Diamonds** (shown here as relationship labels between arrows) represent relationships (Maintains, Grants, Works_At, Opens, Takes)
- The **"1" and "M"** labels on each side of a relationship denote cardinality — e.g., "1" near Branch and "M" near Account in the *Maintains* relationship means **one Branch maintains many Accounts**, but each Account is maintained by only **one Branch**.

## 10.5 Standard ER Diagram Notation Reference

| Symbol | Meaning |
|--------|---------|
| **Rectangle** | Entity (a real-world object, e.g., Customer, Account) |
| **Ellipse/Oval** | Attribute (a property of an entity, e.g., Name, Balance) |
| **Double Ellipse** | Multi-valued attribute (an attribute that can have multiple values, e.g., Phone Numbers) |
| **Dashed Ellipse** | Derived attribute (computed from other data, e.g., Age derived from DOB) |
| **Diamond** | Relationship (an association between two or more entities, e.g., Opens, Takes) |
| **Underlined Attribute** | Primary key attribute |
| **Double Rectangle** | Weak entity (an entity that cannot be uniquely identified without a related "owner" entity) |
| **Double Diamond** | Identifying relationship (connects a weak entity to its owner entity) |
| **Lines** | Connect entities to their attributes, and entities to relationships |

## 10.6 Deriving Relational Tables from the ER Diagram

Converting the ER diagram above into actual relational schema (a common follow-up question):

```sql
Branch(BranchCode PK, BranchName, City)

Customer(CustomerID PK, Name, Address, Phone)

Account(AccountNo PK, AccountType, Balance,
        BranchCode FK REFERENCES Branch,
        CustomerID FK REFERENCES Customer)

Loan(LoanNo PK, LoanType, Amount,
     BranchCode FK REFERENCES Branch,
     CustomerID FK REFERENCES Customer)

Employee(EmployeeID PK, Name, Salary,
         BranchCode FK REFERENCES Branch)
```

> **Key conversion rule to remember:** In a **one-to-many** relationship, the foreign key is placed on the "many" side, referencing the primary key of the "one" side. Here, since one Branch relates to many Accounts, `Account` gets the `BranchCode` foreign key (not the other way around).

---

<a name="part-11"></a>
# PART 11 — SUPER KEY, CANDIDATE KEY, PRIMARY KEY, COMPOSITE KEY

One of the **most important key-related conceptual questions** — know exact definitions and be ready to give examples for each.

## 11.1 Super Key

A **super key** is **any set of one or more attributes** that, taken together, can **uniquely identify a tuple (row)** in a relation. A super key may contain *extra, unnecessary* attributes beyond what's minimally needed for uniqueness.

**Example:** In a `Student(RollNo, Name, Email, Phone)` table:
```
{RollNo}                        → Super Key
{RollNo, Name}                  → Super Key (redundant — Name isn't needed for uniqueness)
{Email}                         → Super Key (if emails are unique)
{RollNo, Name, Email, Phone}    → Super Key (the entire set is trivially a super key)
```

> Every relation has at least one super key: the set of **all** its attributes.

## 11.2 Candidate Key

A **candidate key** is a **minimal super key** — a super key with **no unnecessary/redundant attributes**. Removing even one attribute from a candidate key would cause it to lose its uniqueness property.

**Example:** From the super keys above:
```
{RollNo}   → Candidate Key (minimal — no attribute can be removed)
{Email}    → Candidate Key (minimal, and also uniquely identifies rows)
{RollNo, Name} → NOT a candidate key (Name is redundant, since RollNo alone suffices)
```

A table can have **multiple candidate keys** — in this example, both `{RollNo}` and `{Email}` are candidate keys.

## 11.3 Primary Key

A **primary key** is the **one candidate key chosen by the database designer** to be the main, official identifier for a table. Every table has **exactly one** primary key (though it may be composed of multiple attributes).

**Properties of a primary key:**
- Must be **unique** for every row
- Must **not be NULL** (this is called the *entity integrity constraint*)
- Should be chosen for **stability** — a value that rarely or never changes

**Example:** Between `{RollNo}` and `{Email}` (both candidate keys), the designer might choose `RollNo` as the **Primary Key** because it's a more stable, simpler identifier (email addresses can change).

The remaining, unchosen candidate key(s) — here, `{Email}` — are called **Alternate Keys** (or secondary keys).

## 11.4 Composite Key

A **composite key** (also called a **compound key**) is a **candidate key or primary key made up of two or more attributes**, where **no single attribute alone** is sufficient to uniquely identify a row — the *combination* is needed.

**Example:** In an `Enrollment(RollNo, CourseCode, EnrollmentDate)` table, where a student can enroll in multiple courses:
```
{RollNo}       → alone, does NOT uniquely identify a row (a student has multiple enrollments)
{CourseCode}   → alone, does NOT uniquely identify a row (a course has multiple students)
{RollNo, CourseCode} → uniquely identifies a row → Composite Key
```

This composite key would typically be chosen as the **Composite Primary Key** for the `Enrollment` table.

## 11.5 Other Related Key Types (Bonus, Often Asked Together)

| Key Type | Definition |
|----------|------------|
| **Foreign Key** | An attribute in one table that references the primary key of another table, establishing a relationship between the two tables |
| **Alternate Key** | A candidate key that was *not* chosen as the primary key |
| **Unique Key** | A constraint ensuring all values in a column are distinct, similar to a primary key but *can* allow one NULL value (in most RDBMSs) |
| **Surrogate Key** | An artificially generated key (e.g., an auto-incrementing ID) with no business meaning, used purely for uniqueness |

## 11.6 Comparison Table — Super Key vs Candidate Key vs Primary Key vs Composite Key

| Feature | Super Key | Candidate Key | Primary Key | Composite Key |
|---------|-----------|----------------|---------------|-----------------|
| **Definition** | Any attribute set that uniquely identifies a row | Minimal super key (no redundant attributes) | The one candidate key selected as the main identifier | A candidate/primary key made of 2+ attributes |
| **Minimality** | ❌ Not necessarily minimal | ✅ Always minimal | ✅ Always minimal | ✅ Minimal, but needs multiple attributes |
| **Uniqueness per table** | Multiple possible | Multiple possible | Exactly **one** per table | Can be the primary key or a candidate key |
| **Can contain NULL?** | Depends on attributes | Depends on attributes | ❌ Never (entity integrity) | ❌ Never, if it's the primary key |
| **Example** | {RollNo, Name} | {RollNo}, {Email} | {RollNo} (chosen) | {RollNo, CourseCode} |

### ⭐ Model Exam Answer (Relationship Summary)
> "A super key is any combination of attributes that uniquely identifies a tuple, possibly with redundant attributes. A candidate key is a minimal super key — removing any attribute would break uniqueness. Among all candidate keys, the database designer selects one to serve as the primary key, which must be unique and non-null for every row; the remaining candidate keys become alternate keys. A composite key is simply a candidate or primary key that requires two or more attributes together to achieve uniqueness, because no single attribute alone is sufficient."

### Visual Hierarchy

```
                    SUPER KEYS
              (all possible unique combos)
                        │
                        │  (remove redundant attributes)
                        ▼
                  CANDIDATE KEYS
             (minimal super keys — could be
              multiple per table)
                        │
                        │  (designer picks ONE)
                        ▼
                   PRIMARY KEY
           (the chosen official identifier)

   Note: any Candidate Key OR Primary Key can also be
   a COMPOSITE KEY if it requires multiple attributes.
```

---

<a name="part-12"></a>
# PART 12 — GENERALIZATION VS SPECIALIZATION

Both are ER-modeling techniques used to express **hierarchical relationships between entities**, particularly useful when entities share some attributes but differ in others (similar in spirit to inheritance in OOP).

## 12.1 Generalization (Bottom-Up Approach)

**Generalization** is the process of **combining two or more lower-level (child) entities into a single higher-level (parent/super) entity**, based on their **common attributes**. It works **bottom-up** — you start with specific entities and generalize them into a broader category.

**Example:** Suppose you have two separate entities:
```
Car(RegNo, Model, NumDoors)
Truck(RegNo, Model, LoadCapacity)
```

Both share `RegNo` and `Model` as common attributes. Generalization combines them into a higher-level entity:
```
Vehicle(RegNo, Model)
   ├── Car(NumDoors)
   └── Truck(LoadCapacity)
```

Here, `Vehicle` is the **generalized (super) entity**.

## 12.2 Specialization (Top-Down Approach)

**Specialization** is the reverse process — it takes a **higher-level (parent) entity** and divides it into **two or more lower-level (child) entities** based on **distinguishing characteristics**. It works **top-down** — you start with a general entity and specialize it into more specific sub-types.

**Example:** Suppose you start with a general entity:
```
Employee(EmpID, Name, Salary)
```

You notice that employees fall into distinct sub-types with extra specific attributes:
```
Employee(EmpID, Name, Salary)
   ├── Manager(TeamSize)
   ├── Engineer(Specialization)
   └── Salesperson(SalesTarget)
```

Here, `Employee` is **specialized** into `Manager`, `Engineer`, and `Salesperson`.

## 12.3 Key Conceptual Similarity

Both generalization and specialization ultimately produce the **same kind of hierarchical (superclass/subclass) structure** in the ER diagram — the difference lies purely in the **direction of the design process**, not the final result.

```
        Generalization: Car + Truck  ──►  Vehicle   (bottom-up, merging)
        Specialization: Employee     ──►  Manager, Engineer, Salesperson   (top-down, splitting)
```

## 12.4 ER Diagram Notation for Generalization/Specialization

```
                    ┌───────────┐
                    │  Vehicle  │   (Superclass / Generalized entity)
                    │ RegNo     │
                    │ Model     │
                    └─────┬─────┘
                          │
                          ▽   (triangle = ISA / generalization symbol)
                ┌─────────┴─────────┐
                │                   │
          ┌─────▼─────┐      ┌──────▼──────┐
          │    Car     │      │    Truck    │   (Subclasses / Specialized entities)
          │ NumDoors   │      │ LoadCapacity│
          └────────────┘      └─────────────┘
```

The **triangle symbol (▽)**, often labeled "ISA" (Is-A), represents the generalization/specialization relationship between the superclass and its subclasses.

## 12.5 Constraints on Specialization/Generalization

Two important sub-classifications, often asked as a follow-up:

### (a) Disjoint vs Overlapping
| Type | Meaning |
|------|---------|
| **Disjoint** | An entity instance can belong to **only one** subclass at a time (e.g., an Employee is either a Manager OR an Engineer, not both) |
| **Overlapping** | An entity instance **can belong to multiple** subclasses simultaneously (e.g., a person could be both a Student AND an Employee) |

### (b) Total vs Partial Participation
| Type | Meaning |
|------|---------|
| **Total (Mandatory)** | Every instance of the superclass **must** belong to at least one subclass |
| **Partial (Optional)** | Some instances of the superclass **may not** belong to any subclass |

## 12.6 Comparison Table — Generalization vs Specialization

| Feature | Generalization | Specialization |
|---------|------------------|-------------------|
| **Direction** | Bottom-up | Top-down |
| **Process** | Combines multiple lower-level entities into one higher-level entity | Divides one higher-level entity into multiple lower-level entities |
| **Starting point** | Two or more specific (child) entities | One general (parent) entity |
| **Basis** | Common attributes/features shared across entities | Distinguishing/unique attributes within a general entity |
| **Goal** | Reduce redundancy by abstracting shared features | Add detail/specificity to a general concept |
| **Example** | Car + Truck → Vehicle | Employee → Manager, Engineer, Salesperson |
| **Also known as** | Bottom-up design, abstraction | Top-down design, inheritance-like division |

### ⭐ Model Exam Answer
> "Generalization is a bottom-up process that combines two or more entities sharing common attributes into a single generalized superclass entity, reducing redundancy. Specialization is the reverse, top-down process, where a general entity is divided into more specific subclass entities based on distinguishing attributes. Both result in the same ISA hierarchical structure in the final ER diagram — the difference is purely in the direction of the design thought process, not the outcome."

---

<a name="part-13"></a>
# PART 13 — IMPORTANT EXAM QUESTIONS (RANKED BY PRIORITY)

## 🔴 Very High Priority

**Q1. Differentiate between physical file and logical file.**
→ Cover: definitions, worked example, full comparison table (Part 1).

**Q2. Explain the different types of file organization.**
→ Cover: Sequential, Heap, Hash, Indexed Sequential, Clustered — with pros/cons of each (Part 2).

**Q3. List and explain the properties of a database.**
→ Cover: Self-describing nature, data independence, multiple views, sharing/concurrency, integrity, security, minimal redundancy, persistence (Part 3).

**Q4. Explain the classification of databases.**
→ Cover: by data model, by number of sites, by number of users, by purpose (OLTP/OLAP) (Part 4).

**Q5. Explain the different database languages (DDL, DML, DCL, TCL) with examples.**
→ Cover: full definitions + SQL command examples for each category (Part 5).

**Q6. Explain database schema and the three-schema architecture.**
→ Cover: schema vs instance, External/Conceptual/Internal levels, logical & physical data independence (Part 6).

**Q7. Differentiate between DBMS and File System.**
→ Cover: the full comparison table — redundancy, consistency, sharing, security, integrity, concurrency, backup, cost (Part 8).

**Q8. Draw and explain an ER diagram for a Bank database.**
→ Cover: entities, attributes, relationships, cardinalities, the full diagram, and conversion to relational tables (Part 10).

**Q9. Define super key, candidate key, primary key, and composite key with examples. Differentiate between them.**
→ Cover: definitions, worked example on a single table, comparison table, hierarchy diagram (Part 11).

**Q10. Differentiate between generalization and specialization with examples.**
→ Cover: definitions, direction (bottom-up vs top-down), worked examples, ISA notation, disjoint/overlapping and total/partial constraints (Part 12).

## 🟠 Medium Priority

**Q11.** Explain the advantages and disadvantages of a DBMS (Part 7).
**Q12.** Explain the different database models (hierarchical, network, relational, object-oriented, NoSQL) (Part 9).
**Q13.** What are the different types of files based on content (master, transaction, report, work files)? (Part 2.2)
**Q14.** Explain the standard ER diagram notations (rectangle, ellipse, diamond, etc.) (Part 10.5)

## 🟡 Good to Know

**Q15.** What is the difference between a foreign key, alternate key, and surrogate key? (Part 11.5)
**Q16.** Explain disjoint vs overlapping and total vs partial participation constraints in specialization (Part 12.5).
**Q17.** Why is logical data independence considered harder to achieve than physical data independence? (Part 6.4)

---

<a name="part-14"></a>
# PART 14 — ONE-PAGE REVISION TABLE

A condensed, at-a-glance summary of the entire handbook — ideal for last-minute revision.

| Topic | Core Takeaway |
|-------|----------------|
| **Physical vs Logical File** | Logical = user's record/field view; Physical = actual disk blocks/storage layout |
| **File Organization Types** | Sequential (ordered, slow random access), Heap (fast insert, slow search), Hash (O(1) lookup, poor range queries), Indexed Sequential (best of both), Clustered (co-located related data) |
| **Database Properties** | Self-describing, data independence, multiple views, multi-user sharing, integrity, security, minimal redundancy, persistence |
| **Database Classification** | By model (relational/NoSQL/etc.), by sites (centralized/distributed), by users, by purpose (OLTP/OLAP) |
| **Database Languages** | DDL (structure: CREATE/ALTER/DROP), DML (data: INSERT/UPDATE/DELETE), DQL (query: SELECT), DCL (permissions: GRANT/REVOKE), TCL (transactions: COMMIT/ROLLBACK) |
| **Database Schema** | Schema = blueprint (rarely changes); Instance = actual data (changes constantly). Three levels: External, Conceptual, Internal |
| **DBMS Advantages** | Less redundancy, more consistency, better security, concurrency control, backup/recovery, data independence |
| **DBMS Disadvantages** | Higher cost, complexity, performance overhead, migration difficulty |
| **DBMS vs File System** | DBMS wins on redundancy, consistency, sharing, security, integrity, concurrency, backup, querying — at the cost of complexity/cost |
| **Database Models** | Hierarchical (tree), Network (graph), Relational (tables + keys) ⭐, ER Model (design tool), Object-Oriented, NoSQL (doc/key-value/column/graph) |
| **Bank ER Diagram** | Entities: Branch, Customer, Account, Loan, Employee. Relationships: Opens, Takes, Maintains, Grants, Works_At — mostly 1:M. FK goes on the "many" side |
| **Key Hierarchy** | Super Key (any unique combo) ⊇ Candidate Key (minimal unique combo) → Primary Key (the chosen one). Composite Key = any key needing 2+ attributes |
| **Generalization vs Specialization** | Generalization = bottom-up (merge similar entities into one superclass); Specialization = top-down (split one entity into subclasses). Both use the ISA (▽) notation |

---

**Study strategy:** Prioritize the ⭐-marked sections first — DBMS vs File System, Bank ER Diagram, and the Key hierarchy (Super/Candidate/Primary/Composite) are the three most consistently tested topics in this list. Make sure you can *draw* the ER diagram and the key hierarchy from memory, not just describe them in words — diagram-based questions carry disproportionate marks in most DAA/DBMS sessionals.

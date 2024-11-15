# How do Teradata store data
Teradata stores data in a unique and efficient way using a combination of parellel processing and hashing to distribute data evenly across multiple storage units. 
Here's an overview of how Teradata Manages and stores data:

### 1. Primary Index and Hashing
- Primary Index (PI):  Every Table in Teradata has a primary index, which can be defined as either unique(Unique Primary Index or UPI) or non-unique (Non_unique Primary Index or NUPI). The primary index is critical for data distribution.
- Hashing: Teradata uses a hashing algorithm on the primary iindex column(s) of each row to assign a uinique value. Thas hash value determines which AMP (Access Module Processor_ will store the row.
- Benefits: hashing enables even distribution across AMPs, which balances the workload and allows Teradata to scale efficiently as data volumes grows. Each AMP manages a subset of data, meaning that Teradata can perform parallel processing of data retrieval and updates.

### 2. AMPS and Data Distribution
- Access Module Processors (AMPs): Each AMP is responsible for a portion of the data, and they work in parallel. When a row is inserted, Teradata's hashing algorithm assigns it to an AMP based on the hash of the primary index value. The AMP stores the row on tis dedicated disk space.
- Data Distribution: Data is distributed across AMPs based on the hash values, ensuring that each AMP gets a balanced portion of the data. This means that no single AMP becomes a bottleneck, enabling Teradata to process large volumes of data simultaneously.

### 3. Storage on Disks
- Virtual Disks(vdisks): Each AMP has its own virtual disks, which holds the rows assigned to that AMP. These virtual disks are logical representations of physical storage that each AMP controls.
- Data Blocks: With each AMP's disk, data is stored in blocks.Teradata uses data blocks as the physical storage unit, which contains multiple rows.These blocks are optimized for read and write operations/
- Row Storage: Rows are stored sequencially within blocks, and Teradata employes a structure called row Id(RID) to indentify each row uniquely with an AMP.
### 4. Teradata File System (TFS)
- Purpose: The Teradata File System (TFS) manages data on each AMP’s disk. It organizes data into tables, manages data blocks, and ensures that data access is efficient.
- Index Management: TFS also manages secondary indexes (if defined), which allow faster data access for specific queries, without impacting the distribution of data across AMPs.

### 5. Data Compression 
- Multi-Value Compression (MVC): Teradata supports multi-value compression, which stores frequently repeated values only once and references them as needed, reducing storage space.
- Block-Level Compression (BLC): This compresses entire data blocks, providing additional space savings and making disk I/O more efficient.
- Columnar Storage (optional): Teradata offers columnar storage options for specific use cases where column-based storage can improve performance and compression.
  
### 6. Fallback and Data Redundancy
- Fallback Protection: Teradata provides an option called Fallback, which maintains a duplicate copy of each row on a different AMP. This ensures data availability in case of an AMP or disk failure.
- Mirroring and RAID: In addition to Fallback, Teradata uses RAID (Redundant Array of Independent Disks) configurations at the hardware level for additional fault tolerance, ensuring data protection and durability.

### 7. Journaling and Transaction Management
- Transient Journal: Teradata uses a transient journal to ensure consistency during transactions. Before any change is made, a copy of the data is stored in the transient journal, which allows the system to roll back if a transaction fails.
- Permanent Journal (optional): This is used for long-term storage of before-and-after images of data, enabling recovery of data to a specific point in time if necessary.
  
### 8. Partitioned Primary Indexes (PPI)
- For large tables, Teradata supports Partitioned Primary Indexes (PPI), which allows data to be stored in partitions based on a specified range or condition (e.g., date ranges). This enables faster query performance by limiting the data scanned to only relevant partitions.

### Example of Data Storage Process in Teradata
When you insert a row into a Teradata table:
- 1.Hash Calculation: The primary index value of the row is hashed to determine the AMP.
- 2.AMP Assignment: Based on the hash value, the row is assigned to an AMP.
- 3.Storage: The AMP stores the row in its virtual disk, within a specific data block.
- 4.Redundancy (if enabled): Fallback is created on another AMP for data redundancy.

This distribution model allows Teradata to support high concurrency, fast data access, and efficient parallel processing.

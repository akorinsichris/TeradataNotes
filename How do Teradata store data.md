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

### 5. Data Compression 

### 6. Fallback and Data Redundancy

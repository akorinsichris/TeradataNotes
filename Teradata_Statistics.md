## Statistics in Teradata
In Teradata, statistics are metadata that describe the distribution and cardinality (number of unique values) of data in columns, indexes, or other database objects. These statistics are used by the Optimizer—the component responsible for query planning and execution—to make informed decisions about how to retrieve data efficiently.

#### Purpose of Statistics
- **Query Optimization:** Statistics help the Optimizer choose the most efficient execution plan by understanding:
  - The size of the data set.
  - Data distribution (e.g., skewed or uniform).
  - Number of rows or unique values.
- **Accurate Resource Allocation:** Helps in estimating memory, CPU, and I/O requirements for executing queries.
- **Improved Performance:** Ensures that the Optimizer avoids inefficient query plans, such as unnecessary full-table scans or skewed joins.

---
#### Where Statistics Are Collected
Statistics can be collected on:
- **Columns:**
    - Individual columns: Helps the Optimizer understand the distribution of values in the column.
    - Multiple columns: Useful for correlated columns that influence each other's data distribution.
- **Indexes:**
    - Primary Index: Optimizer uses stats to understand row distribution across AMPs.
    - Secondary Index: Helps in determining the effectiveness of index usage.
- **Derived Data:**
    - Joins and aggregations: Statistics can be collected on expressions or derived results to optimize repeated queries.

---
#### How Statistics Are Collected
You use the **COLLECT STATISTICS** statement to gather statistics for specific database objects. The system analyzes the data and stores statistical metadata in the Data Dictionary.

*Syntax:*
```
COLLECT STATISTICS ON <table_name> COLUMN <column_name>;

```
#### Examples:
  ****1. Single Column Statistics:****
  ```
  COLLECT STATISTICS ON employees COLUMN department_id;
  ```
  ****2. Multi-Column Statistics (for correlated columns):****
  ```
  COLLECT STATISTICS ON employees COLUMN (department_id, location_id);
  ```
  ****3. Index Statistics:****
  ```
  COLLECT STATISTICS ON employees PRIMARY INDEX;
  ```
***
#### What Statistics Include
  1. Number of Rows: Total number of rows in the table.
  2. Number of Unique Values: Cardinality of the column(s) or index.
  3. Data Distribution: How values are distributed across AMPs.
  4. Skew Information: Degree of data skew, which impacts performance.
  5. High-Value and Low-Value: The range of values in numeric or date columns.
***
### How the Optimizer Uses Statistics
1. **Access Path Selection:**
    - The Optimizer decides whether to use a full-table scan, an index, or a combination of both based on the number of rows and data distribution.
2. **Join Strategy:**
    - Determines the best join method (e.g., merge join, hash join, nested join) and join order by estimating the size of intermediate results.
3. **Aggregation Optimization:**
    - Estimates the grouping size to optimize memory allocation and aggregation operations.
4. **Skew Avoidance:**
    - If data skew is detected (uneven distribution of rows across AMPs), the Optimizer may adjust its strategy to minimize performance degradation.
***
### When to Collect Statistics
  1. **Before Query Execution:**
      - Collect stats when tables are created or before executing complex queries to give the Optimizer accurate information.
  2. **After Data Changes:**
      - Re-collect statistics after significant changes in the table's data (e.g., bulk inserts, updates, or deletes) to ensure the statistics remain relevant.
  3. **On Tables with Frequent Use:**
      - Focus on frequently queried tables or those used in joins, aggregations, or subqueries.
  4. **On Columns with Non-Uniform Data:**
      - For skewed data distributions (e.g., Zip codes, product categories), collecting statistics helps the Optimizer account for uneven data access.
***
### Managing Statistics
  1. **Recollecting Statistics:**
      - Regularly recollect statistics to ensure the metadata reflects the current state of the data:

     ```
        COLLECT STATISTICS ON employees COLUMN department_id;
      ```
  
  3.  **Dropping Statistics:**
      - Remove obsolete statistics using the DROP STATISTICS statement:

      ```
        DROP STATISTICS ON employees COLUMN department_id;
      ```
  
  4.  **Viewing Statistics:**
      - Use the HELP STATISTICS command to check what statistics are available:
        
      ```
        HELP STATISTICS employees;
      ```
      
  5.  **Automatic Statistics Collection:**
      - Some Teradata systems may use AutoStats to automatically manage and recollect statistics based on workload patterns.
***
### Challenges Without Statistics
If statistics are not collected or outdated:
  - The Optimizer may make poor decisions:
    - Use inefficient join strategies.
    - Overestimate or underestimate table sizes.
  - Queries may take longer to execute due to unnecessary full-table scans or improper join orders.
  - Data skew can go unnoticed, leading to resource bottlenecks.

***
### Best Practices for Statistics
1. **Target Key Columns and Indexes:**
      - Collect stats on columns used in WHERE, JOIN, GROUP BY, or ORDER BY clauses.
      - Prioritize indexes, especially the Primary Index, since it impacts data distribution.
2. **Use Multi-Column Stats for Correlated Columns:**
      - For example, state and city are often correlated, so collect stats on both together to improve query accuracy.
3. **Update Stats Regularly:**
      - Automate the recollection process for dynamic or frequently changing tables.
4. **Leverage Sampling:**
      - Use sampled statistics to reduce the overhead of collecting full-table statistics:
       
        ```
            COLLECT STATISTICS USING SAMPLE ON employees COLUMN department_id;
        ```
        
***
### Summary
- Statistics in Teradata are crucial for query optimization, enabling the Optimizer to make accurate and efficient query execution plans.
- They are collected on columns, indexes, or multi-column sets and help estimate row counts, data distribution, and skew.
- Regularly collecting and updating statistics ensures consistent query performance and resource efficiency.


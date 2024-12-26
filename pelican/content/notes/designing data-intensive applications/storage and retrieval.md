Title: storage and retrieval
Date: 2024-12-23
Category: notes

## Storage Engines

This chapter concentrates on storage engines. It addresses the question of how to store data for fast reads and writes. There are broadly two categories of storage engines. LSM-based and BTree based. LSM-based engines first write the data into memory as an efficient data structure and spill them into disk when the size grows. An implementation of this is done using SSTables. The data structure stored in memory is called the “memtable”, when spilled on disk it is called a segment. A key can exist in multiple segments. This can be thought of as an append-only log. An update operation is another entry into the segment with an existing key. This makes the older entry irrelevant. Compaction can be performed on the segment as an optimization. Segments could vary in size due to compaction and there is no restriction on them being of fixed length.

### B-tree based implementation

A B-tree-based implementation involves writing fixed-length pages of data on the disk. A write/update in B-Tree-based storage engines involves physically updating the disk block associated with the page in which the key lies.

### Comparison

|            | LSM                                                      | B-tree                                                   |
|--------------|----------------------------------------------------------|----------------------------------------------------------|
| **Reads**    | - Multiple hops from the memtable to the latest segment  <br> - Continues to the last available live segment | - Based on the key range, locate the right page on the disk <br> - Find the key |
| **Writes**   | - Append to the segment                                   | - Based on the key range, locate the right page <br> - Insert the given key |
| **Reliability** | - Uses WAL                                             | - Also uses WAL                                           |
| **Compaction** | - Keys may be written multiple times in multiple places <br> - Compaction is necessary, leading to potential disk write bandwidth issues | - B-tree pages are of fixed sizes, preventing space from being freed even after deletes <br> - Leads to fragmentation, but beneficial in transactional semantics for key latching |




## Discussions on Indexes

### Secondary Indexes

SSTables and Btrees can be assumed as forms of primary-key indexes, where key-value pairs are being stored.

Secondary indexes are those where the index is on a non-unique attribute/column. This can be created by extending the idea of primary indexes and adding another field to the column to achieve uniqueness like row number.

### Storing values within an Index

So the index can store key values, the value could be either the actual data, or reference to where the data is stored, or a reference to another key. Postgres uses heap files to store the actual data, and the index references it. In MySQL, the actual data is stored in the data structure itself. Such indexes are called clustered indexes. 

Concatenated indexes are those in which more than one key is stored in the database. Here multiple keys are combined to form a single key.

### Fuzzy indexes

Databases like Lucene use techniques like Levenshtein distance to enable similar key searches and not just exact matches for the key.

### In Memory Databases

With RAM getting cheaper, there have been developments in in-memory databases. Redis can be considered one. Since they do not have to comply with models that are easy to store on disk, they can offer complex data structures to the application.

## OLTP/OLAP

Another way to categorize database systems is based on the use case they cater to. Online transaction processing databases cater to transactional needs like order management, restaurant orders, and similar commercial transactions. 

Usually, when these transactions grow in scale and the organization wants to make sense of them, it no longer remains trivial to query data across many OLTP databases. In large organizations, a data warehouse is adopted. These systems are optimized for the cheap storage cost and write once, read multiple times semantics. Data is loaded into these systems with a process called Extract-Transform-Load. 

Some of the popular OLAPs are SQL-on-Hadoop projects like Presto and spark-SQL


Schema for OLAPs are different, they are geared towards ease of querying. So these are more denormalized than the OLTP schemas. Like in the star schema, there is a fact table at the center that captures transactional facts like a change in the inventory and some of the columns would refer to their dimensions, for example, fact_inventory would have fields such as seller_id, warehouse_id and there would be dimension tables like dim_sellers and dim_warehouses which would be associated with those fields.

## Column Oriented Storage

In OLAP databases, tables are denormalized to have many (sometimes hundreds or more) columns for ease of querying. This leads to another choice when storing the actual data on disk. If each row has multiple columns, imagine all of them being loaded into memory when a SELECT happens. To optimize this, data is stored in a columnar manner, so only the selected columns are loaded, unless someone runs a SELECT *.

Another optimization when storing data in a columnar way is compression. Usually, an OLAP table would have a million rows and a few thousand columns in the worst case. However, the unique value for several columns doesn't usually run in the same order as the number of rows. Let's say each column can have “n” unique values. To store this column instead of repeating the column value, a bitmask compression can be used. This column can be stored as n bitmaps. These bitmaps can be thought of as 1xM matrices where M is the number of rows. Wherever a row has a given column value out of the n possibilities, its bitmap can have that row set as 1. 

This can lead to faster reads in queries where OR/AND clauses are used on the value of a bitmask compressed column.

Another optimization that leads to faster reads is storing the data in sorted order. This also leads to better compression as many repeated values can be compressed using run-length encoding.

When writing the data into a disk, LSM is better suited for OLAP databases, due to the large volume of data and sorted order. In a BTree implementation, the data would have to be shuffled a lot to maintain the sorted order.

To optimize query performance for frequent queries, materialized views are a common approach in OLTPs but they make the writes expensive in OLAPs. Materialized views need to be updated each time new writes happen in the table. In OLAPs, something called data cubes is used to precompute aggregates across multiple dimensions. Consider a fact table, which can be aggregated on columns like date, and other keys like product_id.  The result can be stored in a multi-dimensional table called a datacube. 



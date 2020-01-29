>1. What are Indexes?

A. Index is a data structure to provide the quick lookup in columns of a table. It enhances the speed of operations accessing the data at cost of asdditional writes and memory.

[Resource for Indexing on youtube](https://www.youtube.com/watch?v=aZjYr87r1b8)

***

> Storage Methodology

Database stores data in Secondary memory which is divided into blocks.
While searching the data, effective search time is searching a block.

* Generally each block size is 512 bytes.
* Lets say a given record(row) takes 128 bytes, then every block will contain 4 records.
* During any operation like (SELECT, INSERT) the data is brought into main memory.
* In case of indexes, it stores a key attribute and pointer to a record.

| key | pointer           |
| --- | ----------------- |
| 1   | pointer to record |

* Therefore, the each record of an index takes lesser space, lets say 16bytes and thus 1 block will contain 512/16=32 records of indexes which makes it easier in searching, as larger reference to database records can be found in single block.
* Hence fast search can be provided by index table when the size of index table is smaller.

***


## Types Of Indexing

### On the basis of sorting and attribute type: 
Before moving ahead, Index file is always sorted irrespective of main file.

> Primary Indexing

* When indexing is done on primary key as search key.
* It can be done only when main file is sorted.
* It is a type of sparse indexing in which all the valuesa are not being used.

> Clustered Indexing

* When indexing is done on non key attribute which means values can be repeat .
* It can also be done only when main file is sorted.
* It is an example of sparse and dense indexing.

> Secondary Indexing

* Indexing can be done on any of key or non key attribute.
* It is used when main file is unsorted on a given attribute.
* It is a type of dense indexing.

### On the basis of density

> Sparse Indexing:

* If records are sorted and we can store fewer **records** than actual records in our index table.

> Dense Indexing:

* If records are not sorted and we have to store entry for every **value**.

## Indexes

- Indexes improve query execution
- Without index whole collection must be scanned (COLLSCAN or COLLECTIONSCAN)
- Index stores sorted field values
- If appropriate index exists, MongoDB performs only index scan (IXSCAN)

## Create Index

- Index is created by key
- Key contains key value pair where value is the sorted order => 1 (ASC) or -1 (DESC)

## How Index works
- MongoDB uses B-Tree Index data structure
- We scan only index and when it's found then the pointer leads to the resulting doc

## Default _id Index
- **_id** is default index in each mongo collection
- Name of _id index is **_id_**
- Default _id index is unique

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

## Default \_id Index

- **\_id** is default index in each mongo collection
- Name of _id index is \*\*\_id_\*\*
- Default \_id index is unique

## getIndexes()

- Returns current indexes for certain collection
- Indexes can't be retrieved from b-trees but info about indexes can be retrieved from mongo collection

```sh
db.shop.getIndexes()
```

- O/P => Array of objects

## Create new Index

```sh
db.persons.createIndex({ age: 1 })

db.persons.createIndex({ name: 1 })
```

## Index creation options

- **{ background: true }** => Create index in the background, other operations will not be blocked. Can be used on large collections and large db
- **{ unique: true }** => Create unique index
- **{ name: "<indexName>" }** => Specify name for the index

```sh
db.persons.createIndex(
    {
        index: 1
    },
    {
        unique: true
    }
)

db.persons.createIndex(
    {
        name: 1
    },
    {
        background: true
    }
)

db.persons.createIndex(
    {
        age: 1
    },
    {
        name: "customAgeIndex"
    }
)
```

- O/P

```sh
{
        "createdCollectionAutomatically" : false,
        "numIndexesBefore" : 1,
        "numIndexesAfter" : 2,
        "ok" : 1
}
```

- db.persons.getIndexes()

```sh
[
        {
                "v" : 2,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_"
        },
        {
                "v" : 2,
                "unique" : true,
                "key" : {
                        "index" : 1
                },
                "name" : "index_1"
        }
]
```

## Query Performance

### explain()

- Returns info about the query
- Arg => "executionStats" => optional

```sh
db.persons.explain().find(
    {
        age: {
            $gt: 25
        }
    }
)
```

- O/P

```sh

        "queryPlanner" : {
                "plannerVersion" : 1,
                "namespace" : "db.persons",
                "indexFilterSet" : false,
                "parsedQuery" : {
                        "age" : {
                                "$gt" : 25
                        }
                },
                "queryHash" : "4BB283C4",
                "planCacheKey" : "DF7FEF1F",
                "winningPlan" : {
                        "stage" : "FETCH",
                        "inputStage" : {
                                "stage" : "IXSCAN",
                                "keyPattern" : {
                                        "age" : 1
                                },
                                "indexName" : "customAgeIndex",
                                "isMultiKey" : false,
                                "multiKeyPaths" : {
                                        "age" : [ ]
                                },
                                "isUnique" : false,
                                "isSparse" : false,
                                "isPartial" : false,
                                "indexVersion" : 2,
                                "direction" : "forward",
                                "indexBounds" : {
                                        "age" : [
                                                "(25.0, inf.0]"
                                        ]
                                }
                        }
                },
                "rejectedPlans" : [ ]
        },
        "serverInfo" : {
                ...
        },
        "ok" : 1
}
```

- IXSCAN => Index Scan

```sh
db.persons.explain("executionStats").find(
    {
        age: {
            $gt: 25
        }
    }
)
```

- O/P

```sh
{
        "queryPlanner" : {
                "plannerVersion" : 1,
                "namespace" : "db.persons",
                "indexFilterSet" : false,
                "parsedQuery" : {
                        "age" : {
                                "$gt" : 25
                        }
                },
                "winningPlan" : {
                        "stage" : "FETCH",
                        "inputStage" : {
                                "stage" : "IXSCAN",
                                "keyPattern" : {
                                        "age" : 1
                                },
                                "indexName" : "customAgeIndex",
                                "isMultiKey" : false,
                                "multiKeyPaths" : {
                                        "age" : [ ]
                                },
                                "isUnique" : false,
                                "isSparse" : false,
                                "isPartial" : false,
                                "indexVersion" : 2,
                                "direction" : "forward",
                                "indexBounds" : {
                                        "age" : [
                                                "(25.0, inf.0]"
                                        ]
                                }
                        }
                },
                "rejectedPlans" : [ ]
        },
        "executionStats" : {
                "executionSuccess" : true,
                "nReturned" : 692,
                "executionTimeMillis" : 2,
                "totalKeysExamined" : 692,
                "totalDocsExamined" : 692,
                "executionStages" : {
                        "stage" : "FETCH",
                        "nReturned" : 692,
                        "executionTimeMillisEstimate" : 0,
                        "works" : 693,
                        "advanced" : 692,
                        "needTime" : 0,
                        "needYield" : 0,
                        "saveState" : 0,
                        "restoreState" : 0,
                        "isEOF" : 1,
                        "docsExamined" : 692,
                        "alreadyHasObj" : 0,
                        "inputStage" : {
                                "stage" : "IXSCAN",
                                "nReturned" : 692,
                                "executionTimeMillisEstimate" : 0,
                                "works" : 693,
                                "advanced" : 692,
                                "needTime" : 0,
                                "needYield" : 0,
                                "saveState" : 0,
                                "restoreState" : 0,
                                "isEOF" : 1,
                                "keyPattern" : {
                                        "age" : 1
                                },
                                "indexName" : "customAgeIndex",
                                "isMultiKey" : false,
                                "multiKeyPaths" : {
                                        "age" : [ ]
                                },
                                "isUnique" : false,
                                "isSparse" : false,
                                "isPartial" : false,
                                "indexVersion" : 2,
                                "direction" : "forward",
                                "indexBounds" : {
                                        "age" : [
                                                "(25.0, inf.0]"
                                        ]
                                },
                                "keysExamined" : 692,
                                "seeks" : 1,
                                "dupsTested" : 0,
                                "dupsDropped" : 0
                        }
                }
        },
        "ok" : 1
}
```

- **"keysExamined"** => less than the total docs
- **"executionTimeMillisEstimate"** => 0
- "stage": "IXSCAN"

- Without indexes => all the docs from first to last will be scanned and the stage will be **COLLSCAN**

## executionStats with RegExp

```sh
db.persons.find({
    name: /el/i
}).explain("executionStats")
```

- All keys (whole index) will be examined as per the regexp but very few docs will be examined

## Indexing Deletion

```sh
db.persons.dropIndex({
    age: 1
})

db.persons.dropIndexes() // drops all indexes, except the _id ofc
```

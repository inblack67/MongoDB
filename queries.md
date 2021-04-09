## Comparison Operators

- $eq => equal
- $ne => not equal
- $lt => less than
- $lte => less than or equal
- $gt => greator than
- $gte => greator than or equal

```sh
db.persons.find({
    age: {
        $lt: 35
    }
}).pretty()

db.persons.find({
   eyeColor: {
       $ne: "blue"
   }
}).pretty()

db.persons.find({
    registered: {
        $eq: ISODate("2014-08-21T11:37:18Z")
    }
}).pretty()
```

## $in & $nin

```sh
db.persons.find({
    age: {
        $nin: [23,30]
    },
    eyeColor: {
        $in: ["green", "blue"]
    }
}).pretty()
```

## $and

- Combines multiple conditions
- Resulting docs must match all conditions
- Requires an array of conds.

```sh
db.persons.find(
    {
        $and: [
            {
                gender: "male",
                age: 25
            }
        ]
    }
).pretty()
```

- Explicit **$and** must be used if conditions contains same field or operators
- In following query, implicit **and** (conds. seperated by comma) is used and thus, condition **age > 25** will be overwritten by condition **$age > 20**

```sh
db.persons.find({
    age: {
        $gt: 25
    },
    age: {
        $gt: 20
    }
}).pretty()
```

- Explicit $and

```sh
db.persons.find({
    $and: [
        {
            age: {
                $gt: 20
            },
        },
        {
            age: {
                $lt: 25
            }
        }
    ]
}).pretty()
```

## or

- Combines multiple conditions
- Resulting docs must match ANY of the conditions

```sh
db.persons.find({
    $or: [
        {
            age: 25
        },
        {
            gender: "male"
        }
    ]
}).pretty()
```

- $in can be used instead of $or while querying via same field

```sh
db.persons.find({
    $or: [
        {
            age: 25
        },
        {
            age: 23
        }
    ]
}).pretty()
```

- $in can be used here (cleaner)

```sh
db.persons.find({
    age: {
        $in: [25,23]
    }
}).pretty()
```

## Query embedded docs

```sh
db.persons.find({
    "company.location.country": "USA"
}).pretty()
```

## Query Array by field value

- Query by value

```sh
db.persons.find({
    tags: "ad"
}).pretty()
```

- Query by index

```sh
db.persons.find({
    "tags.0": "ad"
}).pretty()
```

## $all & $size

- Array contains all specified value independent of order

```sh
db.persons.find({
    tags: {
        $all: ["ad", "eu"]
    }
}).pretty()
```

- Array is of certain size

```sh
db.persons.find({
    tags: {
        $size: 2
    }
}).pretty()
```

## Query Array of objects

- eg:-

```sh
{
    friends: [
        {
            name: "ok",
            age: 1
        },
        {
            name: "ok",
            age: 1
        },
    ]
}
```

- Query:-

```sh
db.persons.find({
    "friends.name": "ok"
}).pretty()

- Order matters (name, age)
db.persons.find({
    friends: {
        name: "ok",
        age: 1
    }
}).pretty()
```

## $elemMatch

- At least one nested doc in the array must match all conditions. Order of conditions does not matter

```sh
db.persons.find({
  friends: {
       $elemMatch: {
       name: "Bob",
       registered: false
   }
 }
}).pretty()
```

## $exists & $type

- **$exists** => certain field exists in the doc or not

```sh
db.persons.find({
    tags: {
        $exists: true
    }
}).pretty()
```

- **$type** => query by value and type
```sh
db.persons.find({
    tags: {
        $type: "array"
    }
}).pretty()
```

## BSON Types
| Type |  Number ID | String ID
| ---- |  ---- | ---
| String | 2 | "string"
| Object | 3 | "object"
| Array | 4 | "array"
| Boolean | 8 | "bool"
| 32-bit Integer | 16 | "int"
| 64-bit Integer | 18 | "long"
| Double | 1 | "double"
| Date | 9 | "date"
| ObjectId | 7 | "objectId"

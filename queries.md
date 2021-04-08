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

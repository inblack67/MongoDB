# Aggregations

## $match
```sh
db.persons.aggregate([
    {
        $match: {
            age: {
                $gt: 25
            }
        }
    }
]).pretty()

db.persons.aggregate([
    {
        $match: {
            isActive: true
        }
    }
]).pretty()
```

## $match and $size
```sh
db.persons.aggregate([
    {
        $match: {
            tags: {
                $size: 3
            }
        }
    }
]).pretty()
```

## $group

    - Group by unique ages but output will have only _id as a key and distinct age as value
    - _id field is mandatory in this query
```sh
db.persons.aggregate([
    {
        $group: {
            _id:  "$age"
        }
    }
]).pretty()

db.persons.aggregate([
    {
        $group: {
            _id: '$gender'
        }
    }
]).pretty()

- Nested fields
db.persons.aggregate([
    {
        $group: {
            _id: "$company.location.country"
        }
    }
])

- Multiple fields
db.persons.aggregate([
    {
        $group: {
            _id: {
                age: "$age",
                gender: "$gender"
            }
        }
    }
]).pretty()

- Output will be like this - 
{ "_id" : { "age" : 23, "gender" : "female" } }
```
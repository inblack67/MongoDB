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

## $match & $group
```sh
db.persons.aggregate([
    {
        $match: {
            favoriteFruit: "banana"
        },
    },
    {
        $group: {
            _id: {
                age: "$age",
                isActive: "$isActive",
                favoriteFruit: "$favoriteFruit"
            }
        }
    }
]).pretty()
```

- No O/P => as the result of $group query will have _id field only
```sh
db.persons.aggregate([
    {
        $group: {
            _id: {
                age: "$age",
                isActive: "$isActive",
                favoriteFruit: "$favoriteFruit"
            }
        }
    },
    {
        $match: {
            favoriteFruit: "banana"
        },
    },
]).pretty()
```

- Works
```sh
db.persons.aggregate([
    {
        $group: {
            _id: {
                age: "$age",
                isActive: "$isActive",
                favoriteFruit: "$favoriteFruit"
            }
        }
    },
    {
        $match: {
            "_id.favoriteFruit": "banana" 
        },
    },
]).pretty()
```

## $count

- Must be the last stage of aggregation
- O/P => Total Docs In Person Collection
- "allDocumentsCount" => placeholder key (custom) for count's value 
- 0.21 sec
```sh
db.persons.aggregate([
    {
        $count: "allDocumentsCount"
    }
])
```
- $count is very fast => server side calculation => no docs returned

- Count method of Find is the wrapper of $count aggregation
- 0.21 sec (server side)
```sh
db.persons.find({}).count()
```


### Client Side Counting => Iterating over the docs => slower
- 1.4 sec
```sh
db.persons.aggregate([]).itcount()
```

- 1.7 sec
```sh
db.persons.aggregate([]).toArray().length
```
- $group & $count

- Count the number of countries => unique ofc
```sh
db.persons.aggregate([
{
       $group: {
        _id: "$company.location.country"
    }
},
{
    $count: "numberOfCountries"
}
])
```

## $sort
- 1 => ASC
- -1 => DESC
- First sort by age, then gender then eyeColor
```sh
db.persons.aggregate([
    {
        $sort: {
            age: -1,
            gender: 1,
            eyeColor: 1
        }
    }
]).pretty()
```

- $sort & $group
```sh
db.persons.aggregate([
    {
        $group: {
            _id: "$age"
        }
    },
    {
        $sort: {
            _id: -1
        }
    }
]).pretty()

db.persons.aggregate([
    {
        $group: {
            _id: {
                age: "$age",
                eyeColor: "$eyeColor",
                gender: "$gender"
            } 
        }
    },
    {
        $sort: {
            "_id.gender": 1,
            "_id.age": -1
        }
    }
]).pretty()
```
## $project
- Choose what you need (graphql?)
- 0 => Exclude
- 1 => Include
- _id included by default
```sh
db.persons.aggregate([
    {
        $project: {
            name: 1,
            "company.title": 1
        }
    }
]).pretty()
```
- Exclude _id
- All fields except _id will be displayed
```sh
db.persons.aggregate([
    {
        $project: {
            _id: 0
        }
    },
    {
        $count: "total"
    }
]).pretty()
```
- If only exluding conditions are written => rest of the fields are shown, else only the included ones

- Restructured Results
```sh
db.persons.aggregate([
    {
        $project: {
            _id: 0,
            customInfo: {
                eyes: "$eyeColor",
                sex: "$gender"
            }
        }
    }
]).pretty()
```

## $limit
```sh
db.persons.aggregate([
    {
        $limit: 2
    }
]).pretty()
```
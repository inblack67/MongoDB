## Updates

- update(), updateOne(), updateMany()
- 3 args => first query, second update and third options (optional)

## $set

- Replace or set value of the field
- Add fields if they do not exist in the doc

```sh
db.shop.update({
    index: 3
}, {
    $set: {
        cart: [],
        cartId: 100,
        customer: {
            name: "John",
            email: "john@gmail.com",
            age: 24
        }
    }
})
```

## $unset

- Deletes fields
- 1 for values to be deleted => can use any value though

```sh
db.shop.update(
    {
        index: 3
    },
    {
        $unset: {
            cartId: 1
        }
    }
)
```

## update()

- If not supplied any options it will find first matching doc and update it and then stops

```sh
"nMatched": 1, // number matched
"nModified": 1
```

## Update multiple docs

- 3rd arg => **{ multi: true }**

```sh
db.shop.update({}, {
    $set: {
        active: true
    }
}, {
    multi: true
})
```

## updateOne()
- Returns:-

```sh
{
    acknowledged: true,
    matchedCount: 1,
    modifiedCount: 1 // no update was needed
}
```

## updateMany()
- Same as updateOne but for multi docs

## replaceOne()
- Replace one doc
```sh
db.shop.replaceOne(
    {
        index: 1
    },
    {
        index: 1,
        cart: ["novel"]
    }
)
```
## Multiple Operators
```sh
db.shop.update({
    index: 1
}, {
    $set: {
        hello: "worlds"
    },
    $unset: {
        cart: 1
    }
})
```

## $rename
```sh
db.shop.update({
    index: 1
}, {
    $rename: {
        hello: "greet"
    }
})
```
```sh
db.shop.update(
    {
        index: {
            $exists: true
        }
    },
    {
        $rename: {
            index: "INDEX"
        }
    },
    {
        multi: true
    }
)
```

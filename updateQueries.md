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

## $currentDate

- Set the value of the field to the current date
- Add createdAt to all docs:-

```sh
db.shop.update(
    {},
    {
        $currentDate: {
            createdAt: true
        }
    },
    {
        multi: true
    }
)
```

## Array update operators

- $ - positional operator
- $push - adds element to the array
- $addToSet - adds el but ensure uniqueness
- $pull - deletes an element
- $pullAll
- $pop

## $push

- Appends element to the array
- If array does not exist then it will be created automatically

```sh
db.shop.update(
    {},
    {
        $push: {
            cart: "veggies"
        }
    },
    {
        multi: true
    }
)
```

## $push & $each

- Push mutiple elements in an array

```sh
db.shop.update(
    {},
    {
        $push: {
            cart: {
                $each: ["item1", "item2"]
            }
        }
    },
    {
        multi: true
    }
)
```

## $addToSet

- Appends element to the array if it doesn't exist already

```sh
db.shop.update(
    {},
    {
        $addToSet: {
            cart: "item1"
        }
    },
    {
        multi: true
    }
)
```

## $pop

- 1 => last element will be deleted
- -1 => first element will be deleted
- If only one element in the array then 1 or -1 => that element will be deleted

```sh
db.shop.update({
    INDEX: 1
}, {
    $pop: {
        cart: 1
    }
})
```

## $pull

- Removes all elements from the array matching specified condition

```sh
db.shop.update({
    INDEX: 1
}, {
    $pull: {
        cart: "item1",
        spent: {
            $gt: 50
        }
    }
})
```

## $pullAll

- Removes all elements in the array that match specified valus
- Similar occurences will also be deleted
- No conditions

```sh
db.shop.update(
    {},
    {
        $pullAll: {
            cart: ["item1","item2"]
        }
    },
    {
        multi: true
    }
)
```

- Equivalent to

```sh
db.shop.update(
    {},
    {
        $pull: {
            cart: {
                $in: ["item1", "item2"]
            }
        }
    },
    {
        multi: true
    }
)
```

- $pull uses queries, $pullAll does not

## $

- Positional Operator
- Points to certain element in the array that machted query
- Refer certain element of the array:-

```sh
db.shop.update(
    {
        cart: "item2"
    },
    {
        $set: {
            "cart.$": "updatedItem2"
        }
    },
    {
        multi: true
    }
)
```

- For array of objects
  ```sh
  {
      $set: {
          "<arrayField>.$.prop": <value>
      }
  }
  ```

## $ in nested docs

- Data:-

```sh
{
    cart: [
        {
            title: "TV",
            price: 20,
            quantity: 2
        },
        {
            title: "Phone",
            price: 10,
            quantity: 3
        }
    ]
}
```

- Update quantity to 4 with title "Phone"

```sh
db.shop.update(
    {
        "cart.title": "Phone"
    },
    {
        "cart.$.title": 4
    },
)
```

- With $elemMatch

```sh
db.shop.update(
    {
        cart: {
            $elemMatch: {
                title: "Phone"
            }
        }
    },
    {
        $set: {
            "cart.$.quantitiy": 4
        }
    }
)
```

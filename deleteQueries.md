## Delete Docs

- remove() => either one or many
- deleteOne() => only one
- deleteMany() => many

## remove()

- Remove all docs

```sh
db.persons.remove()
```

- Remove one
  db.persons.remove({}, true)

## deleteOne()

- Delete first doc

```sh
    db.persons.deleteOne({})
```

## deleteMany()

- Empty query will delete all docs

## drop()

- Deletes entire collection, it's docs and indexes

```sh
db.persons.drop()
```

## dropDatabase()

- Deletes entire database
- Switch to db which needs to be deleted and then delete it

```sh
use myDb

db.dropDatabase()
```

## mongoexport
- Exports collection data in JSON or CSV
- Strict mode extended JSON format is used 
```sh
mongoexport --db dbname --collection persons --out persons.json
```
- --type=csv

- If mongodb server is remote then we need to add 4 more options before the rest
    - --host
    - --username
    - --password
    - --authenticationDatabase
- --jsonArray will export data as Array of JSON objects

## mongoimport
- Imports collection data in JSON or CSV format.
- Strict mode extended JSON format is used 
```sh
mongoimport --db mydb --collection personsImport --file persons.json
```

## mongodump
- Binary export of the db
- All BSON types and indexes are exported without any changes
- Without **--db** option, all dbs except **local** will be dumped
```sh
mongodump --db dbname
```

## mongorestore
- Binary import of the db backup
- Indexes will be re created
- Following will restore data from the local dump folder to the db
```sh
mongorestore
```

## mongostate
- Monitor db performance in real time
```sh
mongostat
```

## mongotop
- Top current read and write operations
```sh
mongotop
```
- Each 3 minutes
```sh
mongotop 80
```
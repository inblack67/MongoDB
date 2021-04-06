## mongoexport
- Exports collection data in JSON or CSV
- Strict mode extended JSON format is used 
```sh
mongoexport --db inblack67 --collection persons --out persons.json
```
- --type=csv

- If mongodb server is remote then we need to add 4 more options before the rest
    - --host
    - --username
    - --password
    - --authenticationDatabase
- --jsonArray will export data as Array of JSON objects

## mongoimport
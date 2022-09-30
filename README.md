# MongoDB University (M001) - Notes

#### Course Agenda

- Chapter 1: Concepts of RDBMS and MongoDB

- Chapter 2: Modeling for MongoDB

- Chapter 3: Code and Queries

- Chapter 4: The Life Cycle of an Application and Additional Resources


## Connect

---

```bash
mongosh "mongodb+srv://sandbox.<...>.mongodb.net/myFirstDatabase" --apiVersion 1 --username m001-student

mongo "mongodb+srv://sandbox.<...>.mongodb.net/myFirstDatabase" --username m001-student
``` 

## Importing and Exporting Data

---

```bash
# BSON

mongodump --uri "mongodb+srv://m001-student:m001-student@sandbox.<...>.mongodb.net/sample_supplies"  --forceTableScan

mongorestore --uri "mongodb+srv://m001-student:m001-student@sandbox.<...>.mongodb.net/sample_supplies"  --drop dump


#JSON

mongoexport --uri="mongodb+srv://m001-student:m001-student@sandbox.<...>.mongodb.net/sample_supplies" --collection=sales --out=sales.json  --forceTableScan

mongoimport --uri="mongodb+srv://m001-student:m001-student@sandbox.<...>.mongodb.net/sample_supplies" --drop sales.json
```

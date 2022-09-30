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

## Initial Commands

---

```bash
show dbs

show collections

use.<collection>
``` 

## Find Command

---

```bash
db.zips.find({"state": "NY"}).count()

db.zips.find({"state": "NY", "city": "ALBANY"})

db.zips.find({"state": "NY", "city": "ALBANY"}).pretty()
``` 

## Insert Command

---

During the insert, mongod will create the _id field and assign it a unique ObjectId() value, as verified by the inserted
document:

```bash
db.products.insert( { item: "card", qty: 15 } )

db.products.insert(
   [
     { _id: 11, item: "pencil", qty: 50, type: "no.2" },
     { item: "pen", qty: 20 },
     { item: "eraser", qty: 25 }
   ]
)
``` 

The following example performs an unordered insert of three documents. With unordered inserts, if an error occurs during
an insert of one of the documents, MongoDB continues to insert the remaining documents in the array.

```bash
db.products.insert(
   [
     { _id: 20, item: "lamp", qty: 50, type: "desk" },
     { _id: 21, item: "lamp", qty: 20, type: "floor" },
     { _id: 22, item: "bulk", qty: 100 }
   ],
   { ordered: false }
)
``` 

## Update Command

---

```bash
db.collection.update(query, update, options)

db.zips.updateOne({ "zip": "12534" }, { "$set": { "pop": 17630 } })

db.zips.updateMany({ "city": "HUDSON" }, { "$inc": { "pop": 10 } })

db.grades.updateOne(
    {
        "student_id": 250,
        "class_id": 339
    },
    {
        "$push": {
            "scores": {
                "type": "extra credit",
                "score": 100
            }
        }
    }
)
  
db.grades.findOneAndUpdate(
   { "name" : "R. Stiles" },
   { $inc: { "points" : 5 } }
)
  
# The $inc operator increments the value
# The $set operator replaces the value
``` 

## Delete Command

---

```bash
db.inspections.deleteOne({ "test": 3 })

db.inspections.deleteMany({ "test": 1 })

db.inspection.drop()
``` 


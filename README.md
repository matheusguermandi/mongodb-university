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

## Comparison Query Operators

---

| Operators | Description                                                                                  | Examples                                                        |
| --------- | -------------------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| $eq       | It is used to match the values of the fields that are equal to a specified value.            | db.contributor.find({branch: {$eq: "CSE"}}).pretty()            |
| $ne       | It is used to match all values of the field that are not equal to a specified value.         | db.contributor.find({branch: {$ne: "CSE"}}).pretty()            |
| $gt       | It is used to match values of the fields that are greater than a specified value.            | db.contributor.find({salary: {$gt: 1000}}).pretty()             |
| $gte      | It is used to match values of the fields that are greater than equal to the specified value. | db.contributor.find({joiningYear: {$gte: 2017}})                |
| $lt       | It is used to match values of the fields that are less than a specified valueo               | db.contributor.find({salary: {$lt: 2000}}).pretty()             |
| $lte      | It is used to match values of the fields that are less than equals to the specified value    | db.contributor.find({salary: {$lte: 1000}}).pretty()            |
| $in       | It is used to match any of the values specified in an array.                                 | db.contributor.find({name: {$in: ["Amit", "Suman"]}}).pretty()  |
| $nin      | It is used to match none of the values specified in an array.                                | db.contributor.find({name: {$nin: ["Amit", "Suman"]}}).pretty() |

## Logical Query Operators

---

| Operators | Description                                                                                                                   | Examples                                                                     |
| --------- | ----------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| $and      | It is used to join query clauses with a logical AND and return all documents that match the given conditions of both clauses. | db.contributor.find({$and: [{branch: "CSE"}, {joiningYear: 2018}]}).pretty() |
| $or       | It is used to join query clauses with a logical OR and return all documents that match the given conditions of either clause. | db.contributor.find({$or: [{branch: "ECE"}, {joiningYear: 2017}]}).pretty()  |
| $not      | It is used to invert the effect of the query expressions and return documents that does not match the query expression.       | db.contributor.find({salary: {$not: {$gt: 2000}}}).pretty()                  |
| $nor      | It is used to join query clauses with a logical NOR and return all documents that fail to match both clauses.                 | db.contributor.find({$nor: [{salary: 3000}, {branch: "ECE"}]}).pretty()      |

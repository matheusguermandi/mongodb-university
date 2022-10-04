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

## Expressive Query Operator

---

Allows the use of aggregation expressions within the query language.

```bash
# Simple example
db.monthlyBudget.find( { $expr: { $gt: [ "$spent" , "$budget" ] } } )
``` 


## Array Operators

---

```bash
db.listingsAndReviews.find({ "amenities": {
                                  "$size": 20,
                                  "$all": [ "Internet", "Wifi",  "Kitchen",
                                           "Heating", "Family/kid friendly",
                                           "Washer", "Dryer", "Essentials",
                                           "Shampoo", "Hangers",
                                           "Hair dryer", "Iron",
                                           "Laptop friendly workspace" ]
                                         }
                            }).pretty()

db.listingsAndReviews.find({ "amenities": { "$all": [ "Free parking on premises", "Wifi", "Air conditioning" ] },
                             "bedrooms": { "$gte":  2 }
                            }).pretty()

db.companies.find({ "relationships":
                      { "$elemMatch": { "is_past": true,
                                        "person.first_name": "Mark" } } },
                  { "name": 1 }).count()

db.companies.find({ "funding_rounds": { "$size": 8 } }, { "name": 1, "_id": 0 })

db.listingsAndReviews.find({ "amenities.0": "Internet" }, { "name": 1, "address": 1 }).pretty()
```

## Project Fields to Return from Query (Projection)

---

```bash
# Simple example
db.inventory.find( { status: "A" }, { item: 1, status: 1 } )

# Suppress _id Field
db.inventory.find( { status: "A" }, { item: 1, status: 1, _id: 0 } )

# Return All But the Excluded Fields
db.inventory.find( { status: "A" }, { status: 0, instock: 0 } )

# Project Specific Array Elements in the Returned Array
db.inventory.find( { status: "A" }, { item: 1, status: 1, instock: { $slice: -1 } } )

# Array Operator and Projection ($elemMatch)
db.grades.find({ "class_id": 431 }, { "scores": { "$elemMatch": { "score": { "$gt": 85 } } } }).pretty()
```

## Aggregation Framework

Aggregation operations process multiple documents and return computed results. You can use aggregation operations to:

- Group values from multiple documents together.

- Perform operations on the grouped data to return a single result.

- Analyze data changes over time.

To perform aggregation operations, you can use:

- Aggregation pipelines, which are the preferred method for performing aggregations.

- Single purpose aggregation methods, which are simple but lack the capabilities of an aggregation pipeline.

---

```bash
db.listingsAndReviews.aggregate([
                                  { "$match": { "amenities": "Wifi" } },
                                  { "$project": { "price": 1,
                                                  "address": 1,
                                                  "_id": 0 }}]).pretty()

db.listingsAndReviews.aggregate([ { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country" }}])
                                  
db.listingsAndReviews.aggregate([
                                  { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country",
                                                "count": { "$sum": 1 } } }
                                ])                                                                                    
```

## Exercises

--- 

```bash
db.companies.find({ "$or": [ { "$and": [ { "founded_year": 2004 }, { "$or": [ { "category_code": "social" }, { "category_code": "web" } ] } ] }, { "$and": [ { "founded_month": 10 }, { "$or": [ { "category_code": "social" }, { "category_code": "web" } ] } ] } ] })

db.companies.find({ "$expr": { "$eq": [ "$twitter_username", "$permalink" ] } } ).count()

# Chapter 4: Advanced CRUD Operations - Lab 1: Array Operators
db.listingsAndReviews.find({ "$and": [ { "accommodates": { "$gt": 6 } }, { "reviews": { "$size": 50 } } ] })

# Chapter 4: Advanced CRUD Operations - Lab 2: Array Operators
db.listingsAndReviews.find({ "$and": [ { "property_type": { "$eq": "House" } }, { "amenities": "Changing table" } ] }).count()

# Chapter 4: Advanced CRUD Operations - Lab: Array Operators and Projection
db.companies.find({ "offices": { "$elemMatch": { "city": { "$eq": "Seattle" } } } }).count()

# Chapter 4: Advanced CRUD Operations - Lab 1: Querying Arrays and Sub-Documents
db.trips.find({"start station location.coordinates": {"$lt": -74}}).count()

# Chapter 4: Advanced CRUD Operations - Lab 2: Querying Arrays and Sub-Documents
sample_training> db.inspections.find({"address.city": {"$eq":"NEW YORK"}}).count()

``` 


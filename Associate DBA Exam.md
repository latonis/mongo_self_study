[MongoDB Associate DBA Exam](https://learn.mongodb.com/pages/mongodb-associate-database-administrator-exam)

## Creating and Deploying an Atlas Cluster

### Creating the cluster

```bash
atlas setup --clusterName myAtlasClusterEDU --provider AWS --currentIp --skipSampleData --username myAtlasDBUser --password myatlas-001 | tee atlas_cluster_details.txt
```

### Importing Data
```bash
atlas clusters sampleData load myAtlasClusterEDU
```
![[Pasted image 20230802114319.png]]

### Querying Data
```bash

show databases
select <database>
show collections
db.<collection>.find({field: key})

```

```json
db.accounts.find({account_id: 674364})
[
  {
    _id: ObjectId("5ca4bbc7a2dd94ee5816238f"),
    account_id: 674364,
    limit: 10000,
    products: [ 'InvestmentStock' ]
  }
]

```

### Resources

#### Lesson 01: Introduction to MongoDB Atlas, the Developer Data Platform

- [MongoDB Docs: MongoDB Clusters](https://www.mongodb.com/basics/clusters?_ga=2.98660183.810066485.1665291537-836515500.1666025886)
- [MongoDB Docs: MongoDB Atlas](https://www.mongodb.com/docs/atlas/?_ga=2.98660183.810066485.1665291537-836515500.1666025886)

#### Lesson 02: Creating and Deploying an Atlas Cluster

- [MongoDB Docs: Get Started with Atlas](https://www.mongodb.com/docs/atlas/getting-started/?_ga=2.56158050.810066485.1665291537-836515500.1666025886)
- [MongoDB Docs: Deploy a Free Cluster](https://www.mongodb.com/docs/atlas/tutorial/deploy-free-tier-cluster/?_ga=2.56158050.810066485.1665291537-836515500.1666025886)
- [MongoDB Docs: Load Sample Data](https://www.mongodb.com/docs/atlas/sample-data/?_ga=2.56158050.810066485.1665291537-836515500.1666025886)


## MongoDB and the Document Model
### Key Terms
1. Document: basic unit of data in MongoDB
2. Collection: grouping of documents
3. Database: container for collections
	- Database holds collection(s) which holds document(s)

### MongoDB Document Model
Syntax
```json
{
"key": value,
"key": value,
"key" : value
}
```

1. BSON is binary json
	- supports additional types like datetime, numbers, and objectIds 
		- ObjectId is a datatype used to create unique identifiers for the required `_id_` field in documents
```json
{
"_id": 1,
"name": "AC3 Phone",
"colors" : ["black", "silver"],
"price" : 200,
"available" : true
}
```
Value types in a document can be any data type, including str, obj, array, boolean, null, datetime, objectIds, and more.
Optionally, we can use schema validation.

### Managing Databases, Collections, and Documents in #Atlas
Atlas Data Explorer allows you to interact and manage data from the UI
	- Create and View
		- Document
		- Collection
		- Document

## MongoDB connection with Shell

- JS repl environment
- Can create variables within the shell-```
```javascript
const greetingArray = ["hello", "world", "welcome"]
const loop = (array) => array.forEach(el => console.log(el));
loop(greetingArray);
```
- List Connection Strings
```bash
$ atlas clusters connectionStrings describe myAtlasClusterEDU
> STANDARD CONNECTION STRING
> mongodb+srv://myatlasclusteredu.xuu09tn.mongodb.net

MY_ATLAS_CONNECTION_STRING=$(atlas clusters connectionStrings describe myAtlasClusterEDU | sed "1 d")

mongosh -u myAtlasDBUser -p myatlas-001 $MY_ATLAS_CONNECTION_STRING


```

## Common Errors and Troubleshooting for Connection Errors

`MongoServerSelectionError: connection <monitor> to 34.239.188.169:27017 closed`
- check if IP address is allowlisted in Atlas, network connection timed out
`MongoServerError: bad auth : Authentication failed.`
- check the username and password in the connection string
- validate credentials are correct

## MongoDB CRUD Operations
### Inserting Documents
automatically creates the collection if it doesn't exist
```python
insertOne()
db.<collection>.insertOne()

db.grades.insertOne({
  student_id: 654321,
  products: [
    {
      type: "exam",
      score: 90,
    },
    {
      type: "homework",
      score: 59,
    },
    {
      type: "quiz",
      score: 75,
    },
    {
      type: "homework",
      score: 88,
    },
  ],
  class_id: 550,
})

```

```shell
insertMany()
db.<collection>.insertMany()

db.grades.insertMany([
  {
    student_id: 546789,
    products: [
      {
        type: "quiz",
        score: 50,
      },
      {
        type: "homework",
        score: 70,
      },
      {
        type: "quiz",
        score: 66,
      },
      {
        type: "exam",
        score: 70,
      },
    ],
    class_id: 551,
  },
  {
    student_id: 777777,
    products: [
      {
        type: "exam",
        score: 83,
      },
      {
        type: "quiz",
        score: 59,
      },
      {
        type: "quiz",
        score: 72,
      },
      {
        type: "quiz",
        score: 67,
      },
    ],
    class_id: 550,
  },
  {
    student_id: 223344,
    products: [
      {
        type: "exam",
        score: 45,
      },
      {
        type: "homework",
        score: 39,
      },
      {
        type: "quiz",
        score: 40,
      },
      {
        type: "homework",
        score: 88,
      },
    ],
    class_id: 551,
  },
])
```

### Finding Documents
- operators
	- `$eq`
		- implicit
	- `$in`
	- `$gt`
	- `$lt`
	- `$gte`
	- `$lte`
	- `$elemMatch`
`<field>: { <operator>: <value>}`

```python
db.<collection>.find({field: value})
db.<collection>.find({field: {$eq: "value"}})
db.sales.find({_id: ObjectId("123abc")})


db.<collection>.find({field: {$in: ["val1", "val2"]}})
db.sales.find({ storeLocation: { $in: ["London", "New York"] } })

db.sales.find({ "items.price": { $gt: 50}})
db.sales.find({ "items.price": { $lt: 50}})
db.sales.find({ "customer.age": { $lte: 65}})
db.sales.findOne({ "customer.age": { $gte: 65}})

db.accounts.find({ products: "InvestmentFund"})


db.sales.find({
  items: {
    $elemMatch: { name: "laptop", price: { $gt: 800 }, quantity: { $gte: 1 } },
  },
})

db.routes.find({ "airline.name": "Southwest Airlines", stops: { $gte: 1 } })

db.routes.find({
  $or: [{ dst_airport: "SEA" }, { src_airport: "SEA" }],
})

db.routes.find({
  $and: [
    { $or: [{ dst_airport: "SEA" }, { src_airport: "SEA" }] },
    { $or: [{ "airline.name": "American Airlines" }, { airplane: 320 }] },
  ]
})

db.sales.find({ couponUsed: true, purchaseMethod: "Online", "customer.age": { $lte: 25 } })

db.sales.find({$or: [
	{"items.name": "pens"},
	{"items.tags": "writing"}	
]})

```

`db.books.find({ genre: "Historical" })`
- can return with scalar value or value in the array
	-  `["Historical", "Fiction"]`
	- `"Historical"`
- when including the same operator more than once in your query, you need to the use explicit `$or` operator

### Replace and Delete Documents

`replaceOne() has three parameters`
- `filter`: query that matches the document to replace
- `replacement`: The new document to replace the old one with.
- `options`: An object that specifies options for the update.

```python
replaceOne()

db.books.replaceOne(
  {
    _id: ObjectId("6282afeb441a74a98dbbec4e"),
  },
  {
    title: "Data Science Fundamentals for Python and MongoDB",
    isbn: "1484235967",
    publishedDate: new Date("2018-5-10"),
    thumbnailUrl:
      "https://m.media-amazon.com/images/I/71opmUBc2wL._AC_UY218_.jpg",
    authors: ["David Paper"],
    categories: ["Data Science"],
  }
)

```

`updateOne() has three parameters`
- `filter`: query that matches the document to update
- `update`: The update to make to the document
- `options`: An object that specifies options for the update.
	- `upsert` (update or insert)
		- insert a document with the provided information if matching documents don't exist

	Operators:
	- `$set`
		- Adds new fields and values to a document
		- Replaces the value of a field with a specified value
	- `$push`
		- appends a value to an array
		- Adds the array field with the value as an element if no array present
		- `$each`
			- Add each value to an array
			- `{ $push: { <field>: { $each: [ <value1>, <value2> ... ] } } }`
``` python
db.podcasts.updateOne(
  {
    _id: ObjectId("5e8f8f8f8f8f8f8f8f8f8f8"),
  },

  {
    $set: {
      subscribers: 98562,
    },
  }
)

db.podcasts.updateOne(
  { title: "The Developer Hub" },
  { $set: { topics: ["databases", "MongoDB"] } },
  { upsert: true }
)

db.podcasts.updateOne(
  { _id: ObjectId("5e8f8f8f8f8f8f8f8f8f8f8") },
  { $push: { hosts: "Nic Raboy" } }
)
```

```python
db.birds.updateOne(
{_id: ObjectId("6268413c613e55b82d7065d2"},
{
	$set: {tags: ["geese", "herbivore", "migration"]}},
)
```

`findAndModify()`
- `filter`: query that matches the document to update
- `replacement`: The document to replace the current with
- `options`: An object that specifies options for the update.

```python
db.podcasts.findAndModify({
  query: { _id: ObjectId("6261a92dfee1ff300dc80bf1") },
  update: { $inc: { subscribers: 1 } },
  new: true,
})
```

`updateMany()`
- `filter`: query that matches the document to update
- `update`: The update to make to the document
- `options`: An object that specifies options for the update.
```python
db.books.updateMany(
  { publishedDate: { $lt: new Date("2019-01-01") } },
  { $set: { status: "LEGACY" } }
)

db.birds.updateMany({$or: [{}, {}]})
```
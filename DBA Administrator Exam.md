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

db.birds.updateMany(
	{$or: [ {name: "abc"}, {name: "xyz"} ] },
	{ $set: { status: "LEGACY" } }
)
```

`deleteOne()`
```python
db.podcasts.deleteOne({ _id: Objectid("6282c9862acb966e76bbf20a") })
```

`deleteMany()`
```python
db.podcasts.deleteMany({category: “crime”})
```

## Modifying Query Results
### Sorting and Limiting Query Results in MongoDB
#### Sorting Results
- Descending order: `-1`
- Ascending order: `1`
```python
db.collection.find(<query>).sort(<sort>)

db.companies.find({ category_code: "music" }).sort({ name: 1 });

db.companies.find({ category_code: "music" }).sort({ name: 1, _id: 1 });

```

#### Limiting Results
```python
db.companies.find(<query>).limit(<number>)
db.companies.find() # for all companies

// Return the three music companies with the highest number of employees. Ensure consistent sort order.
db.companies
  .find({ category_code: "music" })
  .sort({ number_of_employees: -1, _id: 1 })
  .limit(3);

db.sales.find({storeLocation: "London", "items.name": {$in: ["laptop", "backpack", "printer paper"]}})
```


### Returning Specific Data from a Query in MongoDB
```
db.collection.find( <query>, <projection> )
```

#### Include a Field
```python
db.collection.find( <query>, { <field> : 1 })

// Return all restaurant inspections - business name, result, and _id fields only
db.inspections.find(
  { sector: "Restaurant - 818" },
  { business_name: 1, result: 1 }
)

db.sales.find({ storeLocation: "Denver", }, { storeLocation: 1, saleDate: 1, purchaseMethod: 1 })
```

#### Exclude a Field
```python
db.collection.find(query, { <field> : 0, <field>: 0 })


// Return all inspections with result of "Pass" or "Warning" - exclude date and zip code
db.inspections.find(
  { result: { $in: ["Pass", "Warning"] } },
  { date: 0, "address.zip": 0 }
)

db.sales.find({ "customer.age": { $lt: 30 }, "customer.satisfaction": { $gt: 3 }, }, { "customer.satisfaction": 1, "customer.age": 1, "storeLocation": 1, "saleDate": 1, "_id": 0, });

db.sales.find({ storeLocation: { $in: ["Seattle", "New York"] } }, {purchaseMethod: 0, customer: 0, couponUsed: 0})
```

### Counting Documents in a MongoDB Collection
```python
db.collection.countDocuments( <query>, <options> )

// Count number of docs in trip collection
db.trips.countDocuments({})

db.trips.countDocuments({ tripduration: { $gt: 120 }, usertype: "Subscriber" })

db.sales.countDocuments({ items: { $elemMatch: { name: "laptop", price: { $lt: 600 } } } } ) # when querying for multiple items to be true, need #elemMatch
```

## MongoDB Aggregation
- Aggregation: collection and summary of data
- Stage: built-in method to compute data but not alter, akin to stream
- Aggregation pipeline: series of stages completed on the data in order
```python
db.collection.aggregate([
    {
        $stage1: {
            { expression1 },
            { expression2 }...
        },
        $stage2: {
            { expression1 }...
        }
    }
])
```

### `$match`
```
{
  $match: {
     "field_name": "value"
  }
}
```
### `$group`
```
{
  $group:
    {
      _id: <expression>, // Group key
      <field>: { <accumulator> : <expression> }
    }
 }
```

```python
db.zips.aggregate([
{   
   $match: { 
      state: "CA"
    }
},
{
   $group: {
      _id: "$city",
      totalZips: { $count : { } }
   }
}
])

db.sightings.aggregate([ { $match: { species_common: 'Eastern Bluebird' } }, { $group: { _id: '$location.coordinates', number_of_sightings: { $count: {} } } } ])
```
### `$sort`
```
{
    $sort: {
        "field_name": 1
    }
}
```

### `$limit`
```
{
  $limit: 5
}
```

```
db.zips.aggregate([
{
  $sort: {
    pop: -1
  }
},
{
  $limit:  5
}
])


```

### `$project`
The `$project` stage specifies the fields of the output documents.
```
{
    $project: {
        state:1, 
        zip:1,
        population:"$pop",
        _id:0
    }
}
```

### `$count`
The `$count` stage creates a new document, with the number of documents at that stage in the aggregation pipeline assigned to the specified field name.
```
{
  $count: "total_zips"
}
```

### `$set`
The `$set` stage creates new fields or changes the value of existing fields, and then outputs the documents with the new fields.
```
{
    $set: {
        place: {
            $concat:["$city",",","$state"]
        },
        pop:10000
     }
  }
```

```
db.sightings.aggregate([ { $project: { _id: 0, species_common: 1, date: 1 } } ])

db.birds.aggregate([ { $set: { 'class': 'bird' } } ])
```

### `$out`
Write the documents that are returned by an aggregation pipeline into a collection
- must be the last stage
```
{
    $set: {
        place: {
            $concat:["$city",",","$state"]
        },
        pop:10000
     }
  }

db.sightings.aggregate([ { $match: { date: { $gte: ISODate('2022-01-01T00:00:00.0Z'), $lt: ISODate('2023-01-01T00:00:00.0Z') } } }, { $out: 'sightings_2022' } ]) db.sightings_2022.findOne()

```
## MongoDB Indexes
- Indexes improve query performance at the cost of write performance. For most use cases, this tradeoff is acceptable. Indexes should be used on data that is frequently queried or on queries that are infrequent but costly in terms of computational resources.
- **Compound index:** MongoDB supports compound indexes, where a single index structure holds references to multiple fields within a collection's documents. A compound index is created by specifying the fields that the index should reference, followed by the order in which the fields should be sorted. The order of the fields in the index is important because it determines the order in which the documents are returned when querying the collection. A compound index can also be a multikey index if one of the fields is an array.
- **Single field index:** A single field index is an index on a single field of a document. MongoDB creates a single field index on the `_id` field by default, but additional indexes may be needed for other fields as well. A single field index can also be a multikey index if it operates on an array field.
	-  Once the unique index is created, any inserts or updates including duplicated values in the collection for the index field/s will fail.
```python
db.customers.createIndex({
  birthdate: 1
})

db.customers.createIndex({
  email: 1
},
{
  unique:true
})

db.customers.getIndexes()

```
- **Multikey index:** A multikey index is an index on an array field. Each element in the array gets an index key, which supports efficient querying against array fields. Both single field and compound indexes can have an array field, so there are both multikey single field indexes and multikey compound indexes.
### Check index being used
Use `explain()` in a collection when running a query to see the Execution plan. This plan provides the details of the execution stages (IXSCAN , COLLSCAN, FETCH, SORT, etc.).

- The `IXSCAN` stage indicates the query is using an index and what index is being selected.
- The `COLLSCAN` stage indicates a collection scan is perform, not using any indexes.
- The `FETCH` stage indicates documents are being read from the collection.
- The `SORT` stage indicates documents are being sorted in memory.
```python
db.customers.explain().find({
  birthdate: {
    $gt:ISODate("1995-08-01")
    }
  })

db.customers.explain().find({
  birthdate: {
    $gt:ISODate("1995-08-01")
    }
  }).sort({
    email:1
    })
```

### Multikey Indexes
Use `createIndex()` to create a new index in a collection. Include an object as parameter that contains the array field and sort order. In this example `accounts` is an array field.
- Multikey indexes support efficient queries against array fields by creating an index key for each element in the array. This allows MongoDB to search for the index key of each element in the array rather than scan the entire array, which results in dramatic performance gains in your queries.
- The maximum number of array fields per multikey index is 1. If an index has multiple fields, only one of them can be an array.

```
db.customers.createIndex({
  accounts: 1
})
```

### Compound Indexes
Use `createIndex()` to create a new index in a collection. Within the parentheses of `createIndex()`, include an object that contains two or more fields and their sort order.

```
db.customers.createIndex({
  active:1, 
  birthdate:-1,
  name:1
})
```

The order of the fields matters when creating the index and the sort order. It is recommended to list the fields in the following order: Equality, Sort, and Range.

- Equality: field/s that matches on a single field value in a query
- Sort: field/s that orders the results by in a query
- Range: field/s that the query filter in a range of valid values

The following query includes an equality match on the active field, a sort on birthday (descending) and name (ascending), and a range query on birthday too.

```
db.customers.find({
  birthdate: {
    $gte:ISODate("1977-01-01")
    },
    active:true
    }).sort({
      birthdate:-1, 
      name:1
      })
```

Here's an example of an efficient index for this query:

```
db.customers.createIndex({
  active:1, 
  birthdate:-1,
  name:1
})
```

### Deleting Index
Use `dropIndex()` to delete an existing index from a collection. Within the parentheses of `dropIndex()`, include an object representing the index key or provide the index name as a string.

Delete index by name:

```
db.customers.dropIndex(
  'active_1_birthdate_-1_name_1'
)
```

Delete index by key:

```
db.customers.dropIndex({
  active:1,
  birthdate:-1, 
  name:1
})
```

Use `dropIndexes()` to delete all the indexes from a collection, with the exception of the default index on _id.

```
db.customers.dropIndexes()
```

The `dropIndexes()` command also can accept an array of index names as a parameter to delete a specific list of indexes.

```
db.collection.dropIndexes([
  'index1name', 'index2name', 'index3name'
  ])
```

The `hideIndex()` command hides an index. By hiding an index, you'll be able to assess the impact of removing the index on query performance. MongoDB does not use hidden indexes in queries but continues to update their keys. This allows you to assess if removing the index affects the query performance and unhide the index if needed. Unhiding an index is faster than recreating it. In this example, you would use the command `db.customers.hideIndex({email:1})`.
## MongoDB Atlas Search

## MongoDB Data Modeling Intro
![[Pasted image 20230818123605.png]]

## MongoDB Shell

```sh
apt update 
apt install <code>gnupg</code>
wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -
```

```sh
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu 
focal/mongodb-org/6.0 multiverse" | sudo tee 
/etc/apt/sources.list.d/mongodb-org-6.0.list
```

```sh
apt update
apt install -y mongodb-mongosh
```

```
config
```

The available configuration options are as follows:

```
- `displayBatchSize` - The number of documents to display when using the `it` iterator.
- `maxTimeMS` - The maximum amount of time to allow a query to run.
- `enableTelemetry` - Whether to enable telemetry.
- `editor` - The editor to use when editing code.
- `snippetIndexSourceURLs` - The URLs to use when fetching snippet index files.
- `snippetRegistryURL` - The URL to use when fetching snippet registry files.
- `snippetAutoload` - Whether to automatically load snippets.
- `inspectCompact` - Whether to use compact mode when inspecting objects.
- `inspectDepth` - The maximum depth to use when inspecting objects.
- `historyLength` - The number of history entries to keep.
- `showStackTraces` - Whether to show stack traces when errors occur.
- `redactHistory` - Whether to redact sensitive information from history.
```

`config.get(<field>) -> config.get('enableTelemetry') returns the bool value
`config.set(<field>)` to set config values

```
- Windows: `mongosh.cfg`, in the same directory as the `mongosh.exe` binary.
- macOS:
 - `/usr/local/etc/mongosh.conf`
 - `/etc/mongosh.conf`
 - `/opt/homebrew/etc/mongosh.conf`
- Linux: `/etc/mongosh.conf`
```

Finally, you can set configuration options in `mongosh` by using the `--eval` flag with a command. For example, to disable telemetry, you would run the following command to invoke the `disableTelemetry()` function:

```
mongosh --eval "disableTelemetry()"
```

You can also use the `--eval` flag to run queries and other commands. For example, to run a query for some documents, you would run the following command:

```
mongosh --eval "db.accounts.find().limit(3)" --quiet
```

To run an external script in `mongosh`, use the `load()` method. For example, to run the `randomPost.js` file, you would run the following code:

```
load('randomPost.js')
```

Within your script, you can use the `db.getSiblingDB()` method to access a database without having to switch to it in `mongosh`. For example, here’s how you would access the `sample_training` database in the `randomPost.js` script:

```
db = db.getSiblingDB("sample_training");
console.log(`\nCurrent database: ${db.getName()}`);
console.log("Random post sample of 500 words:\n");
let result = db.posts.aggregate({
 $sample: { size: 1 },
});
printjson(result.next().body.slice(0, 500) + " ...");
```

To edit a function or command in an external text editor, like Vim or nano, while using `mongosh`, use the `edit` command. To use this command, you must first set the `config.editor` value in `mongosh`. To do so, use the `config.set()` method:

```
config.set("editor", "vim")
```

Once the editor is set, you can then use `edit` to modify a new or existing command. For example, to edit the `giveMeADate` function, you would run the following code:

```
edit giveMeADate
```
# Mongo DBA Specific Content


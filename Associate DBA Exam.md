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
```
insertOne()
db.<collection>.insertOne()
```

```
insertMany()
db.<collection>.insertMany()
```

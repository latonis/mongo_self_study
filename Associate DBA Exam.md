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
### 
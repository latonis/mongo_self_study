[MongoDB Associate Developer Exam](https://learn.mongodb.com/pages/mongodb-associate-developer-exam)

## Inserting Documents

### Inserting One
```python
# Get reference to 'bank' database
db = client.bank

# Get reference to 'accounts' collection
accounts_collection = db.accounts

new_account = {
    "account_holder": "Linus Torvalds",
    "account_id": "MDB829001337",
    "account_type": "checking",
    "balance": 50352434,
    "last_updated": datetime.datetime.utcnow(),
}

# Write an expression that inserts the 'new_account' document into the 'accounts' collection.
result = accounts_collection.insert_one(new_account)

document_id = result.inserted_id
print(f"_id of inserted document: {document_id}")

client.close()
```

### Inserting Many
```python

# Get reference to 'bank' database
db = client.bank

# Get a reference to 'accounts' collection
accounts_collection = db.accounts

new_accounts = [
    {
        "account_id": "MDB011235813",
        "account_holder": "Ada Lovelace",
        "account_type": "checking",
        "balance": 60218,
    },
    {
        "account_id": "MDB829000001",
        "account_holder": "Muhammad ibn Musa al-Khwarizmi",
        "account_type": "savings",
        "balance": 267914296,
    },
]

# Write an expression that inserts the documents in 'new_accounts' into the 'accounts' collection.
result = accounts_collection.insert_many(new_accounts)

document_ids = result.inserted_ids
print("# of documents inserted: " + str(len(document_ids)))
print(f"_ids of inserted documents: {document_ids}")

client.close()
```

## Querying

### Single Document
```python
# Get reference to 'bank' database
db = client.bank

# Get a reference to the 'accounts' collection
accounts_collection = db.accounts

# Query by ObjectId
document_to_find = {"_id": ObjectId("62d6e04ecab6d8e1304974ae")}

# Write an expression that retrieves the document matching the query constraint in the 'accounts' collection.
result = accounts_collection.find_one(document_to_find)
pprint.pprint(result)

client.close()
```

### Multiple Documents
```pytho
```
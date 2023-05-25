# MongoDB

## Install
```shell
# Mac M2
brew tap mongodb/brew
brew update
brew install mongodb-community@6.0

#  Installed files
## 1. Servers: mongod_
## 2. Sharded Cluster Query Router: mongos
## 3. MogoDB Shell：mongosh

# The configuration file generated after installation
## 1. /opt/homebrew/etc/mongod.conf
## 2. /opt/homebrew/var/log/mongodb
## 3. /opt/homebrew/var/mongodb

# Running MongoDB (mongod) as a service for MacOS
brew services start mongodb-community@6.0
# Stop mongod from running as a MacOS service
brew services stop mongodb-community@6.0

# Backend Launch
mongod --config /opt/homebrew/etc/mongod.conf --fork    # MacOS M2
mongod --config /usr/local/etc/mongod.conf --fork       # MacOS Intel

# Verify that MongoDB is running
## 1. service operation
brew services list
## 2. backend operation
ps aux | grep -v grep | grep mongod

# Connecting to and using MongoDB
mongosh

# MongoDB database tools
mongotop
```
## What is a Database?
- Database is a collection of data
- In MongoDB context:
    - Database can also be described as a physical container for collections.
    - A Database can have any number of collections.
    - Each database gets it's own set of files on the file system.
    - A MongoDB server can host multiple databases inside it.
## What is a Collection?
- Collection is a group of MongoDB Documents.
- You can relate a collection as a "Table" `Not literally but hypothetically`.
- Unlike tables, Collections does not have any schema definition.
- Unlike RDBMS Database - the collections DO NOT have any concept of JOINS `However we can achieve joins functionality using Aggregations in MongoDB`.
## What is a Document?
- Document in MongoDB is a set of "key-value" Pairs.
- Every document in MongoDB has a unique value via Key "_id".
- Documents have flexible and synamic schema.
- Document shcema is user-defined and is not Fixed or Static.
- Documents can hold any data as along as they are valid data types in MongoDB.
- Documents within a collection can have different schema or fields.
- Documents within a collection are related data belonging to a particular subject.
## Compare
```txt
## RDBMS
    - Relational Database Management Systems
    - Tables, Columns and Rows
    - Table
        - stands for entity
    - Columns
        - Fields inside the table. e.g Person
    - Rows - Actual Data

## MongoDB
Database
    - Single collection or more collections
    - 1 or More collections together become a database

Collections
    - Collections do NOT enforce any schema
        - NO joins concept
    - We can join multiple collections using Aggeration
    
    - Its a set of Documents
    - Can have any number of document
    - Documents can have any dynamic schema
        - they can be same or different

Documents
    - is a simple JSON key-value pair data
    - MongoDB valid document example
    - Document can have any type - as long as it is valid MongoDB data type
    - schema can be diffrent for diffrent documents
    - User defined schema in MongoDB and they are NOT static or fixed
    - MongoDB will add a key automatically for each document
        - _id
    {
        "_id": "<unique-value>",
        "Firstname": "Du",
        "Lastname": "Emory"
        "Email": "example@test.com"
    },
    {
        "Firstname": "Du",
        "Lastname": "Emory"
        "Email": "example@test.com"
        "Address": "Shanghai"
    }

```
## Create and Drop Databases
- Creating Databases
```shell
user <database-name>
# The newly created database will NOT visible unless we insert any document inside it.
# Make sure we see the message "switched to db" <database-name>
```
- Check your working space DB
- To remove Database use the command - db.dropDatabase()
- show database 
    - list all databases in MongoDB
- use <database-name> 
    -  select the database workspace as current working DB
    - if not, it will create but it will not display till we add some collections/data inside it 
- db 
    - show you the database you are working currently in 
- To drop a database, First we need to select the DB
    - use <database-name>
    - db.dropDatabase();

## Create and Drop Collections
- Creating and Dropping Collections
    - Creating Collections
```shell
db.createCollection(name, options)
```
- Dropping Collections
```shell
db.collection.drop()
```
- To create a new collection
    - db.createCollection("<name of collection">);
- To drop a collection
    - db.collectionName.drop()
## Data Types in MongoDB
- BSON
- JSON
- Integer
- Boolean
- Double
- Arrays
- Object
- Null
- Date
- Timestamp
- Object Id
- Code
## BSON and JSON
- Difference between BSJON and JSON
    - JSON based databases usually return query results which can be effortlessly\
    parsed, having modest or nix transformation, straightforwardly by the use of \
    JavaScript along with most well-liked programming languages.
    - In the case of MongoDB, data representation is done in JSON document\
    format, but here the JSON is binary-encoded, which is termed as BSON.
    - BSON is the extended version of the JSON model, which is providing\
    additional data types, makes performance to be competent to encode and\
    decode in diverse languages and ordered fields.
> JSON: 
> - {"name": "emorydu", "address": "Shanghai"}
> - esay to parse, easy to render, most languages

> BSON: 
> - MongoDB data type
>   - store and process data
> - Binary-encoded JSON = BSON
> - And it has some extended data types which are not supported by JSON
>   - Date
>   - Timestamp
>   - Object Id

## Insert Document
- Inserting Documents into collections
    - Insert is used for creating new documents inside the collection.
    - To insert any document into Collection.
    ```shell
    db.<collection-name>.insert({ "name": "emorydu"})
    ```
    - To insert many documents at once into collection.
    ```shell
    db.<collection-name>.insertMany([
        { "name": "emorydu" },
        { "name": "lin" }
    ])
    ```
    - We can insert any number of documents into a collection
    - We can insert one or more documents into the collection

    - Ever document that we insert will have a unique key "_id"
        - the value for this key is always unique and 24 character
        - _id as a Primary Key in your collection

    - Can we change the value of _id??
        - Yes, we can change it
        - But - 
            - it is tricky
        - `DO NOT DO THIS` - change the _id value
## Update Document
- Update any document into Collection
    - Update can be applied with 
        - update
        - updateOne
        - updateMany
```shell
db.<collection-name>.update(
    { "name": "emorydu" },
    {
        $set: {
            "key": "value"
        }
    });

db.<collection-name>.updataMany(
    {"isActive": true},
    {
        $set: {
            "isActive": false
        }
    }
)
```
## Read Data From Collections
- Multiple ways to Read data from collection
    - find() - finds all documents in collection
    ```shell
    db.collection.find()
    ```
    - findOne() - find first document in collection
    - find({"key1": "value1", "key2": "value2"}) - by setting query conditions
    - findOneAndReplace({"key1": "value1", "key2": "value2"}, <replacement>)
    - findOneAndDelete({"key1": "value1", "key2": "value2"})

- find() // return all the documents
- findOne() // return only 1st document
- find({"key1": "value1"});
- findOneReplace({condition}, {replecement});
- findOneDelete({condition});
## Delete documents from Collections
- Multiple ways to Delete data from collection
    - deleteOne() - find all documents in collection
    ```shell
    db.collection.find()
    Example: db.orders.deleteOne({"_id": ObjectId("asdfa32413dsfasd...")});
    ```
    - deleteMany({condition})
        - Will delete many documents at once
        - When passed with empty curly brace - it will delete all documents in collections
        - Example: db.orders.deleteMany({condition});
- deleteOne()
    - will only delete one document based on creiteria
- deleteMany()
    - pass multiple conditions to satify the document search and delete them
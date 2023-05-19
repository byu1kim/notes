+++
title = "MongoDB"
weight = 3
pre = "<i class='fas fa-pen'></i> &nbsp"
+++

Mongo DB is No SQL Databases that stores data as documents in a JSON-like format.

| MongoDB  | SQL         |
| -------- | ----------- |
| Database | Database    |
| Tables   | Collections |
| Columns  | Fields      |
| Rows     | Documents   |

## Install

https://www.mongodb.com/nosql-explained/nosql-vs-sql

- Local : MongoDB Community Server
- Cloud : MongoDB Atlas

Commands in Local machine

```
show databases
use nameOfDatabase
insert : db.table.insertMany([ {}, {}, {} ])
list : db.table.find()
```

## MongoDB in Node

> #### Get connection string

`MongoDB > Database > Connect > Connect your application`

> #### Configuration

```
npm install mongodb mongoose
```

```js
const { mongoose } = require("mongoose");
const Profile = require("./models/Profile.js");

const uri = ".../DBNAME?..."; // put DBname prefix ? inside connection string

// set up default mongoose connection, use .env for your uri
mongoose.connect(uri, { useNewUrlParser: true, useUnifiedTopology: true });

// store a reference to the default connection
const db = mongoose.connection;

// Bind connection to error event (to get notification of connection errors)
db.on("error", console.error.bind(console, "MongoDB connection error:"));

// Once we have our connection, let's load and log our profiles
db.once("open", async function () {
  const profiles = await await Profile.find({}); // if we don't close the db connection, our app will keep running
  db.close();
});
```

## Mongoose

Mongoose is a Node.js-based Object Data Modeling(ODM) library for MongoDB. Mongoose allows us to define data models using **schemas** and provides and interface to interact with the MongoDB database using these models.

It simeplifies the interation with MongoDB by providing a schemabased solution, validation, middelware, and other utilities.

- ODM : Object Document Mapping (\*ORM: Object Relational Mapping in Sql Database)
- ODM is a technique to perform CRUD operations between an application and a database system
- Mongoose is mapping Node.js classes to MongoDB collection(table)
- Schema defines the fields
- Use schema as the basis of the Model

> #### Schema

```js
import mongoose from "mongoose";

const schema = new mongoose.Schema({
  name: { type: String, required: true },
  created: { type: Date, required: true, default: Date.now },
  obj: { type: mongoose.Schema.Types.ObjectId, ref: "CollectionName" },
});

const Schema = mongoose.model("CollectionName", schema);
export default Schema; // Collection name as singular form
```

## Mongoose Query

`const Model = mongoose.model('Model', schema)`

> #### Create

`Model.create({schema...})`

`Model.object.add({})`

Add the relationship

> #### Read

`Model.find()`

`Model.findById()`

`Model.findOne()`

`Model.populate()`

> #### Update

`Model.save()`

`Model.findByIdAndUpdate()`

`Model.findOneAndUpdate()`

> #### Delete

`Model.findByIdAndDelete()`

`Model.findOneAndDelete()`

```js
const doc = await ModelName.findOne();
doc.name = "";

doc.save();
```

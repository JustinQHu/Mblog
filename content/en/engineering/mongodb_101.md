---
title: "MongoDB 101: A Beginner’s Guide to NoSQL Databases"
date: 2025-03-21T14:56:46-04:00
lastmod: 2025-03-21T14:56:46-04:00
author: ["Justin Hu"]
categories: ['engineering']
tags: ['MongoDB', 'NoSQL', 'Non-relational DB', 'Nonrelational DB', 'MongoDB Shell', 'MongoDB Compass']
description: "A Beginner’s Guide to NoSQL Database MongoDB"
draft: false # draft or not
cover:
    image: "/engr/mongo/mongodb.jpg"
    caption: "MongoDB 101: A Beginner’s Guide to NoSQL Databases"
    alt: "MongoDB 101: A Beginner’s Guide to NoSQL Databases"
    relative: false
---


If you’re diving into the world of databases, you’ve likely heard of [MongoDB](https://www.mongodb.com/). It’s one of the most popular NoSQL databases out there, and for good reason—it’s flexible, scalable, and developer-friendly. In this MongoDB 101 guide, we’ll break down what MongoDB is, how it works, and why it might be the right choice for your next project.

## What is MongoDB?

MongoDB is an open-source, document-oriented NoSQL database designed to handle unstructured or semi-structured data. Unlike traditional relational databases (like MySQL or PostgreSQL), which store data in rigid tables with rows and columns, MongoDB uses a more dynamic, JSON-like structure called BSON (Binary JSON). This makes it ideal for applications where data doesn’t fit neatly into a predefined schema.
Launched in 2009 by MongoDB Inc., it’s become a go-to solution for startups, enterprises, and developers who need a database that can scale horizontally and adapt to changing requirements.

## Key Concepts of MongoDB

To understand MongoDB, you need to grasp a few core concepts:

1. **Documents**

    *Document* is the basic unit of data in MongoDB is a document, which is a set of key-value pairs, similar to a JSON object.

    For example:

    ```json
    {
    "name": "Alice",
    "age": 29,
    "city": "New York"
    }
    ```

    Each document can have a unique structure—no need for every document to follow the same schema.

2. **Collections**

    Documents are grouped into *collections*, which are analogous to tables in a relational database. However, collections don’t enforce a fixed structure, so documents within a collection can vary widely.

3. **Databases**

    A MongoDB instance can host multiple *databases*, each containing its own set of collections.

4. **BSON**

   MongoDB stores data in *BSON*, a binary representation of JSON. This format supports additional data types (like dates and binary data) and makes querying faster.

## Why Use MongoDB?

So, why choose MongoDB over a traditional relational database? Here are some compelling reasons:

- **Flexibility**
  - No strict schema means you can evolve your data model as your application grows. Add new fields to documents without breaking anything.
- **Scalability**
  - MongoDB is built to scale horizontally by adding more servers (sharding), making it perfect for handling large datasets and high traffic.
- **Speed**
  - Its document-based design allows for fast reads and writes, especially for complex queries.
- **Developer-Friendly**
  - With support for languages like JavaScript, Python, and Java, and a syntax that feels natural to modern developers, MongoDB fits seamlessly into many tech stacks.

## How Does MongoDB Work?

At its core, MongoDB is all about storing and retrieving data efficiently. Here’s a quick rundown of how it operates:

1. Installation: You can download MongoDB from its official site and run it locally, run it in containerized way, or use a cloud-hosted version like MongoDB Atlas.
2. CRUD Operations: MongoDB supports Create, Read, Update, and Delete (CRUD) operations. For example:
   - Create: ```db.users.insertOne({"name": "Bob", "age": 25})```
   - Read: ```db.users.find({"age": 25})```
   - Update: ```db.users.updateOne({"name": "Bob"}, {"$set": {"age": 26}})```
   - Delete: ```db.users.deleteOne({"name": "Bob"})```
3. Indexing: To speed up queries, MongoDB uses indexes, much like a book’s index helps you find pages faster.
4. Aggregation: MongoDB’s aggregation pipeline lets you process and transform data—like grouping, filtering, or calculating averages—directly in the database.

## When to Use MongoDB?

MongoDB shines in scenarios like:

- **Content Management**: Storing articles, user profiles, or comments with varying structures.
- **Real-Time Analytics**: Handling large volumes of data from IoT devices or user interactions.
- **E-Commerce**: Managing product catalogs with diverse attributes (e.g., a phone has a "camera" field, but a shirt doesn’t).
- **Prototyping**: Quickly building apps without worrying about a rigid schema upfront.
  
However, it’s not a silver bullet. If your application relies heavily on complex transactions or relationships (like banking systems), a relational database might be a better fit.

## Getting Started with MongoDB

Ready to try it out? Here’s a simple example:

### 1. Install MongoDB  

Multiples ways to install MongoDB:

- Local Installation:  Download it from mongo [website](https://www.mongodb.com/) and install it
- Container App: Pull an image of Mongo from Docker's Container Hub and run the service
- Cloud:  use [MongoDB Atlas](https://www.mongodb.com/products/platform/atlas-database) for a cloud setup.  
  
Here we use the container app approach to download a MongoDB docker image from the [Docker Hub](https://hub.docker.com/).
![MongoDB](/engr/mongo/mongodb-1.png)

### 2. Run the Service

Once we have pulled mongo image to local environment,  we can run the container with the docker run button with Docker GUI or with the commmand:

```shell
docker run --name mongo-test -d mongo:latest
```

we should see a mongo container running:
![MongoDB](/engr/mongo/mongodb-2.png)

Exec into the container and use ```ps``` command to verify if mango is running:

![MongoDB](/engr/mongo/mongodb-3.png)

### 3. Connect

Use the [MongoDB shell](https://www.mongodb.com/try/download/shell) or a GUI DB tool like [MongoDB Compass](https://www.mongodb.com/docs/compass/current/) to interact with Mongo

1. MongoDB Shell  
    To connect to a MongoDB deployment running on localhost with default port 27017, run mongosh without any options from the shell of the container:  
    ```mongosh```  
    It's equivalent to the following command:  
    ```mongosh "mongodb://localhost:27017"```
    ![MongoDB](/engr/mongo/mongodb-4.png)
2. MongoDB Compass  
    An GUI tool that is easy to use.
    ![MongoDB](/engr/mongo/mongodb-compass.png)

### 4. Experiment

Create a database, add some documents, and play with queries.
For example, in the MongoDB shell:

```shell
use mydb
db.people.insertOne({"name": "Charlie", "hobby": "coding"})
db.people.find()
```

![MongoDB](/engr/mongo/mongodb-5.png)

Dive deeper into [MongoDB Shell Commands](https://www.mongodb.com/docs/mongodb-shell/run-commands/).

## Wrapping Up

MongoDB is a powerful tool in the modern developer’s toolkit, offering a flexible and scalable alternative to traditional databases. Whether you’re building a startup app, managing big data, or just experimenting, MongoDB is a great starting point. As you dig deeper, you’ll discover features like replication, sharding, and geospatial queries that make it even more versatile.

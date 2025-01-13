# MongoDB Commands Guide

**_All MongoDb commands you will ever need (MongoDb Cheatsheet)
In this post, we will see a comprehensive list of all the MongoDB commands. This list is not complete and is imposible to put the entire docs. This are the most used commands._**

#### If you have not completed the installation follow this [Installation](Installation.md) guide.

# Database Commands
----
### View all databases

```
show dbs
```

### Create a new or switch databases

```
use dbName
```

### View current Database

```
db
```

### Delete Database

```
db.dropDatabase()
```
----
# Collection Commands

### Show Collections

```
show collections
```

### Create a collection named 'content'

```
db.createCollection('content')
```

### Drop a collection named 'content'

```
db.content.drop()
```

# CRUD operations Row(Document) Commands
----
## C - create operations
### Insert One Row

```
db.content.insertOne({
    'name': 'tirtha',
    'lang': 'Python',
    'joined': 5
 })
```

### Insert many Rows

```
db.content.insertMany([{
    'name': 'montu',
    'lang': 'JavaScript',
    'joined': 5
    },
    {'name': 'laltu',
    'lang': 'cpp',
    'joined': 3
    },
    {'name': 'monturBap',
    'lang': 'Java',
    'joined': 4,
    'date':new Date()
}])
```

## R - find data from a collection
### Show all Rows in a Collection

```
db.content.find()
```

### Eqauality Search in a MongoDb Database

```
db.content.find({lang:'Python'})
```

### Find the first row matching the object

```
db.content.findOne({name: 'tirtha'})
```

### Find a Document by Using the ```$in``` Operator
```
db.zips.find({ city: { $in: ["PHOENIX", "CHICAGO"] } })
```


### Use of Comparison Operator

> #### Know more about [this here.](https://www.mongodb.com/docs/manual/reference/operator/query/)

```
db.content.find({joined: {$lt: 5}})
db.content.find({joined: {$lte: 5}})
db.content.find({joined: {$gt: 5}})
db.content.find({joined: {$gte: 5}})
```

### Find By Element in Array (```$elemMatch```)

```
db.content.find({
  comments: {
     $elemMatch: {
       user: 'Mary Williams'
       }
    }
  }
)
```

### Find a Document by Using Logical Operator(```$and, $or```)
>### visit for [More Info](https://docs.mongodb.com/manual/reference/operator/query-logical/)
```
db.routes.find({
  $and: [
    { $or: [{ dst_airport: "SEA" }, { src_airport: "SEA" }] },
    { $or: [{ "airline.name": "American Airlines" }, { airplane: 320 }] },
  ]
})
```






## R - Projection of data

> To specify fields to include or exclude in the result set, add a projection document as the second parameter in the call to ```db.collection.find()```.

```
db.collection.find( <query>, <projection> )
```

### Include a Field 
>To include a field set its value to 1 in the projection document.
```
db.collection.find( <query>, { <field> : 1 })
```
### Exclude a Field
> To exclude a field, set its value to 0 in the projection document.
```
db.collection.find(query, { <field1> : 0, <field1>: 0 })
```

#### Example of inclution and exclution in projections
```
db.inspections.find(
  { sector: "Restaurant - 818" },
  { business_name: 1, result: 1, _id: 0 }
)
```

### Use of ```$slice``` operator to limit array element count
> The $slice projection operator specifies the number of elements in an array to return in the query result.

```
db.posts.find( {}, { comments: { $slice: 3 } } )
```

#### To get last 3 elements
```
db.posts.find( {}, { comments: { $slice: -3 } } )
```

### Use of ```.sort()```
```
db.companies.find({ category_code: "music" }).sort({ name: 1, _id: 1 });
```


### Limit the number of rows in output with ```.limit()```

```
db.content.find().limit(2)
```

### Count the number of rows in the output

```
db.trips.countDocuments({ tripduration: { $gt: 120 }, usertype: "Subscriber" })
```
#### With ```.count()```
```
db.content.find().count()
```

## U - Update and replace functions
> #### Know more about these [Update operators](https://www.mongodb.com/docs/manual/reference/operator/update/).
### Replacing a Document in MongoDB
```
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
### Update One row

```
db.content.updateOne({name: 'tirtha'},
{$set: {'name': 'tirtha',
    'lang': 'JavaScript',
    'joined': 51
}})
```

### Update Multiple row

```
db.content.updateMany({name: 'tirtha'},
{$set: {'name': 'tirtha',
    'lang': 'JavaScript',
    'joined': 51
}})
```
### Comparison Operator with update command
```
db.content.updateMany({joined: {$lt: 5}},{$set:{fresher:true}});
```

### Update all row without any condion.

```
db.content.updateMany({},
{$set: {'name': 'tirtha',
    'lang': 'JavaScript',
    'joined': 51
}})
```

### Insert a data on update when data not present.

If you want to insert the data if the search query is not found then ```upsert:true``` will do that.

```
db.content.updateOne({name: 'ghontu'},
{$set: {'name': 'ghontu',
    'lang': 'TypeScript',
    'joined': 10
}}, {upsert: true})
```

### Add a new value to the hosts array field with ```$push``` operator 
```
db.podcasts.updateOne(
  { _id: ObjectId("5e8f8f8f8f8f8f8f8f8f8f8") },
  { $push: { hosts: "Nic Raboy" } }
)
```

### Remove array element from a array
```
db.profiles.update({ _id: 1 }, { $pull: { votes: 10}})

db.profiles.update({ _id: 1 }, 
{ $pull: { votes: { $gte: 6 }}}
)
```

#### If you have multiple items the with the same value, you should use ```$pullAll``` instead of ```$pull```.

```
collection.update(
  { _id: id },
  { $pullAll: { 'contact.phone': { number: '+1786543589455' } } }
);
```

### Updating MongoDB Documents by Using ```findAndModify()```
> The findAndModify() method is used to find and replace a single document in MongoDB.
```
db.podcasts.findAndModify({
  query: { _id: ObjectId("6261a92dfee1ff300dc80bf1") },
  update: { $inc: { subscribers: 1 } },
  new: true,
})
```


### Mongodb Increment Operator

SQL : `update tablename set joined=joined+2 where name='montu'`

```
db.content.update({name: 'Rohan'},
{$inc:{
    joined: 2
}})
```



### Mongodb ```$rename``` Operator

> Rename a field(key) in a document

```
db.content.update({name: 'ghontu'},
{$rename:{
    joined: 'Experience'
}})
```

### Remove a column/field
```
 db.content.updateMany({}, { $unset: { 'fieldname': 1}});
```

## D - Delete documents
### Delete Single Row
```
db.podcasts.deleteOne({
    _id: Objectid("6282c9862acb966e76bbf20a") 
})
```

### Delete Multuple Row
```
db.podcasts.deleteMany({category: "crime"})
```

----

# Aggration Pipeline
> This section contains key definitions for this lesson, as well as the code for an aggregation pipeline.
## Definitions
- Aggregation: Collection and summary of data

- Stage: One of the built-in methods that can be completed on the data, but does not permanently alter it

- Aggregation pipeline: A series of stages completed on the data in order

### Structure of an Aggregation Pipeline
```
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

## Aggration stage filters
### Use of ```$match``` and ```$group``` stage filter
> #### ```$match```
The ```$match``` stage filters for documents that match specified conditions.
#### ```$group```
> The ```$group``` stage groups documents by a group key.
```
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
```
### Use of ```$sort``` and ```$limit``` stage filters
#### ```$sort```
> The $sort stage sorts all input documents and returns them to the pipeline in sorted order. We use 1 to represent ascending order, and -1 to represent descending order.
#### ```$limit```
> The $limit  stage returns only a specified number of records.

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

### Using ```$project, $count, $set and $out``` Stages in a MongoDB Aggregation Pipeline

#### ```$project```
> The $project stage specifies the fields of the output documents. 1 means that the field should be included, and 0 means that the field should be supressed. The field can also be assigned a new value.
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
#### ```$set```
> The $set stage creates new fields or changes the value of existing fields, and then outputs the documents with the new fields.

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
#### ```$count```
> The $count stage creates a new document, with the number of documents at that stage in the aggregation pipeline assigned to the specified field name.

```
{
  $count: "total_zips"
}
```

#### ```$out```
> Takes the documents returned by the aggregation pipeline and writes them to a specified collection. You can specify the output database.

```
db.zips.aggregate([
{   
   $match: { 
      state: "AK"
    }
},
{
   $group: {
      _id: "$city",
      totalZips: { $count : { } }
   }
},
{
    $match: {
        totalZips:{$gt:1}
    }
},
{
    $out:"zips_count"
}
])
```
----

# Indexing - performence tuning
### Types of Indexes
1. Single field Index
2. munti field Index
3. compound Index

### Create single field Index
> Add ```unique:true``` as a second, optional, parameter in createIndex() to force uniqueness in the index field values.
```
db.customers.createIndex({
  email: 1
},
{
  unique:true
})
```

### View the Indexes used in a Collection
```
db.customers.getIndexes()
```

### Use ```.explain()``` to Check if an index is being used on a query
> This plan provides the details of the execution stages (IXSCAN , COLLSCAN, FETCH, SORT, etc.).

```
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

### Create a Single field Multikey Index
> Include an object as parameter that contains the array field and sort order. In this example accounts is an array field.

```
db.customers.createIndex({
  accounts: 1
})
```


### Create a Compound Index

> Within the parentheses of createIndex(), include an object that contains two or more fields and their sort order.

```
db.customers.createIndex({
  active:1, 
  birthdate:-1,
  name:1
})
```

### Order of Fields in a Compound Index
The order of the fields matters when creating the index and the sort order. It is recommended to list the fields in the following order: Equality, Sort, and Range.

- Equality: field/s that matches on a single field value in a query

- Sort: field/s that orders the results by in a query

- Range: field/s that the query filter in a range of valid values


### Delete a Index
#### Step 1 : Get the name of the index
```
db.customers.getIndexes()
```
#### Step 2 : Delete the index
```
db.customers.dropIndex(
  'active_1_birthdate_-1_name_1'
)
```

#### Delete index by key:

```
db.customers.dropIndex({
  active:1,
  birthdate:-1, 
  name:1
})
```

### Delete all Indexes
```
db.customers.dropIndexes()
```
### Delete more than One specified columns 
```
db.collection.dropIndexes([
  'index1name', 'index2name', 'index3name'
  ])

```
----
# Text Search

```
db.posts.find({
$text: {
$search: "hello"
}
})
```

Use the $meta query operator to obtain and sort by the relevance score of each matching document. For example, to order a list of coffee shops in order of relevance, run the following:

```
db.stores.find(
{ $text: { $search: "coffee shop cake" } },
{ score: { $meta: "textScore" } }
).sort( { score: { $meta: "textScore" } } )
```

# MongoDB Transaction

### Using a Transaction
```
const session = db.getMongo().startSession()

session.startTransaction()

const account = session.getDatabase('< add database name here>').getCollection('<add collection name here>')

//Add database operations like .updateOne() here

session.commitTransaction()
```

### Aborting a Transaction
```
const session = db.getMongo().startSession()

session.startTransaction()

const account = session.getDatabase('< add database name here>').getCollection('<add collection name here>')

//Add database operations like .updateOne() here

session.abortTransaction()
```





# MongoDB Commands Guide

**_All MongoDb commands you will ever need (MongoDb Cheatsheet)
In this post, we will see a comprehensive list of all the MongoDB commands. This list is not complete and is imposible to put the entire docs. This are the most used commands._**

#### If you have not completed the installation follow this [Installation](Installation.md) guide.

## 1. Database Commands

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

## 2. Collection Commands

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

## 3. Row(Document) Commands

### Show all Rows in a Collection

```
db.content.find()
```

### Find By Element in Array ($elemMatch)

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

### Text Search

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

### Search in a MongoDb Database

```
db.content.find({lang:'Python'})
```

### Find the first row matching the object

```
db.content.findOne({name: 'tirtha'})
```

### Limit the number of rows in output

```
db.content.find().limit(2)
```

### Count the number of rows in the output

```
db.content.find().count()
```

### Less than/Greater than/ Less than or Eq/Greater than or Eq

Know more about [this here.](https://www.mongodb.com/docs/manual/reference/operator/query/)

```
db.content.find({joined: {$lt: 5}})
db.content.find({joined: {$lte: 5}})
db.content.find({joined: {$gt: 5}})
db.content.find({joined: {$gte: 5}})
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

### Update all row without any condion.

```
db.content.updateMany({},
{$set: {'name': 'tirtha',
    'lang': 'JavaScript',
    'joined': 51
}})
```

### Upsert a data on update

If you want to insert the data if the search query is not found then ``upsert:true` will do that.

```
db.content.updateOne({name: 'ghontu'},
{$set: {'name': 'ghontu',
    'lang': 'TypeScript',
    'joined': 10
}}, {upsert: true})
```

### Mongodb Increment Operator

SQL : `update tablename set joined=joined+2 where name='montu'`

```
db.content.update({name: 'Rohan'},
{$inc:{
    joined: 2
}})
```

### Mongodb Rename Operator

Rename a field in a document

```
db.content.update({name: 'ghontu'},
{$rename:{
    joined: 'Experience'
}})
```

### Know more about these [Update operators](https://www.mongodb.com/docs/manual/reference/operator/update/).

### update on Less than/Greater than/ Less than or Eq/Greater than or Eq

Know more about [this here.](https://www.mongodb.com/docs/manual/reference/operator/query/)

```
db.content.update({joined: {$lt: 5}},{fresher:true});
db.content.find({joined: {$lte: 5}},{fresher:true})
db.content.find({joined: {$eq: 5}},{fresher:true})
db.content.find({joined: {$gt: 5}},{fresher:false})
db.content.find({joined: {$gte: 5}},{fresher:false})
```

### Delete Row

```
db.content.remove({name: 'tirtha'})
```
# MongoDB-Guide

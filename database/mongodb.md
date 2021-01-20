### Commands
- start server
	- mongod
- start mongo shell
	- mongo
- stop mongo shell
	- quit()
- Show All Databases
	- show dbs
- Show Current Database
	- db
- Create Or Switch Database
	- use mybrary
- Drop
	- db.dropDatabase()
- Create Collection
	- db.createCollection('posts')
- Show Collections
	- show collections
- Insert Row
```js
// posts
db.posts.insert({
  title: 'Post One',
  body: 'Body of post one',
  category: 'News',
  tags: ['news', 'events'],
  user: {
    name: 'John Doe',
    status: 'author'
  },
  date: Date()
})

// tours
db.tours.insertOne({name:"The Forest Hiker", price:297,rating:4.7})
```
- Insert Multiple Rows
```js
db.posts.insertMany([
  {
    title: 'Post Two',
    body: 'Body of post two',
    category: 'Technology',
    date: Date()
  },
  {
    title: 'Post Three',
    body: 'Body of post three',
    category: 'News',
    date: Date()
  },
  {
    title: 'Post Four',
    body: 'Body of post three',
    category: 'Entertainment',
    date: Date()
  }
])

// tours
db.tours.insertMany([{name:"The Sea Explorer", price:497, rating:4.8},{name:"The Snow Adventure",price:997,rating:4.9,difficulty:"easy"}])
```
- Get All Rows
	- db.posts.find()
- Get All Rows Formatted
	db.find().pretty()
- Find Rows
	- db.posts.find({ category: 'News' })
- Sort Rows (asc)
	- db.posts.find().sort({ title: 1 }).pretty()
- Sort Rows (desc)
	- db.posts.find().sort({ title: -1 }).pretty()
- Count Rows
	- db.posts.find().count()
	- db.posts.find({ category: 'News' }).count()
- Limit Rows
	- db.posts.find().limit(2).pretty()
- Chaining
	- db.posts.find().limit(2).sort({ title: 1 }).pretty()
- Foreach
```js
db.posts.find().forEach(function(doc) {
  print("Blog Post: " + doc.title)
})
```
- Find Row
	- db.posts.findOne({ category: 'News' })
	- db.tours.find({name:"The Forest Hiker"})
	- db.tours.find({ price: {$lte: 500} })
	- db.tours.find({ price: {$lt: 500}, rating: {$gte:4.8}  })
	- db.tours.find({ $or: [ {price: {$lt: 500}}, {rating: {$gte:4.8}} ]  })
- Find Specific Fields
```js
db.posts.find({ title: 'Post One' }, {
  title: 1,
  author: 1
})

db.tours.find({ $or: [ {price: {$lt: 500}}, {rating: {$gte:4.8}} ]  }, {name:1})
```
- Update Row
```js
db.posts.update({ title: 'Post Two' },
{
  title: 'Post Two',
  body: 'New body for post 2',
  date: Date()
},
{
  upsert: true
})

// tours
db.tours.updateOne(
	{ name:"The Forest Hiker"}, 
	{ $set: {price:597}}
)

db.tours.updateMany(
	{ price: {$gt: 200} }, 
	{ $set: {premium:true}}
)
```
- Update Specific Field
```js
db.posts.update({ title: 'Post Two' },
{
  $set: {
    body: 'Body for post 2',
    category: 'Technology'
  }
})
```
- Increment Field ($inc)
```js
db.posts.update({ title: 'Post Two' },
{
  $inc: {
    likes: 5
  }
})
```
- Rename Field
```js
db.posts.update({ title: 'Post Two' },
{
  $rename: {
    likes: 'views'
  }
})
```
- Delete Row
	- db.posts.remove({ title: 'Post Four' })
	- db.tours.deleteMany({ rating: {$lt: 4.8} })
	- db.tours.deleteMany({})
- Sub-Documents
```js
db.posts.update({ title: 'Post One' },
{
  $set: {
    comments: [
      {
        body: 'Comment One',
        user: 'Mary Williams',
        date: Date()
      },
      {
        body: 'Comment Two',
        user: 'Harry White',
        date: Date()
      }
    ]
  }
})
```
- Find By Element in Array ($elemMatch)
```js
db.posts.find({
  comments: {
     $elemMatch: {
       user: 'Mary Williams'
       }
    }
  }
)
```
- Add Index
	- db.posts.createIndex({ title: 'text' })
- Text Search
```js
db.posts.find({
  $text: {
    $search: "\"Post O\""
    }
})
```
- Greater & Less Than
	- db.posts.find({ views: { $gt: 2 } })
	- db.posts.find({ views: { $gte: 7 } })
	- db.posts.find({ views: { $lt: 7 } })
	- db.posts.find({ views: { $lte: 7 } })

### References
- [How to Install MongoDB on Windows 10](https://www.youtube.com/watch?v=FwMwO8pXfq0)
- [MongoDB Crash Course 2019](https://www.youtube.com/watch?v=-56x56UppqQ)

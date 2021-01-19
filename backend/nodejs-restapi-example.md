### setting envrionment
- install
	- npm i express mongoose
	- npm i dotenv nodemon --save-dev
- package.json
```js
{
	"scripts": {
		"devStart": "nodemon server.js"
	}
}
```
- run program
	- npm run devStart

### An example
- server.js
```js
require('dotenv').config()

const express = require('express')
const app = express()
const mongoose = require('mongoose')


// Connecting MongoDB
mongoose.connect(process.env.DATABASE_URL, { useNewUrlParser: true })
const db = mongoose.connection
db.on('error', (error) => console.error(error))
db.once('open', () => console.log('Connected to Database'))

app.use(express.json())
// express router
const subscribersRouter = require('./routes/subscribers')
app.use('/subscribers', subscribersRouter) // localhost:3000/subscribers

app.listen(3000, () => console.log('Server Started'))
```
- .env
```js
DATABASE_URL=mongodb://localhost/subscribers
```
- routes/subscribers.js
```js
const express = require('express')
const router = express.Router()
const Subscriber = require('../models/subscriber')

// Getting all
router.get('/', async (req, res) => {
	// res.send('Hello World')
  try {
    const subscribers = await Subscriber.find()
    res.json(subscribers)
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

// Getting One
router.get('/:id', getSubscriber, (req, res) => { 
	// res.send(req.params.id)
  res.json(res.subscriber)
})

// Creating one
router.post('/', async (req, res) => {
  const subscriber = new Subscriber({
    name: req.body.name,
    subscribedToChannel: req.body.subscribedToChannel
  })
  try {
    const newSubscriber = await subscriber.save()
    res.status(201).json(newSubscriber)
  } catch (err) {
    res.status(400).json({ message: err.message })
  }
})

// Updating One
router.patch('/:id', getSubscriber, async (req, res) => {
  if (req.body.name != null) {
    res.subscriber.name = req.body.name
  }
  if (req.body.subscribedToChannel != null) {
    res.subscriber.subscribedToChannel = req.body.subscribedToChannel
  }
  try {
    const updatedSubscriber = await res.subscriber.save()
    res.json(updatedSubscriber)
  } catch (err) {
    res.status(400).json({ message: err.message })
  }
})

// Deleting One
router.delete('/:id', getSubscriber, async (req, res) => {
  try {
    await res.subscriber.remove()
    res.json({ message: 'Deleted Subscriber' })
  } catch (err) {
    res.status(500).json({ message: err.message })
  }
})

async function getSubscriber(req, res, next) {
  let subscriber
  try {
    subscriber = await Subscriber.findById(req.params.id)
    if (subscriber == null) {
      return res.status(404).json({ message: 'Cannot find subscriber' })
    }
  } catch (err) {
    return res.status(500).json({ message: err.message })
  }

  res.subscriber = subscriber
  next()
}

module.exports = router
```

- models/subscriber.js
```js
const mongoose = require('mongoose')
// Mongoose Schema
const subscriberSchema = new mongoose.Schema({
  name: {
    type: String,
    required: true
  },
  subscribedToChannel: {
    type: String,
    required: true
  },
  subscribeDate: {
    type: Date,
    required: true,
    default: Date.now
  }
})
module.exports = mongoose.model('Subscriber', subscriberSchema)
```

### Testing RESTful API 
- Install "REST Client" for Visual Studio Code
- route.rest
```js
GET http://localhost:3000/subscribers
###
GET http://localhost:3000/subscribers/101
###
POST http://localhost:3000/subscribers
Content-Type: application/json
{
	"name":"Harold Kim",
	"subscribedToChannel":"Cheatsheet"
}
###
DELETE http://localhost:3000/subscribers/101
###
PATCH http://localhost:3000/subscribers/101
Content-Type: application/json
{
	"name":"New Name"
}
```
### References
- [Build A REST API With Node.js, Express, and MongoDB(https://www.youtube.com/watch?v=fgTGADljAeg)

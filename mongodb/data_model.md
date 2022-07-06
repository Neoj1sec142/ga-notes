# Mongoose Data Modeling

![](https://res.cloudinary.com/practicaldev/image/fetch/s--9uBLdn55--/c_imagga_scale,f_auto,fl_progressive,h_420,q_auto,w_1000/https://dev-to-uploads.s3.amazonaws.com/i/x534inlbfuxxhial9jnw.png)

## Overview

We'll be learning about modeling data with MongoDB and getting practice with creating Schemas and Models utilizing Mongoose ODM to interact with our MongoDB database.

## Getting started

- fork and clone
- `cd` into this folder
- read the following: [Model Design](https://docs.mongodb.com/manual/core/data-model-design)

## Common Data Model Design Practices

When it comes to building a database, one of the most important aspects is **how to model our data**. With `mongoDB` there are two ways we can accomplish this:

- Normalized
- Denormalized

Because `mongoDB` uses a structure similar to `JSON`, we can dictate the shape that our data takes.

## Denormalized Data Model

The term `denormalized` is used in mongoDB/mongoose to define or describe a model/document with nested data. What do you mean _nested_? Let's take a look at the following example:

```js
{
  _id: ObjectId("3e399709171f6188450e43d2"),
  name: "Joe Schmoe",
  email: "j.schmoe@gmail.com",
  address: {
    street: "123 Fake Street",
    city: "Faketon",
    state: "MA",
    zip: "12345"
  }
}
```

This looks like an object literal in JavaScript right? Well it is. In MongoDB it is common to **embed** related data inside a document. This structure (or schema) of embedding related data is a feature in MongoDB. In database design we call this type of schema "denormalized".

A HUGE (performance) benefit of having related data **embedded** in a document is that we can have all our data in one collection - that means we can create, read, update, and delete data all in one request!

> MongoDB Docs
>
> The embedded data model combines all related data in a single document instead of normalizing across multiple documents and collections. This data model facilitates atomic operations.

## Normalized Data Model

Normalized data takes a different approach. We link different data sets by utilizing a reference. A `reference` is a virtual link between two documents in seperate collections. This is done typically by using the `ObjectId/_id` field which is a unique identifier for each record.

Let's look at the following examples:

`publisher` document

```js
{
  _id: ObjectId("3e399709171f6188450e43d2"),
  name: "Penguin Books",
  location: "375 Hudson St, New York, NY 10014",
  url: "https://penguin.com"
}
```

`book` document

```js
{
  title: "A New Earth",
  author: "Eckhart Tolle",
  published_date: "2005",
  publisher_id: ObjectId("3e399709171f6188450e43d2")
}
```

Notice how we create the relationship between `publisher` and `book` via a `publisher_id` field.

- in the `book` document we are creating a **reference** to a `publisher` document

According to MongoDB docs the reason we would want to use a normalized data model would be:

- when embedding would result in **duplication** of data but would not provide sufficient read performance advantages to outweigh the implications of the duplication. (We will look at an example of this in a later lesson)
- to represent more complex many-to-many relationships
- to model large hierarchical data sets

Don't worry about the last two bullet points. A lot of this knowledge will come with maturity of working with MongoDB - the more you work with it, the more you will begin to understand how the relationships work, and what relationship best suits your app's needs.

> More on normalization and denormalization [here](https://techdifferences.com/difference-between-normalization-and-denormalization.html)

## Exercise

Let's build a normalized data model using referencing:

```sh
npm init -y
npm install mongoose
mkdir db models seed
touch db/index.js models/{publisher,book}.js seed/{publishers,books}.js
```

Start by setting up our database:

`/db/index.js`

```js
const mongoose = require('mongoose')

mongoose
  .connect('mongodb://127.0.0.1:27017/booksDatabase')
  .then(() => {
    console.log('Successfully connected to MongoDB.')
  })
  .catch((e) => {
    console.error('Connection error', e.message)
  })
mongoose.set('debug', true)
const db = mongoose.connection

module.exports = db
```

> Notice the following line `mongoose.set('debug', true)`, this is super useful for debugging our queries, it will show the raw query mongo executes right in the terminal

## Defining Schemas and Generating Models

Let's create the Publisher `Schema` first:

`/models/publisher.js`

```js
const { Schema } = require('mongoose')

const Publisher = new Schema(
  {
    name: { type: String, required: true },
    location: { type: String, required: true },
    url: { type: String, required: true }
  },
  { timestamps: true }
)

module.exports = Publisher
```

Now that we have the Publisher Schema, we can now create the Book `Schema` that **references** the Publisher Schema:

`/models/book.js`

```js
const { Schema } = require('mongoose')

const Book = new Schema(
  {
    title: { type: String, required: true },
    author: { type: String, required: true },
    published_date: { type: String, required: true },
    publisher_id: { type: Schema.Types.ObjectId, ref: 'publisher_id' }
  },
  { timestamps: true }
)

module.exports = Book
```

> Notice how we reference the Publisher Schema by `publisher_id`.

Now that we have both schemas created, we need to define them as models, which will allow our Schemas to interface with our MongoDB database.

Create an `index.js` in your `models` folder.
Add the following code:

```js
const mongoose = require('mongoose')
const BookSchema = require('./book')
const PublisherSchema = require('./publisher')

const Book = mongoose.model('Book', BookSchema)
const Publisher = mongoose.model('Publisher', PublisherSchema)

module.exports = {
  Book,
  Publisher
}
```

We've now created models that we can interact with in our project. When we require the models, we can now require the `models` directory and utitlize destructuring syntax to bring in any models we need like so:

```js
const { Publisher, Book } = require('./models')
```

Now we're ready to create our seed data.

## Seeding Our Database

![seed](https://www.sausd.us/cms/lib/CA01000471/Centricity/Domain/10075/beans%20growing%20animation.gif)

Once again since the Book model depends on the Publisher model, we will create the publisher seed data first:

`/seed/publishers.js`

```js
const db = require('../db')
const { Publisher } = require('../models')

db.on('error', console.error.bind(console, 'MongoDB connection error:'))

const main = async () => {
  const publishers = [
    {
      name: 'Penguin Books',
      location: '375 Hudson St, New York, NY 10014',
      url: 'https://penguin.com'
    },
    {
      name: 'HarperCollins',
      location: '10 E 53rd St, New York, NY 10022',
      url: 'https://www.harpercollins.com'
    }
  ]

  await Publisher.insertMany(publishers)
  console.log('Created publishers!')
}
const run = async () => {
  await main()
  db.close()
}

run()
```

`/seed/books.js`

```js
const db = require('../db')
const { Publisher, Book } = require('../models')

db.on('error', console.error.bind(console, 'MongoDB connection error:'))

const main = async () => {
  const penguinBooks = await Publisher.find({ name: 'Penguin Books' })
  const harperCollins = await Publisher.find({ name: 'HarperCollins' })

  const books = [
    {
      title: 'The Power of Now',
      author: 'Eckhart Tolle',
      published_date: '2005',
      publisher_id: penguinBooks[0]._id
    },
    {
      title: 'Zen and the Art of Motorcycle Maintenance',
      author: 'Robert M. Pirsig',
      published_date: '1999',
      publisher_id: harperCollins[0]._id
    },
    {
      title: 'The Alchemist',
      author: 'Paulo Coelho',
      published_date: '1988',
      publisher_id: harperCollins[0]._id
    }
  ]

  await Book.insertMany(books)
  console.log('Created books with publishers!')
}
const run = async () => {
  await main()
  db.close()
}

run()
```

## Writing Queries

Utilize the provided [resources](#resources) to complete the following exercise .

In the provided `query.js` file add the following:

```js
const db = require('./db')
const { Book } = require('./models')

async function main() {
  try {
  } catch (error) {
    console.log(error)
  } finally {
    await db.close()
  }
}

main()
```

Write 4 functions that perform the following `CRUD` operations:

1. Finds a book
2. Creates a book
3. Updates a book
4. Deletes a book

Make sure to invoke your functions in the `try` block in the `main` function at the bottom of the `query.js` file.

## Recap

In this lesson, we successfully created `schemas` and `models` to interact with the data in our database. We performed `CRUD` operations on the models and queried the data.

## Resources

- [Mongoose Queries](https://mongoosejs.com/docs/api/query.html)
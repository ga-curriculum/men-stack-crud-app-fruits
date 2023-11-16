# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Build the Fruit Model](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to define and export a mongoose model.

## Create the model file `fruit.js`

While this application may not have multiple models, it's still a good idea to create a directory called `models` that will store all Mongoose models used in the application. Let's go ahead and create the directory, along with a `fruit.js` file inside of it:

```bash
mkdir models
touch models/fruit.js
```

We will always have a single file per Mongoose model where we:

1. Define the schema
2. Compile the schema into a model
3. Export that model

We will always name models and model files singularly. For example, `fruit.js` instead of `fruits.js`. This is because a single file will always export just one model.

## Create the fruits schema

Before we're able to define our model, we must first import the mongoose library into our `fruit.js` file, like so:

```javascript
// models/fruit.js

const mongoose = require('mongoose')
```

With that out of the way, we can define the schema, which provides a consistent structure to our data. Remember, schemas are just regular javascript objects, where the key represents the name of the property we want our data to have, and the value is used to define the data type and any restrictions on the values stored in that property.

In this case, our fruit model will be pretty simple, with a `name` property of type `String` and an `isReadyToEat` property of type `Boolean`. Here's how we'll define that in our `fruit.js` file:

```javascript
// models/fruit.js

const fruitSchema = mongoose.Schema({
    name: String,
    isReadyToEat: Boolean
});
```

## Register the model

Once we've defined our schema, we can then tell mongoose to create a collection in mongodb and validate that collection's data using the schema. We do this using the `mongoose.model` method:

```javascript
// models/fruit.js

const Fruit = mongoose.model('Fruit', fruitSchema);
```

It is a standard convention to use a capital letter to start off database models, so be sure your model is named `Fruit` and not `fruit`.

## Export the model from the fruit.js file

Next, we want to export the `Fruit` model we just created, so that the rest of our application has access to it.

The `Fruit` model is what we will use to perform CRUD on the collection.

```javascript
// models/fruit.js

module.exports = Fruit;
```

## Import the model into the `server.js` file

We'll need that model in many of the routes we define in `server.js`, so let's add an import statement to `server.js`. Since we need our application to be connected to our MongoDB instance before connecting our model, this code should be placed directly after the mongoose connection code, like so:

```javascript
// server.js

mongoose.connect(process.env.MONGO_URI);

mongoose.connection.on('connected', () => {
  console.log(`Connected to MongoDB ${mongoose.connection.name}.`)
})

// NEW LINE OF CODE REQUIRING THE MODEL HERE
const Fruit = require('./models/fruit.js');
```

And that's it! Our model is set up and ready to be used in the request handling functions defined in our express routes.

[Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-the-fruit-model-start)

[Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-the-fruit-model-complete)
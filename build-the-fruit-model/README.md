# ![MEN Stack CRUD App - Fruits - Build the Fruit Model](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to define and export a mongoose model.

## Create the model file `fruit.js`

Let's create a directory that will hold the file for our fruit model (`fruits.js`):

```bash
mkdir models
touch models/fruit.js
```

It might seem silly to have a directory with just a single file in it, but it's a good habit to get into. Hypothetically, if we were to decide to add more models later on, it would be nice to have a separate file for each model. At that point, the directory would become more useful, from a structural standpoint.

The `fruit.js` file should:

1. Create the schema, which basically defines the properties of a fruit
2. Link the schema with a _model_, which is essentially a reference to the collection in MongoDB
3. Export that model, so your `server.js` file can use it.

You should name models and model files singularly. For example, `fruit.js` instead of `fruits.js`. This is because a single file should be exporting just one model.

## Create the fruits schema

Before we're able to define our model, we must first import the mongoose library into our `fruit.js` file, like so:

```javascript
// models/fruit.js

const mongoose = require("mongoose");
```

With that out of the way, we can define the schema, which provides a consistent structure to our data. Remember, schemas are just regular javascript objects, where the key represents the name of the property we want our data to have, and the value is used to define the data type and any restrictions on the values stored in that property.

In this case, our fruit model will be pretty simple, with a `name` property of type `String` and an `isReadyToEat` property of type `Boolean`. Here's how we'll define that in our `fruit.js` file:

```javascript
// models/fruit.js

const fruitSchema = mongoose.Schema({
  name: String,
  isReadyToEat: Boolean,
});
```

## Register the model

Once we've defined our schema, we can then tell mongoose to create a collection in MongoDB and validate that collection's data using the schema. We do this using the `mongoose.model` method:

```javascript
// models/fruit.js

const Fruit = mongoose.model("Fruit", fruitSchema);
```

There is a convention to use a capital letter to start off database models, so name your model `Fruit`, as opposed to `fruit`.

## Export the model from the fruit.js file

Next, we want to export the `Fruit` model we just created, so that the rest of our application has access to it.

The `Fruit` model is what we will use to perform CRUD on the collection.

```javascript
// models/fruit.js

module.exports = Fruit;
```

## Import the model into the `server.js` file

We'll need that model in many of the routes we define in `server.js`, so let's add a require statement to `server.js`. Since we need our application to be connected to our MongoDB instance before connecting our model, this code should be placed directly after the mongoose connection code, like so:

```javascript
// server.js

mongoose.connect(process.env.MONGO_URI);

mongoose.connection.on("connected", () => {
  console.log(`Connected to MongoDB ${mongoose.connection.name}.`);
});

// NEW LINE OF CODE REQUIRING THE MODEL HERE
const Fruit = require("./models/fruit.js");
```

And that's it! Our model is set up and ready to be used in the request handling functions defined in our express routes.

[Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-the-fruit-model-start)

[Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-the-fruit-model-complete)

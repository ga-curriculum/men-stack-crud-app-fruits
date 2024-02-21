# ![MEN Stack CRUD App - Fruits - Use Mongoose to Connect to MongoDB](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to connect to a MongoDB cloud database using the Mongoose JavaScript driver.

## Install `mongoose` and `dotenv` from NPM

To use mongoose in our application, we'll first need to install the `mongoose` and `dotenv` packages from NPM:

```bash
npm i mongoose dotenv
```

## Add the MongoDB Atlas connection string to a `.env` file

First, create a `.env` file in your project's root directory:

```bash
touch .env
```

This file will be used to store any sensitive, secret information that application needs to run, but that we don't want to commit to GitHub. Database connection strings definitely qualify: we don't want just anybody to be able to walk into our database using this string! To ensure that our `.env` file doesn't make its way up to GitHub, create a `.gitignore` file:

```bash
touch .gitignore
```

In that file, add the following:

```plain
.env
node_modules
```

The first line above will make sure that the `.env` file isn't tracked by Git. The second line does the same thing, but for the `node_modules` directory that contains all the dependencies that `npm` has already installed or will install in the future. Ignoring `node_modules` is not about security, though. Rather, it reduces the amount of code in our repo by eliminating third party Node packages that can easily be installed via `npm`. Ignoring `node_modules` doesn't have to be done at the same time as ignoring `.env`, but since we're already in this file, we might as well do it now.

Now that we're certain Git won't track it, let's edit our `.env` file.

`.env` files are a simple list of key-value pairs defined like so:

```plain
SECRET_NUMBER=13
PASSWORD=12345
```

This will allow our application to access the `SECRET_NUMBER`, and `PASSWORD` properties on a `process.env` object.

You can now paste your MongoDB Atlas connection string into the app's `.env` file, assigning it to a `MONGODB_URI` environment variable. For example:

```plain
MONGODB_URI=mongodb+srv://<username>:<password>@sei-w0kys.azure.mongodb.net/?retryWrites=true
```

Do not use the above connection string in your application, it will not work.

It is important that there are no spaces between `MONGODB_URI`, `=`, and your atlas connection string. It should be written as one continuous string with no spaces.

This will make the connection string available in our application on the `process.env.MONGODB_URI` property.

Your connection string will default to a generic unnamed database as indicated by `/?` towards the end of the connection string. However, you **_must_** update this to your preferred database name. In this application, that will be `fruits`. You can specify the preferred database name by adding it between the `/` and the `?` in the connection string. Here's how that will look for this app: `/fruits?`. The full connection string for this app will look something like this:

```plain
MONGODB_URI=mongodb+srv://<username>:<password>@sei-w0kys.azure.mongodb.net/fruits?retryWrites=true
```

Again, do not use the above connection string in your application, it will not work. Simply note where `/fruits?` exists in the value for `MONGODB_URI` and replicate it in your own version.

Anytime you need to make a new app, you can use this same connection string and only replace the `/fruits?` portion of the string. Ensure the name you assign is unique to that project - for example, once we've used the name `fruits` for this app, you shouldn't use it again. Also ensure it doesn't contain any special characters.

## Connecting to MongoDB Atlas in the app

With that done, we need to require the `dotenv` package in our `server.js` file, and use it to read the environment variables from this file.

At the **_top_** of your `server.js` file, add:

```js
//server.js

const dotenv = require("dotenv");
dotenv.config();
```

It's important that these two lines of code are at the top of this file - it ensures that the environment variables are available everywhere across your application.

Next, we'll need to require `mongoose` in our application so that we can use it to connect to our database:

```javascript
// server.js

const dotenv = require("dotenv");
dotenv.config();
const express = require("express");
/* NEW LINE OF CODE REQUIRING MONGOOSE BELOW */
const mongoose = require("mongoose");
```

Now we can use the `mongoose.connect()` method to connect to our database. We'll pass this method a single argument - the connection string from our `.env` file. This is the environment variable we just made in our `.env` file - `MONGODB_URI`. Add a new line that accomplishes this after the require statements:

```javascript
// server.js

const dotenv = require("dotenv");
dotenv.config();
const express = require("express");
const mongoose = require("mongoose");

const app = express();

/* NEW LINE OF CODE CONNECTING TO MONGODB BELOW */
mongoose.connect(process.env.MONGODB_URI);
```

To ease debugging, we'll also add a connection message that will print to our terminal when we've connected to the database. Add it below the `mongoose.connect()` method:

```javascript
// server.js

/* NEW CODE TO LISTEN FOR THE MONGOOSE 'connected' EVENT */
mongoose.connection.on("connected", () => {
  console.log(`Connected to MongoDB ${mongoose.connection.name}.`);
});
```

This is a Mongoose event listener that runs the supplied callback function once we have connected to a database. In the callback function, we log the name of the database to which we've connected - in this case, it should be `fruits`.

## Start the app

Launch the application with `nodemon`. You should see two messages in your terminal - one confirming that the app is ready, and a second confirming that you are connected to your database.

> ♻️ Repeatable pattern: Any app you build that connects to a MongoDB database in Node using Mongoose will need a `.env` file with a connection string inside of it, and a `mongoose.connect()` method in your `server.js` file. You will pass this method the environment variable you created in your `.env` file - in this case `MONGODB_URI`.

tktk Notes: Get the app connected to MongoDB with Mongoose. npm i mongoose dotenv

[tktk Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/use-mongoose-to-connect-to-mongodb-start)

[tktk Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/use-mongoose-to-connect-to-mongodb-complete)

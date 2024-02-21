# ![MEN Stack CRUD App - Fruits - Build and Run server.js](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

tktk Notes: Get an app to an error free start with minimal config. npm init and npm i express.

[tktk Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-and-run-serverjs-start)

[tktk Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-and-run-serverjs-complete)

# ![[Introduction to Express] - Build and Run 'server.js'](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to build the most basic structure of an **Express** server and know how to start it.

### Install the Express Module

Before we begin building our server, let's use `npm` to install the Express module in this project:

```bash
npm i express
```

> 📚 Note that `i` is a shortcut for `install`.

Now that we've done this, the Express framework is available for us to use when building our application. Without this step, if we were to try to run our server code, we would receive an error since our app would be be trying to import a node module that we haven't installed yet.

Now that `express` is installed, let's write the basic code needed to create our server.

## Basic Structure of Express App

Here is a helpful outline of what a typical Express app does - let’s put this guide right in our `server.js`:

```js
// Here is where we import modules
// We begin by loading Express
const express = require("express");

const app = express();

app.listen(3000, () => {
  console.log("Listening on port 3000");
});
```

> 📚 The `server.js` file is typically the main entry point and configuration file for setting up an Express web server.

## Run the Server

Let's test our work by running our server. Run the following in your terminal:

```bash
nodemon
```

Our server should be up and running and you should see something like this:

![Server Running](./assets/server-running.png)

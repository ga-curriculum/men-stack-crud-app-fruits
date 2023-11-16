# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Build a Landing Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to build an Express route that renders a landing page for an application.

## The Landing Page

No matter what an application does, nearly every web application has a default landing "home" page for the app. This is the site that users will first visit when typing "webapp.com" into their browser, and will provide navigation to the other parts of the app. 

## Install `ejs` from NPM

We'll need the `ejs` package from NPM so that we're able to deliver views, let's add it to our project:

```bash
npm i ejs
```

We don't need to require it in our `server.js` file becuse express knows how to find this package out of the box.

## Build the route

We'll need to start with a route in our `server.js` file. We'll set it up to send a simple response for testing, then come back and link up our EJS file later. This code, as well as all future routes, should be above the `app.listen()` method.

```javascript
// server.js

// GET /
app.get('/', async (req, res) => {
  res.send('hello, friend!');
});
```

Test the route at this point by browsing to `localhost:3000`. You should receive a message in the browser that says hello, friend!

We've confirmed the route is valid at this point, let's do more than just send a message back.

## Create a `views` directory and the landing page

We're going to need a `views` directory to place all of our templates in, so let's create it now.

```bash
mkdir views
```

Our landing page will be an `index.ejs` file that is inside of this directory. Let's create that too:

```bash
touch views/index.ejs
```

Open that file, and add HTML boilerplate, along with some content inside of the `<body>` tag. Don't forget to also change the title!

```html
<!-- views/index.ejs -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Fruits App</title>
</head>
<body>
  <h1>Welcome to the Fruits app!</h1>
  <p>An app for collecting your favorite fruits.</p>
</body>
</html>
```

With that built, all we need to do is send it in response to a request on the `/` route of our application. Change the existing `res.send()` method to a `res.render()` of this file we just created:

```javascript
// server.js

// GET /
app.get('/', async (req, res) => {
  res.render('index.ejs');
});
```

Browse to `localhost:3000` and see the result!



[Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/landing-page-start)

[Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/landing-page-complete)

# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Build the New Fruit Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to build an express route that renders a form that will allow users to enter data that will be sent to the server for processing.

## The NEW Route

The purpose of this route is to provide users with a form they can use to submit data to our server for eventual entry into the database. In keeping with RESTful routes conventions, the url for this route will be: `/fruits/new`.

We won't need to use the Fruit model in here. No data is actually be used in this route. All this route does is provide the form to the user. If we want to create data through our application, this is where we need to start!

## Create the route

Let's start by defining the route and testing it as soon as possible. Add the following code to `server.js`, beneath the landing page route we've already created and above the `app.listen()` method (remember, all routes should be defined above `app.listen()`).

```javascript
// server.js

// GET /fruits/new
app.get('/fruits/new', (req, res)=>{
  res.send('This route sends the user a form page!');
})
```

Test it by visiting `localhost:3000/fruits/new` in your browser's address bar.

## Create the new template

Now that the route is set up, let's create the template we want this route to render for the user. While we're at it, let's also put all the templates for our fruits collection inside a sub-folder within the `views` directory we already created:

```bash
mkdir views/fruits
touch views/fruits/new.ejs
```

While this application will only have one model, you can imagine that if an app had several different models, we'd want to have a `new.ejs` template for every single model. This is why we create a sub-folder for the views associated with each different model.

Inside this template file, add the HTML boilerplate and an `<h1>` to get started:

```html
<!-- views/fruits/new.ejs -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Create a Fruit</title>
</head>
<body>
  <h1>Create a New Fruit!</h1>
</body>
</html>
```

Our goal throughout this project will be to test pieces of this application as soon as possible, so while it's tempting to go right on ahead and create the form users will need, let's update the route first so we can test it. 

## Updating the new route

Instead of `res.send`, we'll want to render the `new.ejs` template found in the `views/fruit` directory. Here's the full route to get that done:

```javascript
// server.js

// GET /fruits/new
app.get('/fruits/new', (req, res)=>{
  res.render('/fruits/new.ejs');
})
```

We should be able to test it by again visiting `localhost:3000/fruits/new` in our browser, and this time we should see the h1 message on the page.

## Creating the form

Finally, let's add the actual form element to our template page, with inputs and labels for both of the properties of the Fruit model. While the `name` field can easily be a text input, the boolean `isReadyToEat` field will be slightly more complicated, relying on a checkbox element.

```html
<!-- views/fruits/new.ejs -->

<body>
  <h1>Create a New Fruit!</h1>
  <form>
    <label> 
      Name:
      <input type="text" name="name">
    </label>
    <label>
      Ready to Eat?
      <input type="checkbox" name="isReadyToEat">
    </label>
    <button type="submit">Add fruit</button>
  </form>
</body>
```

This takes care of the user interface for creating a new fruit! Sadly, it isn't testable yet, since we don't have the route responsible for handling the HTTP request that this form would generate, let's handle the submission of this form next!

[Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-the-new-fruit-form-start)

[Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-the-new-fruit-form-complete)


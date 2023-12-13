# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Build the Edit Fruit Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to edit a fruit from their rendered form. 

## Step 1: Conceptualize the EDIT route
The `EDIT` route allows us to modify details of a certain fruit in our database. If we examine the RESTful routing guidelines, we'll notice that the corresponding URL for this route is `PUT /fruits/:fruitId`, where `fruitId` represents the unique identifier of the fruit we intend to edit.

## Step 2: Create the UI that will issue the request to that route
Now we can create the UI. We'll add a button next to the name of the fruit on the show page. Make sure to wrap it in an `<a>`. Add the following code to our `views/fruits/show.ejs` page:

```html
<body>
  <h1>Edit the <%= fruit.name %></h1>
  <form action="/fruits/<%=fruit._id%>?_method=PUT" method="POST">
    <label for="name">Name:</label>
    <input type="text" name="name" value="<%= fruit.name %>" id="name">
    <label for="ready-to-eat">Ready to Eat?</label>
    <input
      type="checkbox"
      name="isReadyToEat"
      id="ready-to-eat"
      <% if (fruit.isReadyToEat) { %>
        checked 
      <% } %>
    >
    <button type="submit">Update fruit</button>
  </form>
  <a href="/fruits">Back to Fruits Index</a>
</body>
```

## Step 3: Define the route on the server and map it to an exported controller function
Here's the route for editing a fruit:
```js
// localhost:3000/fruits/:fruitId/edit
router.get("/:fruitId/edit", fruitsCtrl.edit)
```

Let's go ahead and stub up the controller function:
```js
function edit(req, res) {
  console.log("Gonna update fruits!")
}

export {
  index,
  newFruit as new,
  show,
  deleteFruit as delete,
  edit
}
```

## Step 4: Code the controller function
This sends us over to the controller function, where we'll look up the fruit by its `_id` and then render a view containing a form to update it:

```js
function edit(req, res) {
  Fruit.findById(req.params.fruitId)
  .then(fruit => {
    res.render("fruits/edit", {
      fruit, // same as: fruit: fruit
      title: "Edit Fruit"
    })
  })
  .catch(err => {
    console.log(err)
    res.redirect("/")
  })
}
```

## Step 5: Write the view if it doesn't exist
Start by creating the file:
```bash
touch views/fruits/edit.ejs
```

Then let's copy over the code from our `new.ejs` with some modifications. We'll want to set the values for the fields using the fruit data returned from our mongoose query:

```html
<%- include('../partials/html-head') %>
<link rel="stylesheet" href="/stylesheets/fruits/edit.css" />
<%- include('../partials/nav') %>

<main>
  <h1>>%= title %></h1>
    <form action="/fruits" method="POST">
      <label>
        Name: <%= fruit.name %>
        <input type="text" name="name">
      </label>
      <label>
        Ready to Eat?
        <input type="checkbox" name="isReadyToEat">
      </label>
      <button type="submit">Save fruit</button>
    </form>
    <a href="/fruits">Back to fruits index</a>
```
We'll handle submitting the form in the `Update` section.

[tktk Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-the-edit-fruit-page-start)

[tktk Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-the-edit-fruit-page-complete)


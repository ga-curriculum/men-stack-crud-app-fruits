# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Delete a Fruit](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to delete a route from their rendered form and see the updated list of fruits on their MongoDB Atlas dashboard. 

## Step 1: Conceptualizing the DELETE route
The purpose of this route is to remove a specific fruit from the database. If we refer to the RESTful routing conventions [tktk idk if we are going to have access to a chart? I'd rather say "if we look at the RESTful routing chart..."], we know our route will be  `DELETE /fruits/:fruitId`, with `fruitId` refering to the fruit we intend to delete. 

## Middleware

To further enhance our application, we'll introduce two essential middleware components: `method-override` and `morgan`.

The `method-override` middleware enables our application to handle HTTP methods that are not directly supported by the browser, such as `PUT` and `DELETE`. This is particularly useful for form submissions, as browsers often default to `POST` for form data

The `morgan` middleware serves as a logging tool for our HTTP requests, providing valuable insights into application behavior.

In order to use both of these, we need to install their node packages:

```bash
npm i method-override morgan
```

Next, let's import them at the top of our `server.js` file and mount `method-override`:
```js
import logger from "morgan"
import methodOverride from "method-override"
.
.
.
// Mount it along with our other middleware, ABOVE the routes
app.use(methodOverride('_method'))

app.use("/", indexRouter)
```

## Step 2: Create the UI that will issue the request to the route
Now we need to adjust our fruits table to include a column for deleting a fruit:

Adjust the code in our `views/fruits/index.ejs` page to match the following:

```html
<table id="list">
    <thead>
      <tr>
        <th>Fruit</th>
        <th></th>
        <th></th>
      </tr>
    </thead>
    <tbody>
      <% fruits.forEach((fruit) => { %>
      <tr>
        <td><%= fruit.name %></td>
        <td><a href="/fruits/<%= fruit._id %>">DETAILS</a></td>
        <td>
          <form 
            action="/fruits/<%= fruit._id %>?_method=DELETE" 
            method="POST"
          >
            <button type="submit">X</button>
          </form>
        </td>
      </tr>
      <% }) %>
    </tbody>
  </table>
```
tktk idk what the table is supposed to look like because it hasn't been created as far as i can tell, so this will need to be looked at. 


## Step 3: Define the route on the server and map it to an exported controller function
Now that we can override the POST method on our form and make it a `DELETE`, we can write the route for deleting a fruit in our `routes/fruits.js`:
```js
// localhost:3000/fruits/:fruitId
router.delete("/:fruitId", fruitsCtrl.delete)
```

And tub up our controller `controllers/fruits.js`:
```js
function deleteFruit(req, res) {
  console.log("Deleting fruits!")
}

export {
  index,
  newFruit as new,
  create
  show,
  deleteFruit as delete,
}
```

## Step 4: Code our controller function
Now we can write out the controller function that we stubbed up above:
```js
function deleteFruit(req, res) {
  Fruit.findByIdAndDelete(req.params.fruitId)
  .then(fruit => {
    res.redirect("/fruits")
  })
  .catch(err => {
    console.log(err)
    res.redirect("/fruits")
  })
}
```

## Step 5: Write the view if it doesn't exist
We don't need a view! We're just redirecting back to a view that we've already written!

Let's test it out!

[tktk Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/delete-a-fruit-start)

[tktk Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/delete-a-fruit-complete)


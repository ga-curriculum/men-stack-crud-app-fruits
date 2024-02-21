# ![MEN Stack CRUD App - Fruits - Delete a Fruit](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to delete a fruit by going to its show page and clicking a delete button. They should then be redirected back to the index page to see that the fruit was indeed deleted.

## Step 1: Conceptualizing the DELETE route

The purpose of this route is to remove a specific fruit from the database. If we refer to the RESTful routing conventions, we know our route will be `DELETE /fruits/:fruitId`, with `fruitId` referring to the id of the fruit we intend to delete.

## Middleware

To further enhance our application, we'll introduce two essential middleware components: `method-override` and `morgan`. We'll tackle `method-override` here and `morgan` at the end.

By default, web browsers only allow for the `method` attribute of a form to be set to either `POST` or `GET`. This is in direct conflict with our RESTful routing convention, which uses the HTTP methods `PUT` and `DELETE` for certain routes. The solution is to use the `method-override` middleware, which essentially tricks our express app into thinking that we've made `PUT` and `DELETE` requests. By doing this, we're able to stick to our routing conventions, while at the same time, behind the scenes, using HTTP methods that the browser supports. More details below.

The `morgan` middleware serves as a logging tool for our HTTP requests, providing valuable insights into application behavior. Again, we'll see it in action below.

In order to use both of these, we need to install their node packages:

```bash
npm i method-override morgan
```

Next, let's require/mount them at the top of our `server.js` file:

```js
const methodOverride = require("method-override");
const morgan = require("morgan");

// additional code here

// Mount it along with our other middleware, ABOVE the routes
app.use(methodOverride("_method"));
app.use(morgan("dev"));

// routes begin here
```

## Step 2: Create the UI that will issue the request to the route

Now we need to adjust our fruits show page to include a button for deleting a fruit:

Adjust the code in our `views/fruits/show.ejs` file to match the following:

```html
<h1>Here Are the Details about <%= fruit.name %></h1>
<% if(fruit.isReadyToEat){ %>
<p>This fruit is ready to eat!</p>
<% } else { %>
<p>This fruit is not ready to eat!</p>
<% } %>
<form action="/fruits/<%=fruit._id%>?_method=DELETE" method="POST">
  <button type="submit">Delete <%= fruit.name %></button>
</form>
<a href="/fruits">Back to fruits index</a>
```

All we've really added here is a `<form>` with a single button in it. It might initially seem odd to have a form with no actual inputs in it, but that's fine. Really, we just need a way to make a `DELETE` request, and links can't do that. In reality, forms can't either, but now that we've incorporated `method-override` into `server.js`, we can at least fake it using a `<form>`. We'll see how to do that in the next two notes:

1. Notice that the `action` attribute is set up with `<%=fruit._id%>` the same way that we created the links to the fruits' `show` pages in the `index` route (ignore `?_method=DELETE` for now). If we review our RESTful routes conventions, we'll notice that the `DELETE` request has the URL `/fruits/:fruitId`, just like the show route. We need to generate that URL, just like we did for the `show` page links, and then place it somewhere. The `<form>`'s `action` attribute sets up the URL of the request the same way the `<a>`'s `href` attribute does. Let's place it there.
1. Notice that the `method` attribute of our form is `POST`, not `DELETE`. This goes back to what I was saying before: the only values browsers accept for the `method` attribute on the `<form>` tag are either `GET` or `POST`. Having required/mounted the `method-override` middleware, we need to set `method` to `POST` (this is just a requirement of `method-override`). Having done that, as long as we have also included the `?_method=DELETE` query parameter (TKTK have we covered this previously?) at the end of our `action` value, express will think we've made a `DELETE` request.

In summary, the `DELETE` route is very similar to the `show` route, except its method is `DELETE`. To fake a request that has a `DELETE` method we need to:

1. Set up `method-override`
2. Use a `<form>` with a `method` set to `POST`
3. Append `?_method=DELETE` to the `action` attribute of the `<form>`

The final note here is that the form only has a single button in it. No other inputs exist. This is fine because, as we'll see in the next step, the only information we need to delete a fruit is its `_id`. Since this will be part of the URL of the request (review the `<form>`'s `action` attribute), we don't need any further inputs in the form.

## Step 3: Define the route on the server

Now that we can override the POST method on our form and make it a `DELETE`, we can write the route for deleting a fruit in our `server.js` file:

```js
app.delete("/fruits/:fruitId", async (req, res) => {
  await Fruit.findByIdAndDelete(req.params.fruitId);
  res.redirect("/fruits");
});
```

This is fairly similar to what we've done before. Some things to note:

- `app.delete` looks for a `DELETE` route. Now that we've properly implemented `method-override` and set up our `<form>` tag correctly in the `show` page, this should work as expected.
- The database work generated by `Fruit.findByIdAndDelete` takes a relatively long time, so we need to `await` it. In order to do this, we need to mark the route's callback function as `async`.
- `findByIdAndDelete` only needs the id of the fruit to determine which one to delete. This comes to us through the URL of the route via `req.params.fruitId`.
- We redirect back to the `index` route. We have the freedom to do whatever we want with this. We could render an EJS file if we wanted to, but the UX is simplest if we just redirect back to the `index`.

Test it out by going to the `show` page for any fruit in our app and clicking on the delete button. You should be taken back to the `index` page where you'll notice the deleted fruit is no longer listed.

## View `morgan` logs

If you look at your terminal, you may notice additional information regarding the various requests that have been made to Express has been logged. This is what the `morgan` middleware that we initially installed does for us. It's a nice way to have some debug information automatically logged for us without us manually setting it up. If at any point, you feel your terminal is getting crowded with additional information you don't need, simply comment out `app.use(morgan('dev'));`

[tktk Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/delete-a-fruit-start)

[tktk Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/delete-a-fruit-complete)

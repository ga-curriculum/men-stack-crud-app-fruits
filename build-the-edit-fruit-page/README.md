# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Build the Edit Fruit Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to create an edit page with a form pre-populated with the relevant data of the fruit being edited.

## Step 1: Conceptualize the EDIT route

The `Edit` route presents the user with a form for editing the details of a single fruit in our database. If we examine the RESTful routing guidelines, we'll notice that the corresponding URL for this route is `GET /fruits/:fruitId/edit`, where `:fruitId` represents the unique identifier of the fruit we intend to edit.  One thing worth noting is that this is the only RESTful route that contains three segments.  The final segment, (`/edit`) is what distinguishes this route from the `show` route as well as any other route.

Also, compare your new/create and edit/update RESTful routes conventions.  The full process of updating a fruit is similar to the process of creating a fruit in that both processes require two routes: a form route for collecting user data, and a second route for processing the data.  For updating a fruit, the form route is called Edit and the processing route is called Update.  Here's how the two processes line up:

| Process | Form Route | Processing Route |
| ---- | ---- | ---- |
| Create a fruit| New (`GET /fruits/new`) | Create (`POST /fruits`) |
| Update a fruit | Edit (`GET /fruits/:fruitId/edit`) | Update (`PUT /fruits/:fruitId`) |

We'll just be dealing with the `Edit` route for this section, but it's good to keep the above table in mind, especially as we begin creating the EJS file for this route.

## Step 2: Create the UI that will issue the request to that route

Add a link just after the delete form and before the "Back to fruits index" link on the show page. Adjust the following code to our `views/fruits/show.ejs` page:

```html
<form action="/fruits/<%=fruit._id%>?_method=DELETE" method="POST">
	<button type="submit">
		Delete <%= fruit.name %>
	</button>
</form>
<!-- add edit page link here -->
<a href="/fruits/<%= fruit._id %>/edit">Edit <%= fruit.name %></a>
<a href="/fruits">Back to fruits index</a>
```

Nothing really of note here.  Just remember that we're dynamically inserting the fruit's `_id` property into the `href` attribute of the `<a>` tag by using `<%= fruit._id %>`
## Step 3: Define the route in `server.js`

Keep this route grouped together with your other routes.  You can place this route anywhere, as long as it comes after any middleware and before `app.listen`.   Since it is the only route with three segments in its path (the others have at most two segments, e.g. `/fruits/:id`, `/fruits` etc), it can't conflict with any of the other routes in the same way that the `new` route could conflict with the `show` route.  Here's the route for editing a fruit:

```js
// GET localhost:3000/fruits/:fruitId/edit
app.get('/fruits/:fruitId/edit', async (req, res) => {
	const foundFruit = await Fruit.findById(req.params.fruitId);
	console.log(foundFruit);
	res.send(`This is the edit route for ${foundFruit.name}`);
});
```

Nothing terribly new here.  Note the following:

- `Fruit.findById` takes a while in computer time, so we need to `await` it.  To make this work, we need to mark the route's callback function as `async`.
- Once, we've found the fruit, using `req.params.fruitId`, we log it and `res.send` a helpful message to the browser

Test the route by clicking the "Edit" link on the show page of particular fruit.  Check your terminal to make sure you see the data for the fruit that you intend to edit.

## Step 5: Write the view

Let's modify our route so that it renders `views/fruits/edit.ejs`, passing in the `foundFruit` variable we created above:

```javascript
app.get('/fruits/:fruitId/edit', async (req, res) => {
	const foundFruit = await Fruit.findById(req.params.fruitId);
	res.render('fruits/edit.ejs', {
		fruit: foundFruit,
	});
});
```

Just remember that the second argument to `res.render` is an object containing the data we want to be available in our EJS file.  The key of each property of the object becomes the variable name in the EJS file, so we'll have a variable called `fruit` available to us in `views/fruits/edit.ejs`.  The value of `fruit` will be `foundFruit`, which is the data that comes back to us when we search for the fruit with the `_id` that matches `req.params.fruitId`.

Next, create the view file:

```bash
touch views/fruits/edit.ejs
```

Then let's copy over the code from `views/fruits/new.ejs` into `views/fruits/edit.ejs`.

```html
<body>
  <h1>Create a New Fruit!</h1>
  <form action="/fruits" method="POST">
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
  <a href="/fruits">Back to fruits index</a>
</body>
```

Refresh the edit page to see the changes.

Next, let's update the `<h1>` so it's more accurate:

```html
<h1>Edit the <%= fruit.name %></h1>
```

Remember that `fruit` is the data being injected into the EJS file from the Edit route in `server.js`.

Refresh again and check that the name is being displayed properly.  Now let's edit the `<form>` tag:

```html
<form action="/fruits/<%=fruit._id%>?_method=PUT" method="POST">
```

Go back and review the table at the beginning of this lesson.  The Edit form needs to make a `PUT` request to `/fruits/:fruitID` in the same way that the New form needs to make a `POST` request to `/fruits`.  Remember that browsers don't support `PUT` requests, but luckily we already installed `method-override` when we did Delete, so we can fake it just like we did back then.  This `<form>` tag is very similar to our Delete form, except that we're using `?_method=PUT` instead of `?_method=DELETE`.

The rest of the form is currently the same as the New form:

```html
<label>
	Name:
	<input type="text" name="name">
</label>
<label>
	Ready to Eat?
	<input type="checkbox" name="isReadyToEat">
</label>
<button type="submit">Add fruit</button>
```

Technically, we could leave it like this, but I think it would be a nice User Experience if the form came pre-populated with the data of the fruit currently being edited.  For the Name input, we can use the `<input>` tag's `value` attribute like so to populate it:

```html
<input type="text" name="name" value="<%= fruit.name %>">
```

Refresh, and the name of the fruit being edited should show up in the appropriate input field.

The checkbox is a little more difficult, but it's a good lesson.  Each type of input will require its own particular way of being pre-populated.  Rather than memorize a way to pre-populate each input type, instead focus on using basic JS logic along with a little bit of research into the various attributes that each input type can have.  Input types are always being added, and you never know which ones you'll use in advance.  Don't memorize: think!

When it comes to a `checkbox` input, a little research will reveal that it can have the attribute `checked` set on it without any value.  This will make the checkbox checked when the page loads.

Unchecked:

```html
<input type="checkbox" name="isReadyToEat">
```

Checked:

```html
<input type="checkbox" name="isReadyToEat" checked>
```

Update your checkbox code by adding the `checked` attribute and refresh.  The checkbox should be checked when the page loads.

We're almost there, but we want the checkbox to be checked only when `fruit.isReadyToEat` is true.  Let's reformat our HTML in preparation:

```html
<input
	type="checkbox"
	name="isReadyToEat"
	checked
>
```

It might seem odd to have each attribute on its own line, but this is a perfectly good way to make your code more readable as more and more attributes are added to your tags.

Now let's add an `if` statement that will conditionally render the `checked` attribute if `fruit.isReadyToEat` is true:

```html
<input
	type="checkbox"
	name="isReadyToEat"
	<% if (fruit.isReadyToEat) { %>
		checked 
	<% } %>
>
```

That's it!  Test your Edit route on fruits that are both "ready to eat" and "not ready to eat".  Create some extra fruit if necessary so you have both kinds.

Lastly, let's change the submit button so it has some more accurate text:

```html
<button type="submit">Update fruit</button>
```

You can of course test that your form submits properly, but you should get page that says `Cannot PUT /fruits/<fruitID>`.  We'll fix that in the next section.

[tktk Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-the-edit-fruit-page-start)

[tktk Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-the-edit-fruit-page-complete)


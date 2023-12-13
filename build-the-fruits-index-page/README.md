# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Build the Fruits Index Page](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to build an express route that will find and render all of the fruits that have been created in the database.

## Use a Model to Read Data

The querying ability of Mongoose is very capable. For example, we might use the following query to find movies with very specific parameters from a database:

```js
const movies = await Movie.find({ mpaaRating: 'PG' })
  .where('releaseYear').lt(1970)
  .where('cast').in('Bob Hope')
  .sort('-title')
  .limit(3)
  .select('title releaseYear');
```

The above query builder syntax is unique to Mongoose and is not available in MongoDB.

Powerful? Yes, but we're going to start with the basics!

## Useful Model methods to query for data

Here are the common Model methods for querying data:

| Method | Purpose | Syntax |
|---|---|---|
| `find` | Returns an array of all documents matching the *query object* | `const movies = await Movie.find({mpaaRating: 'PG'})`|
|`findById` | Find a document based on it's `_id` | `const movie = await Movie.findById(req.params.id)`|
|`findOne`| Find the first document that matches the *query object* | `const movie = await Movie.findOne({releaseYear: 2000})`|

## The `index` route

The purpose of this route is to provide users with a list of all fruits that have been created in our database. In keeping with RESTful routing conventions, the `url` for this route will be: `/fruits`.

## Create the route

Let's start by defining the route and testing it as soon as possible. Add the following code to `server.js`, beneath the landing page route we've already created and above the `/fruits/new` route. This will place our newly created `/fruits` route still above `app.listen()` (remember, all routes MUST be defined above `app.listen()`).

```js
// GET /fruits
app.get('/fruits', (req, res) => {
  res.send('This route sends the user to the index page!');
})
```

Test it out by visiting [localhost:3000/fruits](http://localhost:3000/fruits) directly. If all goes well you should see the following in the browser:

![Index Page Test in Browser](./assets/index-test-browser.png)

## Retrieve the data (fruits)

Now that we know our route is set up, our next step would be to retreive the data we are looking for in the database. In this case, we are looking for all of the fruits.

To do this, we are going to query the database using one of the common methods listed above. In this case, we will use the `.find()` method. When using `.find()`, as stated above, it will return an array of all documents that match the ***query object***.

### What is the ***query object***?

The ***query object*** is just a JavaScript object passed in as the first argument containing properties that will filter out the data we are looking for in a database. For example, if we were looking for all red fruits, the ***query object*** would probably be entered as follows: `{ color: 'red' }`.

Since in our case we are looking for *all* fruits, not just those that are *red*, we won't be including any properties in our ***query object*** so as not to limit the response, so ours will be just an empty object `{}`, hence selecting **ALL** documents. Equivalently, we can also use `.find()` with no arguments and it will still produce the same result.

Let's now use `.find()` inside our `app.get()`:

```js
app.get('/fruits', async (req, res) => {
    const foundFruits = await Fruit.find();
    res.send('This route sends the user to the index page!');
})
```

Notice also that since we will have to wait for the return value of `.find()` to be assigned to a variable, we have changed the callback function to an ***async*** function and made use of ***await*** in the variable assignment.

To be sure we have the data we are looking for, let's log `foundFruits` to the console:

```js
app.get('/fruits', async (req, res) => {
    const foundFruits = await Fruit.find();
    console.log(foundFruits);
    res.send('This route sends the user to the index page!');
})
```

If we created a single banana document in our database, now when we navigate to [localhost:3000/fruits](http://localhost:3000/fruits) the return value logged to the console should be an array with a single element:

![Fruits Data Array](./assets/data-array.png)

## Update the route

Now that we have the data we need our next step is to render that data back to the user. Currently our callback function is responding with `.send()` and the user is seeing this:

![Index Page Test in Browser](./assets/index-test-browser.png)

We are going to want to update this to a `.render()` so that we can create and render a view with full ***dynamic*** HTML back to the user.

We'll be passing in two arguments to `.render()`. The first argument will be a ***string*** for the template we will be rendering (`'fruits/index.ejs'`) and the second argument will be an object with the data we want to pass to that template (`foundFruits`) as a property (key/value pair).

We can also remove the `console.log(foundFruits)` since we already know we have the data we want:

```js
app.get('/fruits', async (req, res) => {
    const foundFruits = await Fruit.find();
    res.render('fruits/index.ejs', { fruits: foundFruits });
})
```

If we navigate to [localhost:3000/fruits](http://localhost:3000/fruits) now, we won't be seeing our fruits array. We'll actually be seeing an error!

![Error No View](./assets/error-view.png)

There will be some additional details but this is the main part of the error we need to focus on. It is actually a *good* thing! It tells us what our next step should be, just in case we weren't sure. We're trying to render a view named `index.ejs` in the `views/fruits` directory, *but* one doesn't exist yet. Simple fix, so let's create that view in our next step!

## Create the `index` template

Now that the route is set up, let's create the template we want this route to render for the user with our list of fruits from the database. We will be putting the `index` template inside the `fruits` sub-folder within the `views` directory we already created:

```bash
touch views/fruits/index.ejs
```

Inside this template file, add the HTML boilerplate, update the `<title>`, and add an `<h1>` inside the `<body>` to get started:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Fruits Index</title>
</head>
<body>
  <h1>Here Are All the Fruits</h1>
</body>
</html>
```

Now if we do a refresh on or navigate to [localhost:3000/fruits](http://localhost:3000/fruits) that should solve our error and render the `<h1>`.

![Index Temporary View](./assets/index-temp-view.png)

Let's finally render all of our fruits. In our last step, we passed that data along as our second argument to the `res.render()` method as the property of an object `{ fruits: foundFruits }`. Therefore, our list of fruits should be the value in the key of `fruits`.

Because that object was passed in `res.render()`, our view now has a variable called `fruits` available which hold the value of the array of all fruits. To render each fruit, we will have to iterate over that `fruits` array in our `EJS`.

Because we are going to render elements in an array, a good option would be to use an unordered list `<ul>`. We'll use the `.forEach()` method to iterate over the elements in the array and then for each **fruit** we will render its name in an `<li>`.

Using the elements and method above, let's plug the following code into the body of our HTML:

```html
<body>
    <ul>
        <% fruits.forEach(fruit => { %>
            <li>
                <%= fruit.name %>
            </li>
        <% }) %>
    </ul>
</body>
```

Another refresh on or navigation to [localhost:3000/fruits](http://localhost:3000/fruits) and we should see our list!

![Fruits List](./assets/fruits-list.png)

🎉 **Congrats on implementing our app's `index` functionality!** 🎉

[tktk Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-the-fruits-index-page-start)

[tktk Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/build-the-fruits-index-page-complete)

# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Update a Fruit](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be to create a route for updating a fruit in MongoDB.

## Step 1: Conceptualize the route

Reviewing the RESTful routes, we know that the route responsible for updating a fruit should be:

```js
PUT /fruits/:fruitId
```

## Step 2: Define the route on `server.js`

Our form in the `Edit` page makes a `PUT` request to `/fruits/:fruitId`, so let's write the route that will handle that request:

```js
// localhost:3000/fruits/:fruitId
app.put('/fruits/:fruitId', async (req, res) => {
  if (req.body.isReadyToEat === 'on') {
    req.body.isReadyToEat = true;
  } else {
    req.body.isReadyToEat = false;
  }
  await Fruit.findByIdAndUpdate(req.params.fruitId, req.body);
  res.redirect(`/fruits/${req.params.fruitId}`);
});
```

This route is fairly similar to the `Create` route that handles creating a new fruit in MongoDB.  Here's what's important to note:

- `app.put` sets this route up to listen for `PUT` requests.  This is the only route that handles `PUT` requests, so it can't conflict with any of the other routes in the same way that the `new` route could conflict with the `show` route.  You can put it anywhere, as long as it comes after any middleware and before `app.listen`.
- The `_id` of the fruit to update is passed in via the path of the request (`/fruits/:fruitId`), so we can use `req.params.fruitId` when invoking `Fruit.findByIdAndUpdate()` to target that specific fruit
- Just like in the `Create` route, the value of `req.body.isReadyToEat` will initially either be set to the string `"on"` if the checkbox on the `Edit` form was checked or it will be `undefined` if it was not checked.  This is in conflict with our Mongoose schema, so we'll use a basic `if/else` to change `req.body.isReadyToEat` to a boolean value, just like we did in the `Create` route.
- As with all of our previous database work, `Fruit.findByIdAndUpdate()` takes a while in computer time, so let's `await` it.  Mark the route's callback function as `async` to get this to work properly.
- We'll use ``res.redirect(`/fruits/${req.params.fruitId}`);`` to send the user back to the fruit's `Show` page.  We could send them pretty much wherever we want, but this seems like a good User Experience.

## You did it!

We're done with the app!  We have full CRUD and all seven RESTful routes on the fruit model!  Well done!

[tktk Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/update-the-fruit-start)

[tktk Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/update-the-fruit-complete)

tktk App flow: We build each route with the following process:

1) Conceptualize the route. No code is written here, all we are doing is deciding what our route should be to make future steps simpler. 
2) Build the UI (link or form) that will hit that route. Send any relevant data on the request. At this point the route should not function! This is a great prompt to move students into the next step:
3) Scaffold the function that corresponds that that route. `console.log()` any relevant data (for example: `req.params`, `req.body`, etc) so we know what we have access to in the function and the shape of the data we're receiving from the front end. `res.send()` (not `res.render()`) a very brief message identifying the page so there is visual confirmation for students that the route worked. Have students thoroughly test and debug at this stage. Avoid code that does more than logging and doing the `res.send()` action at this stage.
4) Write controller action. After confirming the route and data are accurate and accounted for, the rest of the controller action should be crafted.
5) In the case of a GET request, respond with a view (You may need to build the view in this step, that is expected). Any other kind of request should redirect somewhere.

tktk This follows the flow of data into and out of our server, ensuring students can test their work along the way and view data when necessary.

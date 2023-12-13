# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Update a Fruit](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to edit a fruit from their rendered form and see the updated fruit on their MongoDB Atlas dashboard.

## Step 1: Conceptualize the route
We already wrote the route in the action of the form above:

```js
PUT /fruits/:fruitId
```

## Step 2: Create the UI that will issue the request to that route
We already did this, so we can move on to step 3!

## Step 3: Define the route on the server and map it to an exported controller function

Our form has a `PUT` request stubbed up and ready to go, so let's write the route:
```js
// localhost:3000/fruits/:fruitId
router.put("/:fruitId", fruitsCtrl.update)
```

And export the new controller:
```js
function update(req, res) {
  console.log("Remaking fruits!")
}

export {
  index,
  newFruit as new,
  create
  show,
  deleteFruit as delete,
  edit,
  update
}

```

## Step 4: Code the controller function
Once we finish the controller function we'll be done!

```js
function update(req, res) {
  req.body.isReadyToEat = !!req.body.isReadyToEat
  for (let key in req.body) {
    if(req.body[key] === "") delete req.body[key]
  }
  Fruit.findByIdAndUpdate(req.params.fruitId, req.body, {new: true})
  .then(fruit => {
    res.redirect(`/fruits/${fruit._id}`)
  })
  .catch(err => {
    console.log(err)
    res.redirect("/")
  })
}
```

## Step 5: No step 5 here, we're just redirecting!
We have full CRUD on the fruit model! Fantastic!

tktk Notes: Handle edit fruit form submission. Add method and action to edit fruit form.

[tktk Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/update-the-fruit-start)

[tktk Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/update-the-fruit-complete)

tktk App flow: We build each route with the following process:

1) Conceptualize the route. No code is written here, all we are doing is deciding what our route should be to make future steps simpler. 
2) Build the UI (link or form) that will hit that route. Send any relevant data on the request. At this point the route should not function! This is a great prompt to move students into the next step:
3) Scaffold the function that corresponds that that route. `console.log()` any relevant data (for example: `req.params`, `req.body`, etc) so we know what we have access to in the function and the shape of the data we're receiving from the front end. `res.send()` (not `res.render()`) a very brief message identifying the page so there is visual confirmation for students that the route worked. Have students thoroughly test and debug at this stage. Avoid code that does more than logging and doing the `res.send()` action at this stage.
4) Write controller action. After confirming the route and data are accurate and accounted for, the rest of the controller action should be crafted.
5) In the case of a GET request, respond with a view (You may need to build the view in this step, that is expected). Any other kind of request should redirect somewhere.

tktk This follows the flow of data into and out of our server, ensuring students can test their work along the way and view data when necessary.

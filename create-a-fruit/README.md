# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Create a Fruit](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk



tktk Notes: Handle new fruit form submission. Add method and action to new fruit form. Redirect to `/fruits/new` at this time. Add `express.urlencoded({extended: false})` middleware.

[tktk Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/create-a-fruit-start)

[tktk Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/create-a-fruit-complete)

tktk App flow: We build each route with the following process:

1) Conceptualize the route. No code is written here, all we are doing is deciding what our route should be to make future steps simpler. 
2) Build the UI (link or form) that will hit that route. Send any relevant data on the request. At this point the route should not function! This is a great prompt to move students into the next step:
3) Scaffold the function that corresponds that that route. `console.log()` any relevant data (for example: `req.params`, `req.body`, etc) so we know what we have access to in the function and the shape of the data we're receiving from the front end. `res.send()` (not `res.render()`) a very brief message identifying the page so there is visual confirmation for students that the route worked. Have students thoroughly test and debug at this stage. Avoid code that does more than logging and doing the `res.send()` action at this stage.
4) Write controller action. After confirming the route and data are accurate and accounted for, the rest of the controller action should be crafted.
5) In the case of a GET request, respond with a view (You may need to build the view in this step, that is expected). Any other kind of request should redirect somewhere.

tktk This follows the flow of data into and out of our server, ensuring students can test their work along the way and view data when necessary.

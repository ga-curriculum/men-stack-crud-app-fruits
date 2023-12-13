# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Create a Fruit](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to create a route from their rendered form and see the newly created fruit in their collection on their MongoDB Atlas dashboard.

## The CREATE Route
The purpose of this route is to construct a way for our server to handle the data that is sent along with our form submission from new fruits page.  In keeping with RESTful routes conventions, the url for this route will be: `/fruits`.  The HTTP method associated with this route is `POST` so when we build this route we will make it so that it is a "post route".

Since we will be using some data from a form we will need to add some middleware to aid in handling our form submission. In our `server.js` file let's add the following middleware beneath the line of code that brings in our Fruit model.

```javascript
app.use(express.urlencoded({ extended: false }));
```

## Create the route

Let's start by defining the route and testing it as soon as possible. Add the following code to `server.js`, beneath the new page route we've already created and above the `app.listen()` method (remember, all routes MUST be defined above `app.listen()`).

```javascript
// server.js

// POST /fruits
app.post('/fruits', async (req, res) => {
  console.log(req.body);
  res.redirect('/fruits/new');
});
```

## Modify our existing fruits form

We will need to add a couple of attributes to your opening form tag to help determine the route/method combination that allows us to send the data to our newly created `POST` route.  We'll also be adding another input to the form to indicate if the fruit is ready to eat:

```html
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
```
Test it by adding some data into the form and clicking the `submit` button.

## Build our create function in our post route

Now that we can confirm out form calls the route we have scaffolded, let's add the logic that will allow us to create a fruit once this route is hit:

```javascript
// server.js

// POST /fruits
app.post('/fruits', async (req, res) => {
  if (req.body.isReadyToEat === 'on') {
    req.body.isReadyToEat = true;
  } else {
    req.body.isReadyToEat = false;
  }
  await Fruit.create(req.body);
  res.redirect('/fruits/new');
});
```

In the above code snippet, we are using a conditional to add a boolean value to `req.body.isReadyToEat`.  If the value for that key is checked or 'on', we will reset that value to true before sending it to the Mongo database.  If it isn't checked or 'on', we will set it to false.  After that we will send the contents of the form (`req.body`) over to our database via the `.create()` method on the `Fruit` model. Once that fruit is successfully created, we will respond with a redirect to `/fruits/new`.

Let us test that our method works by navigating to our MongoDB Atlas dashboard for the fruits database.  While on the dashboard for MongoDB Atlas, click on the "Collections" section for your database.

![tktkcollections](./assets/originals/collections.png)

Once you can see the collections for your database, find the database that corresponds to this fruits app and click on it.  You should see a collection called "fruits".  Click on that collection and see if you can see the document of your newly created fruit.

![tktkbanana](./assets/originals/banana.png)

If you can see your newly created fruit you have successfully created a method creates a new fruit from your rendered form.  Congrats!!!



[tktk Starter Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/create-a-fruit-start)

[tktk Complete Code](https://git.generalassemb.ly/modular-curriculum-all-courses/building-a-crud-app-with-mongoose-express-and-ejs-fruits-solution/tree/create-a-fruit-complete)

# ![MEN Stack CRUD App - Fruits - Recap](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to summarize our app

We've finished building our first large app! Let's zoom out and observe the moving pieces here.

tktk for QA, might want to refine the below definitions, just a starting point here.

## Technology used

tktk this header name could use work? If you can think of something better please change it.

**JavaScript**: The Programming language of this tech stack

**Node.js**: The terminal application that executes our JavaScript code in the terminal.

**Express**: The web framework managing the request-response cycle.

**EJS**: The view engine we're using to send HTML to clients.

**MongoDB**: Document-based database that persists (i.e. saves) data outside of the application.

**Mongoose**: Allows our app to interact with MongoDB to create, read, update, and delete data. Also allows us to define the shape of data (e.g. what properties exist on a model as well as their data types).

## Request-response in MVC architecture

tktk Hunter, add asset here for MVC architecture (similar to existing content) diagramming the below:

**Client**: A browser (or other application) that issues HTTP requests.

**Server**: An application that listens for HTTP requests.

**`server.js`**: The entry point for our application. Holds the routes and middleware. Initiates the connection to MongoDB and starts Express listening for connections.

**Controllers (in `server.js`)**: Holds functionality specific to a request route. Using a GET request as an example: Interacts with the model (provided by Mongoose) and passes data received from it to the view engine (EJS). Ultimately, it responds to the client (user) with a fully rendered HTML document.

**Model (Mongoose)**: Interfaces with the database. Ensures that data passing through it conforms to a provided schema.

**Database (MongoDB)**: - Persists data. In our case, this data is held in MongoDB Atlas in the cloud.

**View (EJS)**: - A templating language, very similar to HTML, that allows for the execution and injection of JavaScript within the template. This allows express to mix data (most likely coming from a database), in conjunction with the template's JS functionality to create _dynamic_ HTML. The result is a template file whose HTML changes based on the data it's provided.

## Common properties on the `req` object

**`req.body`**: Object holding the form data a user has submitted. The keys on this object will match the name attributes of the inputs in the form and should conform with a model's schema if it is to be saved to a database. The values will match what the user provided in the form.

**`req.params`**: Object holding the URL parameters of a URL. The keys on this object will match the string provided after the `:` in the route. The value of each key will match the data from that segment in the URL.

tktk could add examples on the above for clarity? But also didn't want this to get too in the weeds, but the above might be a little abstract without concrete examples.

## Common methods on the `res` object

**`res.render()`**: Always provided a string as the first argument. That string should be a file path and will never start with a `/`.

**`res.redirect()`**: - Always provided a string as the first argument. That string should be a valid route and will always start with a `/`.

# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Recap](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will be able to tktk

We've finished building our first large app! Let's zoom out and observe the moving pieces here.

tktk for QA, might want to refine the below definitions, just a starting point here.

## Technology roles

tktk this header name could use work

**JavaScript**: The Programming language of this tech stack

**Node.js**: The environment that executes our JavaScript code outside of the browser (therefore we don't have access to browser-specific tech like the DOM).

**Express**: The web framework managing the request-response cycle.

**EJS**: The view engine we're using to send HTML to clients.

**MongoDB**: Document based database that persists data outside of the application.

**Mongoose**: Allows our app to interact with MongoDB create, read, update, and delete data. Also allows us to define the shape of data.

## Request-response in MVC architecture

tktk Hunter, add asset here for MVC architecture (similar to existing content) diagramming the below:

**Client**: A browser (or other application) that issues HTTP requests.

**Server**: An application that listens for HTTP requests.

**`server.js`**: Holds the middleware pipeline and controller functions

**Controllers (in `server.js`)**: Holds functionality specific to a request route. Using a GET request as an example: Interacts with the model (provided by Mongoose) and passes data received from it to the view engine (EJS). Ultimately responds to the client (user) with a fully rendered HTML document.

**Model (Mongoose)**: Interfaces with the database. Ensures that data passing through it conforms to a provided schema.

**Database (MongoDB)**: - Persists data. In our case this data is held in MongDB Atlas in the cloud.

**View (EJS)**: - Renders a complete HTML document using the data returned from the database. Returns a fully rendered HTML document to the controller to send to the client.

## Common properties on the `req` object

**`req.body`**: Object holding the form data a user has submitted. The keys on this object will match the name of the inputs in the form, and should conform with a model's schema (if it is to be saved to a database). The values will match what the user provided in the form.

**`req.params`**: Object holding the URL parameters of a URL. The keys on this object will match the string provided after the `:` in the route. The value of each key will match the data from that position in the URL.

tktk could add examples on the above for clarity? But also didn't want this to get to in the weeds, but the above might be a little abstract without concrete examples.

## Common methods on the `res` object

**`res.render()`**: Always provided a string as the first argument. That string should be a file path and will never start with a `/`.

**`res.redirect()`**: - Always provided a string as the first argument. That string should be a valid route, and will always start with a `/`.

# ![MEN Stack CRUD App - Fruits - Linking Pages within an App](./assets/hero.png)

**Learning objective:** By the end of this lesson, students will understand how to enhance user experience in a web application by integrating a navigation bar across multiple pages.

## Add navigation to our application

Until now, navigating to different pages in our application required manually typing the URL in the address bar of our browser. While it's fine for us as developers, this approach isn't an ideal user experience, as users might not know all the available pages in our application. To the improve the user experience, we will add a basic navigation bar to our Fruits app. This nav bar will provide links for easy and direct access to different pages within the app.

Add the following HTML snippet at the top of each EJS file (`index.ejs`, `new.ejs`, etc.) in your views directory.

```html
<nav>
  <a href="/">Home</a>
  <a href="/fruits">Fruit List</a>
  <a href="/fruits/new">Add Fruit</a>
</nav>
```

Test the links by navigating through the pages in the browser. Ensure that the links correctly lead to the `Home`, `Fruit List`, and `Add Fruit` pages.

### Review

You may have noticed we've been repeating the same navigation bar code in multiple templates, which isn't very efficient. Remember, in coding, we follow the DRY principle - "Don't Repeat Yourself". It's all about reducing repetition to make our code cleaner and easier to update.

EJS offers a better way to use snippets of code like a `nav` bar that needs to be repeated on multiple pages. 



## Including EJS Partials ( optional )

Partials are reusable pieces of code in EJS, allowing us to write a piece of code once and use it across multiple templates.

Make a partials directory inside views:

```bash
mkdir views/partials
```

Create a file named `_navbar.ejs` in the partials directory.

```bash
touch views/partials/_navbar.ejs
```

> Partials are named with an underscore at the beginning like our `_navbar`. This underscore is a common naming convention used to indicate that the file is a partial. 

Copy the nav bar HTML code into `_navbar.ejs`:

```html
<!-- views/partials/_navbar.ejs -->

<nav>
  <a href="/">Home</a>
  <a href="/fruits">Fruit List</a>
  <a href="/fruits/new">Add Fruit</a>
</nav>
```

Replace the nav bar code in each template with the following EJS include statement:

```html
<%- include('../partials/_navbar') %>
```

> Make sure to adjust the path for the `fruits` views vs. the main homepage view.

By using a partial for our nav bar, we centralize its code in one place. This means if we need to add a new link or update an existing one, we only have to make that change in the partial file. Once updated, this change will automatically be reflected across all pages that include the navigation bar partial.
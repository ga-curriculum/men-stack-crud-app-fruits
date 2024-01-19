# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Level Up - tktk Microlesson Name](./assets/hero.png)




1. create public folder and stylesheets folder (in public) to hold css files
2. add the required middleware to `server.js`: app.use(express.static(path.join(__dirname, 'public')));
3. require path with `const path = require('path')`
4. create `index.css` inside stylesheets 
5. create `style.css` inside stylesheets
6. link `index.ejs` with `index.css` and `style.css` --> <link rel="stylesheet" href="/stylesheets/style.css" /><link rel="stylesheet" href="/stylesheets/index.css" />
7. add our fonts from google fonts into `style.css` --> @import url('https://fonts.googleapis.com/css2?family=DM+Sans:opsz@9..40&display=swap');
8. add a style rule for background color and app font in `style.css`:
```css
   body {
  background: #fff8ed;
  font-family: 'DM Sans', sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100vw;
}
```
1.  add a font color rule for h1s in `style.css`: 
   ```css
   h1 {
  color: #769642;
  }
   ```


10. connect `style.css` to the other pages for the h1 rule to work
11. in `index.ejs`, change the text of the `a tag` to 'BROWSE FRUITS'
12.  add a div inside of the `a tag` with the class of `index-link-text` and move the link text inside of that (instead of directly in the `a tag`)
13. Also give the `a tag` a class of `fruit-index-link`
14. in `style.css` add the following css:
```css
.fruits-index-link {
  text-decoration: none;
}

.index-link-text {
  background-color: #f47b5b;
  color: white;
  font-size: 1.4em;
  width: 10em;
  padding: 5px 15px;
  margin: 15px;
  border-radius: 25px;
  text-align: center;
  box-shadow: 0 5px #ffbd59;
}

.index-link-text:hover {
  background-color: #ffbd59;
  box-shadow: 0 5px #f47b5b;
}

.index-link-text:active {
  background-color: #f47b5b;
  box-shadow: 0 2.5px #ffbd59;
} 
```
15. the same way we added a div to the `a tag` on index, we will do that for all the other pages
on `fruits/new.ejs`, `fruits/show.ejs`, and `fruits/edit.ejs`, change the `a tag` to:
```html
<a class="fruits-index-link" href="/fruits">
    <div class="index-link-text">
      BACK TO FRUITS
    </div>
  </a>
``` 








tktk Optional student-facing level up content should be added here. Not all modules will have Level Up content - if it doesn't, the `level-up` directory should be deleted, and the **Level Up content** section in the main `README.md` file should be removed.

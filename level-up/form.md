# ![Building a CRUD App with Mongoose, Express, and EJS - Fruits - Level Up - tktk Microlesson Name](./assets/hero.png)



1. on `new.ejs` add a div around the label and input for "ready to eat", and give it a class of `checkbox-div`:
```html
    <div class="checkbox-div">
      <label for="ready-to-eat">Ready to Eat?</label>
      <input type="checkbox" name="isReadyToEat" id="ready-to-eat">
    </div>
```
2. add this css to `style.css`
```css
.checkbox-div {
  flex-direction: row;
}

label, input{
  margin: 10px 2px 0 2px;
}

button {
  align-self: center;
  margin: 10px 4px 4px 4px;
  background-color: #3f6a51;
  color: white;
  padding: 5px 10px;
  border-radius: 15px;
  border: none;
  width: 15em;
}

button:hover {
  background-color: #769642;
}

button:active {
  background-color: #2d4d3a;
}
```

tktk Optional student-facing level up content should be added here. Not all modules will have Level Up content - if it doesn't, the `level-up` directory should be deleted, and the **Level Up content** section in the main `README.md` file should be removed.

# typewriter

Vue3 typewrite component! The typewriter inserts text into a span to allow for inline placement. It's usually a good idea to have the typewriter in a fixed width container with text aligned left if the cursor selection is on. This is due to how the blinking of the cursor changes the width of the overall body of text, causing the entirety of the text to move left when the cursor renders. 

Props: 
- text: Can be a string or an array of strings. If it's an array of strings, the typewriter will cycle through the strings from 0 -> len (repeating back to 0 if repeats prop is true).
- delete: Can be either 'full' or 'type'. 'full' will cause the typewriter to delete the entire string upon completion of the typing. 'type' will cause the same typing effect to occur during deletion. 
- repeats: Defaults to false. Decides whether text should repeatedly print and delete. If false, typewriter emits 'done' upon completion. 
- speed: Defaults to 100ms.  Determines how long the typewriter will wait between printing each character (or deleting each character if delete is set to type).
- waitAfterWrite: Defaults to 1500ms. Determines the amount of time the typewriter will wait before deleting a completed string. 
- waitAfterDelete: Defaults to 300ms. Determines how long the typewriter waits after deleting the string. 
- humanize: Defaults to true. Adds a random amount of time ranging from 0-100ms to the writing speed for each character. Intended to mimic the variability of human typing. 
- cursor: Defaults to false. If true, adds blinking cursor ('_') to the end of the typewriter string. 

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

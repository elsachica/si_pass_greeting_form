# Example SI Pass Project

A simple web application to practice JavaScript and form handling. This project prompts the user to enter their name, and upon submission, it displays a personalized greeting message.

## Getting Started

1. **Install dependencies**:
   ```bash
   npm install
   ```
2. **Start the server**: Run the application locally with:
   ```bash
   npm start
   ```
   This will start a local server and open the application in your default browser.

## Project Files

- **index.html**: The main HTML file, displaying a form for entering a name and a button to submit.
- **form.js**: Contains the JavaScript functionality for handling the form submission. It listens for the form submit event, fetches the user’s name, and navigates to a greeting page.
- **greeting.html**: The page displayed after form submission, showing the personalized greeting message, "Hej (user's name)!".

## Usage

Run `npm start` to view the project locally. Enter your name in the form and submit to see the greeting message.

## How to Write the Code

### `index.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hälsningsformulär</title>
</head>
<body>
  <greeting-form></greeting-form>
  <script src="form.js" type="module"></script>
</body>
</html>
```

### `form.js`
```js
const template = document.createElement('template')
template.innerHTML = `
  <style>
    div {
      text-align: center;
      margin: 20px;
    }
    form {
      display: inline-block;
      background-color: #f0f0f0;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    label, input {
      display: block;
      margin-bottom: 8px;
    }
    input[type="submit"] {
      background-color: #4caf50;
      color: white;
      cursor: pointer;
    }
  </style>
  <div>
    <form id="greetingForm">
      <label for="name">Skriv ditt namn:</label>
      <input type="text" id="name" placeholder="Ditt namn" required>
      <input type="submit" value="Skicka">
    </form>
  </div>
`

customElements.define('greeting-form',
  class extends HTMLElement {
    #form
    #nameInput

    constructor () {
      super()
      this.attachShadow({ mode: 'open' }).appendChild(template.content.cloneNode(true))
      this.#form = this.shadowRoot.querySelector('#greetingForm')
      this.#nameInput = this.shadowRoot.querySelector('#name')
    }

    connectedCallback () {
      this.#form.addEventListener('submit', this.#handleSubmit.bind(this))
    }

    disconnectedCallback () {
      this.#form.removeEventListener('submit', this.#handleSubmit.bind(this))
    }

    #handleSubmit (event) {
      event.preventDefault()
      const name = this.#nameInput.value.trim()
      if (name) {
        window.location.href = `greeting.html?name=${encodeURIComponent(name)}`
      }
    }
  }
)
```

### `greeting.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Hej!</title>
</head>
<body>
  <h1 id="greetingMessage"></h1>
  <script>
    const params = new URLSearchParams(window.location.search)
    const name = params.get('name')
    document.getElementById('greetingMessage').textContent = `Hej, ${name}!`
  </script>
</body>
</html>
```

## Dependencies

- **live-server**: A simple, zero-configuration, command-line local server. It auto-refreshes when files change, making it easy to test your application in the browser.
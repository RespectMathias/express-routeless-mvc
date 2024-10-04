# express-routeless-mvc

**⚠️ Experimental - Under Development ⚠️**

Express middleware for dynamic, file-based routing without explicit route definitions, supporting flexible MVC structures. **Controller and view names must currently match.**

## Installation

```bash
npm install express-routeless-mvc
```

## Usage

```javascript
const express = require('express');
const setupRoutelessMVC = require('express-routeless-mvc');
const path = require('path');

const app = express();

// Register compilers if needed (e.g., CoffeeScript, TypeScript)

// Setup other middleware (logger, bodyParser, etc.)

setupRoutelessMVC(app, {
  viewsDir: path.join(__dirname, 'views'),
  controllersDir: path.join(__dirname, 'controllers'),
  viewEngine: 'html',                // Default: 'html'
  viewExtensions: ['.html'],         // Default: ['.html']
  controllerExtensions: ['.js'],     // Default: ['.js']
  staticDir: path.join(__dirname, 'public'), // Default: 'public'
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

## Features

- **Dynamic Routing:** Automatically maps routes based on the `views` directory structure.
- **Flexible MVC:** Organize controllers and views without manual route definitions.
- **Static File Serving:** Serves static assets from the designated `public` directory.

## Options

- `viewsDir` (String): Directory for view templates. Default: `'views'`.
- `controllersDir` (String): Directory for controllers. Default: `'controllers'`.
- `viewEngine` (String): Template engine to use. Default: `'html'`.
- `viewExtensions` (Array): Supported view file extensions. Default: `['.html']`.
- `controllerExtensions` (Array): Supported controller file extensions. Default: `['.js']`.
- `staticDir` (String): Directory for static assets. Default: `'public'`.

## Important

- **Experimental:** Use with caution in production environments.
- **Naming Convention:** Controller and view filenames must match to associate them with the same route.
  - *Example:* For route `/about`, use `views/about.html` and `controllers/about.js`.

## Controllers

Optional functions that provide data to views.

**Example: `controllers/index.js`**
```javascript
module.exports = (req, res, next) => ({
  title: 'Home Page',
  message: 'Welcome to our website!'
});
```

## Views

Place templates in the `viewsDir`. Filename determines the route.

**Example: `views/index.html`**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Home Page</title>
</head>
<body>
    <h1>Welcome to our website!</h1>
</body>
</html>
```

## Error Handling

Add middleware to handle 404 and other errors.

```javascript
// 404 Handler
app.use((req, res) => {
  res.status(404).sendFile(path.join(__dirname, 'views', '404.html'));
});

// Error Handler
app.use((err, req, res, next) => {
  res.status(err.status || 500).send('Internal Server Error');
});
```

## Notes

- **Compilers:** Register necessary compilers before setting up the middleware.
  ```javascript
  // For CoffeeScript
  require('coffeescript/register');

  // For TypeScript
  require('ts-node/register');
  ```

- **Changing View Engine:**
  ```javascript
  // Install Pug
  npm install pug

  // Setup with Pug
  setupRoutelessMVC(app, {
    viewEngine: 'pug',
    viewExtensions: ['.pug'],
  });
  ```

## License

[MIT](LICENSE)
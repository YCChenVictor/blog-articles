# Title

## Purpose

This article describes the key steps to build application with express, a light-weighted framework based on node. Inherited from node, it features real-time, high-performance, and scalable network. NodeJS is built in javascript.

## Concept

### Init

* Install dependencies
  ```bash
  yarn add express dotenv cors
  yarn add nodemon --save-dev
  ```
  * express: simplifies web development in Node.js by providing a framework for handling HTTP requests, defining routes, implementing middleware, and rendering dynamic views
  * cors: CORS (Cross-Origin Resource Sharing) is primarily enforced by the web browser, which checks the server's response headers to determine if a cross-origin request is permitted based on the requesting domain. This helps prevent unauthorized cross-origin requests and enhances security by ensuring that resources are only accessed by allowed domains.
  * dotenv: allows you to load environment variables from a .env file
  * nodemon: a development utility that automatically restarts the server whenever changes are made to the code
* Initialize
  ```bash
  mkdir my-project
  cd my-project
  yarn init
  ```
* `touch .gitignore` with
  ```bash
  # Dependency directories
  node_modules/
  
  # Optional npm cache directory
  .npm
  
  # dotenv environment variables file
  .env
  ```
* env: In `./.env`, we can add environment variables we want; for example, how to connect development database ...etc
  ```javascript
  process.env.xxx
  ```
* ES6: In `package.json`
  ```JSON
  {
    "type": "module",
    ...
  }
  ```
* Typescript
  * `npx tsc --init`
  * `yarn add ts-node`
  * `yarn add tsconfig-paths`
  * `yarn add express @types/express --save`
  * `npx tsc`
  * Update `package.json`
    ```JSON
    "scripts": {
      "start": "node dist/app.ts",
      "dev": "NODE_ENV=development nodemon ./src/app.ts", // If you use nodemon for development
      "build": "tsc"
    }
    ```
  * Allow ES, in `tsconfig.json`
    ```JSON
    {
      "compilerOptions": {
        "esModuleInterop": true,
      }
    }
    ```
  * Update `nodemon.json`
    ```JSON
    {
      "watch": ["src"],
      "ext": "ts,json",
      "ignore": ["src/**/*.spec.ts"],
      "exec": "node --loader ts-node/esm"
    }
    ```
  * run app through `yarn run dev`
* `touch server.ts` with
  ```javascript
  // init
  import express from 'express'
  const app = express();
  
  if (process.env.NODE_ENV === 'development') {
    app.listen(5000, () => {
      api(app)
    })
  } else if (process.env.NODE_ENV === 'test') {
    app.listen(8080, () => {
      api(app)
    })
  } else {
    // TODO for production
  }
  
  export default app
  ```
* test it with curl: `curl http://localhost:5000/`

### structure

The structure I prefer

* node app
  * test
  * routes
  * controllers
  * configs
  * models
  * database
    * migrations
  * middleware
  * service in monolith
  * server.js (core file to start the app)

```mermaid
graph LR
  DB((Database)) <--> M((Models))
  M((Models)) <--read/write data--> C((Controllers))
  R((Routes)) --Forward request--> C((Controllers))
  Request([HTTP Request]) --> R((Routes))
  C((Controllers)) --> Response([HTTP Response])
```

#### Naming Convention

* File: use hyphens; for example, `user-controller.js`
* Url: use hyphens; for example, `/node-graph`

### Basic Structure

```javascript
  const express = require('express');
  const app = express();
  const port = 3000;

  // Define a route with a parameter
  app.get('/api/:paramName', (req, res) => {
    const paramName = req.params.paramName;
    res.send(`Received parameter: ${paramName}`);
  });

  // Start the server
  app.listen(port, () => {
    console.log(`Server running on http://localhost:${port}`);
  });
  ```

### Routes

In Express.js, "Routes" are used to direct incoming requests to the appropriate controller functions. These routes are defined based on the HTTP method (like GET or POST) and the URL of the request. Any information encoded in the request URL, such as parameters or query strings, is also forwarded to the controller function.

```js
const express = require('express');
const router = express.Router();
const userController = require('./controllers/user-controller');

// Route for GET request to /users
router.get('/users', userController.getUsers);

// Route for POST request to /users
router.post('/users', userController.createUser);

module.exports = router;
```

### Controllers

"Controller" functions are responsible for handling requests once they've been routed. They interact with the model to get the requested data, create an HTML page (or some other type of response) displaying the data, and send it back to the user to view in their browser.

```js
// In user-controller.js
exports.getUsers = (req, res) => {
  // Get data from model
  // Create HTML page with data
  // Send response to user
};

exports.createUser = (req, res) => {
  // Get data from request
  // Use model to create new user
  // Send response to user
};
```

#### REST

```javascript
// restful API
// GET /records -> records#index
// POST /records -> records#create
// GET /records/new -> records#new
// GET /records/:id/edit -> records#edit
// GET /records/:id -> records#show
// PATCH /records/:id -> records#update
// PUT /records/:id -> records#update
// DELETE /records/:id -> records#destroy

const modelRecord = require('../database/models/record.js')

module.exports = (app) => {
  // TODO: add RESTful api for record here
}
```

#### internal consume

Just call them with proper endpoint

```javascript
request({
  url: 'http://127.0.0.1:3000/api/', //on 3000 put your port no.
  method: 'POST',
  json: {
    obj: Obj
  }
  }, function (error, response, body) {
    console.log({error: error, response: response, body: body});
});
```

### inherit

In Express, you can create a base API router and then have other routers inherit from it. This can help you organize your routes and middleware effectively. Here's how you can achieve this:

1. Create a Base API Router:

```javascript
// baseRouter.js
const express = require('express');

const baseRouter = express.Router();

// Add middleware or routes specific to the base API here
baseRouter.use((req, res, next) => {
  // Add any base API middleware logic here
  next();
});

module.exports = baseRouter;
```

2. Create Specific Routers Inheriting from the Base Router:

```javascript
// userRouter.js
const express = require('express');
const baseRouter = require('./baseRouter'); // Import the base router

const userRouter = express.Router();

// Add middleware or routes specific to user API here
userRouter.use((req, res, next) => {
  // Add any user-specific middleware logic here
  next();
});

// Use the baseRouter middleware in userRouter
userRouter.use(baseRouter);

// Add user-specific routes here
userRouter.get('/profile', (req, res) => {
  // User profile route logic
});

module.exports = userRouter;
```

3. Use the Routers in Your Main Application File:

```javascript
// app.js
const express = require('express');
const userRouter = require('./userRouter'); // Import the user router

const app = express();

// Use the userRouter middleware in the main app
app.use('/api/user', userRouter);

// Other app-level configurations and routes can be added here

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

In this example, the `userRouter` inherits from the `baseRouter`. You can create additional routers following the same pattern, each inheriting from the `baseRouter` or other relevant routers. This approach helps you keep your code modular and organized.

### Database

[database](/blog/software/express/database)

### Model

Various libraries and frameworks such as Mongoose, Sequelize, or Bookshelf can be used to implement models in Node.js, providing an ORM layer for interacting with databases and managing data models. For more information, please refer to [model]({{site.baseurl}}/node/2022/01/20/model.html)

### Service in monolithic

In a monolithic architecture, a service can refer to a module or a set of related functionality within the application. These services are not independent applications; they are part of the same application and run in the same process. However, they are often organized in a way that separates concerns and makes the application easier to understand and manage.

You can think of the service layer as an intermediary between the controller and model layers in an application. When you find your controllers are too fat and want to extract the code.

Suppose you have two model, product, order. To create a order, you need to check whether there is enough stock in these products. Now, here is the key issue. Think about whether this **checking** is a flow. If the checking may have different route, then it is a flow, then you should not put this logic in model; as this may impose to all the orders. However, as controller mainly deal with http request, we should not put the flow in controller, causing fat controller. As a result, we create services to deal with this kind of flow.

The service:

```javascript
// OrderService.js
class OrderService {
  constructor(productModel, orderModel) {
    this.productModel = productModel;
    this.orderModel = orderModel;
  }

  async createOrder(orderData) {
    const product = await this.productModel.findById(orderData.productId);

    if (product.stock >= orderData.quantity) {
      const order = await this.orderModel.create(orderData);

      product.stock -= orderData.quantity;
      await product.save();

      return order;
    } else {
      // Not enough stock, create a future order
      const futureOrder = await this.orderModel.create({
        ...orderData,
        isFutureOrder: true
      });

      return futureOrder;
    }
  }
}

module.exports = OrderService;
```

The controller:

```javascript
// OrderController.js
const OrderService = require('./OrderService');
const ProductModel = require('./models/Product');
const OrderModel = require('./models/Order');

const orderService = new OrderService(ProductModel, OrderModel);

class OrderController {
  async createOrder(req, res) {
    try {
      const order = await orderService.createOrder(req.body);
      res.status(201).json(order);
    } catch (error) {
      res.status(500).json({ error: error.toString() });
    }
  }
}

module.exports = OrderController;
```

### micro-services

### Test

I prefer jest.

* Install
  ```nash
  yarn add jest
  ```
* In `/test`, run jest
  * Setup script, in `package.json`
    ```JSON
    "scripts": {
      "test": "NODE_ENV=test jest",
    },
    ```
* Create spec directory in root, and run tests in specific directory
  ```bash
  npx jest ./tests --detectOpenHandles
  ```
* Environment variables, in `.env` add following to setup test database
  ```bash
  TEST_DATABASE_URL={database_system}://{username}:{password}@{host_and_port}/{database_name}
  ```
* Refer to [Docker] for setting up test database; for example, PG
  ```bash
  docker run --name my_db \
      -e POSTGRESQL_PORT=5432 \
      -e POSTGRESQL_DB=my_db \
      -e POSTGRESQL_USER=postgres \
      -e POSTGRES_PASSWORD=test1234 \
      -d postgres
  ```
* Use `request(app)` to test the api
  ```javascript
  const request = require('supertest');
  const app = require('../app'); // Import your app
  
  describe('Test the /api path', () => {
    test('It should response the GET method', async () => {
      const response = await request(app).get('/api');
      expect(response.statusCode).toBe(200);
    });
  });
  ```
* Reset database before each case to drop and create tables
  ```javascript
  beforeEach(async () => {
    sequelize.truncate({ cascade: true, restartIdentity: true });
  });
  ```
* Mock: In `spec_config`
  ```javascript
  jest.mock('pg', () => { // should extract to other file
    const mPool = {
      connect: function () {
        return { query: jest.fn() };
      },
      query: jest.fn(),
      end: jest.fn(),
      on: jest.fn(),
    };
    return { Pool: jest.fn(() => mPool) };
  });
  ```

### debugger

In `package.json`,

```JSON
"scripts": {
  "dev": "NODE_ENV=development npx nodemon --inspect server.js",
}
```

In vscode, click `Run and Debug` in the sidebar

In the top, click `Run Script: dev`

Then we can debug the code in [debug console]

## Example

### user

```javascript
import User from '../models/user.js';
import jwt from 'jsonwebtoken';
import passport from '../middleware/passport.js';

const userAPIs = (app) => {
  app.post('/signup', async (req, res) => {
    const { username, password } = req.body;

    try {
      // Create a new user using the User model
      const newUser = await User.create({ username, password });

      res.json({ message: 'Signup successful', user: newUser });
    } catch (error) {
      console.error(error);
      res.status(500).json({ message: 'Internal server error' });
    }
  });

  app.post('/login',
    passport.authenticate('local', { failureRedirect: '/login-failure' }),
    (req, res) => {
      res.json({ message: 'Login successful', user: req.user });
    }
  );

  app.get('/authentication', passport.authenticate('jwt'), (req, res) => {
    res.status(200).json({ loggedIn: true });
  });
};
```

### external API

A real quick example:

```javascript
const axios = require('axios');

const data = {
  param1: 'value1',
  param2: 'value2'
};

axios.post('https://api.example.com/path', data)
  .then(response => {
    console.log(response.data);
  })
  .catch(error => {
    console.log(error);
  });
```

#### Youtube

* Purpose: I want to manage my account’s subscription in a mathematical way
* Repo: https://github.com/googleapis/google-api-ruby-client
* Documentation: https://developers.google.com/youtube/v3/guides/auth/server-side-web-apps#node.js
* Quota (refresh every day): https://console.cloud.google.com/apis/api/youtube.googleapis.com/quotas
* Steps
  * Install required package: `npm install googleapis`
  * Get the Key
    * Start a project in [console.cloud](https://console.cloud.google.com/apis/library/youtube.googleapis.com?project=mineral-balm-313306)
    * Get the key in [Service Detail](https://console.cloud.google.com/apis/api/youtube.googleapis.com/credentials?project=mineral-balm-313306)
    * Put it in `dotenv`
  * Init `youtube`
    ```javascript
    const { google } = require("googleapis");
    const youtube = google.youtube({
      version: "v3",
      auth: apiKey,
    });
    ```
  * Use API
    ```javascript
    app.get("/search-with-googleapis", async (req, res, next) => {
      try {
        const searchQuery = req.query.search_query;
        const response = await youtube.search.list({
          part: "snippet",
          q: searchQuery,
        });

        const titles = response.data.items.map((item) => item.snippet.title);
        res.send(titles);
      } catch (err) {
        next(err);
      }
    });
    ```
    * Test it with: `curl http://localhost:5000/search-youtube-with-googleapis`

#### ChatGPT

* Sign up for an OpenAI account
* Create an API key in OpenAI account dashboard. This key will be used to authenticate requests to the API
* Install the openai package
  ```bash
  npm install openai
  ```
* connect
  ```javascript
  const openai = require('openai');

  // Set up the OpenAI API credentials
  openai.apiKey = 'YOUR_API_KEY';

  // Set up the request parameters
  const prompt = 'Hello, ChatGPT!';
  const model = 'text-davinci-002';
  const temperature = 0.5;
  const maxTokens = 100;

  // Send the request to ChatGPT
  openai.complete({
    engine: model,
    prompt: prompt,
    temperature: temperature,
    maxTokens: maxTokens,
  }).then(response => {
    console.log(response.data.choices[0].text);
  }).catch(error => {
    console.log(error);
  });
  ```

## Reference

[How to organize routes in Nodejs Express app](https://stackoverflow.com/questions/59681974/how-to-organize-routes-in-nodejs-express-app)

[How to use .env file in node.js](https://dev.to/dallington256/how-to-use-env-file-in-nodejs-578h)

[Model Querying - Finders](https://sequelize.org/docs/v6/core-concepts/model-querying-finders/)

[jest debug](https://jestjs.io/docs/troubleshooting)

[Mocking a Database in Node with Jest](https://www.youtube.com/watch?v=IDjF6-s1hGk)

[Node.js v19.5.0 documentation](https://nodejs.org/api/http.html)

[npm Passport 筆記（Learn to Use Passport JS）](https://pjchender.dev/npm/npm-passport/)

[Password hashing in Node.js with bcrypt](https://blog.logrocket.com/password-hashing-node-js-bcrypt/)

[](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs/routes)

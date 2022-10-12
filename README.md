# Schema Design and Building RESTful APIs: ToDo List

Build a full CRUD, RESTful API using Express and Postgres for a Todo List with two tables:
* `Users` Table
* `Todos` Table

## Lab Directions
* Create all of your necessary app files in this directory. (I have included GitHub's standard Node.js `.gitignore` template so that you don't end up pushing `node_modules` to GitHub. 
* You should `npm install express pg knex`. Use express to build your API. Use knex to create your database migrations and seeds. Use pg to connect your express API to your database.
* Your core app logic must be separated into the appropriate files, and your project files should be organized into folders according to their purpose: models, controllers, and/or routes.

### API Endpoints
**Each endpoint should respond with the appropriate JSON response. Our API should support:**

| HTTP Verb | PATH                | Description                                          |
|-----------|---------------------|------------------------------------------------------|
| GET       | /users              | A route to see all users                             |
| POST      | /users              | Create a user                                        |
| GET       | /users/:id/todos    | A route to see all the todos that belong to the user that matches the given id  | 
| GET       | /todos/:id          | A route to see details about an individual todo item |
| POST      | /todos              | Create a todo                                        |
| PUT/PATCH | /todos/:id          | Update a todo's description and completion           |
| DELETE    | /todos/:id          | Delete a todo.                                       |


## Setting Up Your App for Deployment

In order to make your application work both locally and when deployed, you will need to have configurations that works for both your local machine and in Heroku. On the Heroku servers, there will automatically some variables that do not existing locally. These variables exist on the `process.env` object. 

### Configuring Your Port

In your main server file, update the line of code where you declare your port to:
```js
const PORT = process.env.PORT || 8000; // Or whichever port you choose for your local server
```
This allows your application to set the port number to Heroku's production port. Since this variable is `undefined` locally, it will be set to `8000` (or whichever port number you choose) when you are developing on you personal computer. 

### Configuring Your Database Connection

If your application is running on Heroku, there will be a variable called `process.env.NODE_ENV` with a value of `'production'`. If your are on you local machine, then this variable will be `undefined`. Depending on which environment you are in, you'll need to use a different `pg` connection object. We can check with a basic ternary operator like so: 

```js
const { Pool } = require('pg')

const connectionDevelopment = {
  database: 'todo',  // Or replace this with your database name
  user: 'postgres',              // If you have a different postgres user, replace here
  password: '',                  // If you have a postgres password, write it here
  host: 'localhost',
  port: 5432
}

const connectionProduction = {
  connectionString: process.env.DATABASE_URL, 
  ssl: {rejectUnauthorized: false}
}

const pool = new Pool(process.env.NODE_ENV === 'production' ? connectionProduction : connectionDevelopment)
```

### Configuring Your knexfile

The exports of your `knexfile.js` should have two properties, one for development and one for production, like so:

```js
module.exports = {
  development: {
    client: 'pg',
    connection: {
      database: 'todo',       // Or replace this with your database name
      user:     'postgres',
      password: ''
    }
  },
  production: {
    client: 'pg',
    connection: process.env.DATABASE_URL
  }
};
```

## Final Steps: Deploy Your API

**Deploy Your Project to Heroku and submit the link on Canvas. Provide the URL to your github repo as a comment in your submission.** When your app is deployed, be sure to test that is works by making API calls to it.

You can read [this tutorial](https://github.com/The-Marcy-Lab-School/deploy-node-app-to-heroku/blob/main/README.md) on deploying an Express app with Postgres on Heroku and running knex migrations and seeds.

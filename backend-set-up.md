> Need to add:
>
> - dovenv

# Table of contents

- [Github](#Github)
- [Express](#Express)
- [Knex](#Knex)

## From a pre-existing project

- `npm install` to install module dependencies listed in package.json

## Project from scratch

### Github

- create new repo on github
- `git clone repoUrl`
- `cd` into folder
- `git add` & `git commit`
- `git push -u origin branchName`

### Node package manager

- `npm init -y`: create package.json file
- `npx gitignore node`: create .gitignore file with nodemodules in it
- Add depencies. Doing these creates & updates package-lock.json.
  - `npm i express sqlite3 knex knex-cleaner cors bcryptjs`: Express for the node framework/network stuff. Sqlite3 for the database driver. Knex for building SQL queries in JS.
  - `npm i -D nodemon`: installs nodemon as a dev dependency, which monitors changes in node and automatically restarts the server.
- In package.json, add the following:
  ```js
    "scripts": {
      "start": "node index.js",
      "server": "nodemon index.js"
    }
  ```

## Express

> create server

- `touch index.js` ==> nodemon watches this. Import server logic and listen on port.

  ```js
  const server = require("./api/server.js");

  const port = process.env.PORT || 4444;

  server.listen(port, () => console.log(`\n\nSERVER UP!\n\n`));
  ```

> using Express framework

- `touch server.js` ==> server logic, routes, middleware, etc. Replace `name` with the `resource`

  ```js
  const express = require("express");
  const nameRouter = require("../name/name-router.js");

  const server = express(); // create instance of Express application to configure server

  server.use(express.json());

  // Sanity check - basic route handler which executes on every `GET` request to URL specified.
  server.get(`/`, (req, res) => {
    res.status(200).json({ api: "I am up!" });
  });

  // ROUTES
  server.use(`/api/name`, nameRouter);

  module.exports = server;
  ```

> If using http module

```js
const http = require("http"); // built in node.js module to handle http traffic

const hostname = "127.0.0.1"; // the local computer where the server is running
const port = 3000; // a port we'll use to watch for traffic

const server = http.createServer((req, res) => {
  // creates our server
  res.statusCode = 200; // http status code returned to the client
  res.setHeader("Content-Type", "text/plain"); // inform the client that we'll be returning text
  res.end("Hello World from Node\n"); // end the request and send a response with the specified message
});

server.listen(port, hostname, () => {
  // start monitoring for connections on the port specified
  console.log(`Server running at http://${hostname}:${port}/`);
});
```

## Express Router

> Use Express Router to modularize your code (route <> resource)

```js
const express = require("express)
const router = express.Router()

router.get(`/`, (req,res)=> {
  res.status(200).send("hello from /resourceName endpoint")
})

router.get(`/:id`, (req,res)=> {
  const id = req.params.id
  res.status(200).send(`hello from /resourceName/${id} endpoint`)
})

module.exports = router
```

## Knex

> Note: npx allows us to run programs out of our local node_modules.

- `npx knex init` - creates knexfile.js

  - can remove staging & development from knexfile.js if not needed.
  - `pool` ==> Only needed for SQLite because it doesn't enforce foreign keys. Not needed for most databases.
  - `migrations` & `seed` keys to specify directory of where to save.

    ```js
    module.exports = {
      development: {
        client: "sqlite3",
        useNullAsDefault: true,
        connection: {
          filename: "./data/databaseName.db3"
        },
        pool: {
          afterCreate: (conn, done) => {
            conn.run("PRAGMA foreign_keys = ON", done);
          }
        },
        migrations: {
          directory: "./data/migrations"
        },
        seeds: {
          directory: "./data/seeds"
        }
      }
    };
    ```

- `mkdir data` & `touch dbConfig.js`

  ```js
  const knex = require("knex");
  const knexConfig = require("../knexfile.js");
  const db = knex(knexConfig.development);
  module.exports = db;
  ```

## Set up migrations:

> Set up your schema

- `npx knex migrate:make bootstrap` ==> create schema for your database. File has timestamp in the beginning and is a template for you to fill.

- `npx knex migrate:latest` to create database

> To make any changes to your schema...

- `npx knex migrate:rollback` which runs the down function - only for local development, not production. Usually, use only migrate:up or migrate:down.
- Or `npx knex migrate:down` and `npx knex migrate:latest`

## Add starter data to server:

- `npx knex seed:make tableName` ==> creates seeds/tableName.js template

- `npx knex seed:run` ==> Adds data to table.

> TRUNCATE - deletes data inside table, but not table itself. Must truncate in the correct order (tbl with dependent keys, t<<hen tbl with primary key)

## Models

```js
const db = require("../data/db.js");

module.exports = {
  find
};

function find() {
  return db("movies");
}
```

## Router

```js
const reouter = require("express").Router();
const Movies = require("./movie-model.js");

router.get(`/`, (req, res) => {
  Movies.get().then(movies => res.status(200).json(movies));
});

module.exports = router;
```

## Sessions & Cookies

- `npm i express-session`

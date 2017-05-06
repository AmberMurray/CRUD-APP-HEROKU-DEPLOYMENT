*** Start in project root directory ****

# Without Express Generator:
- npm init -y
- npm install --save express body-parser pg knex
- npm install --save-dev morgan nodemon
- touch app.js knexfile.js .gitignore
- Add node_modules/ to the .gitignore
- Add connection information to the knexfile.js
- Require necessary packages into app.js and write code to get your server started

# With Express Generator:
- `npm install -g express-generator` (this is a global install)
- Go to a new empty folder
- `express --hbs --git`
- `npm install` *installs boilerplate dependencies*
- cd into the folder:
- `git init`
- `git commit -m "initial commit"`
- `touch knexfile.js`
- setup knexfile.js config
```
  const path = require('path')

  module.exports = {
    development: {
      client: 'pg',
      connection: 'postgres://localhost/cookies_crud',
      migrations: {
        directory: path.join(__dirname, 'db', 'migrations')
      },
      seeds: {
        directory: path.join(__dirname, 'db', 'seeds')
      }
    },
    production: {
      client: 'pg',
      connection: process.env.DATABASE_URL,
      migrations: {
        directory: path.join(__dirname, 'db', 'migrations')
      },
      seeds: {
        directory: path.join(__dirname, 'db', 'seeds')
      }
    }
  }
```

- `npm install --save pg knex method-override body-parser cookie-parser hbs` (this may resinstall stuff that's already there - FYI)
- `npm install --save-dev nodemon morgan` 
- package.json: add 
  * dev script with nodemon
  * knex script for knex
  * start script (with or without nodemon, this step is necessary for heroku deployment) 

# Databaase
- Create a database folder and a connection file
   `mkdir db && touch db/connection.js` (connection.js is aka: knex.js)
- Set up connection.js config
```
  const env = process.env.NODE_ENV || 'development';
  const config = require('../knexfile.js')[env];
  const knex = require('knex')(config);

  module.exports = connection; (the name here has to be the name of the file - connection.js, knex.js...)
```
- Require the knex connection wherever you have routing
   `var knex = require('../db/connection')`
- Create your database on the command line
   `createdb [your_db_name]`
- Create your tables with migrations: `npm run knex migrate:make [your_table_name]`
   ```
   'use strict'

  exports.up = function(knex) {
  return knex.schema.createTable('messages', (table) => {
    table.increments()
    table.string('name', 255).notNullable()
    table.string('message', 255).notNullable()
    table.timestamps(true, true)
  })
}

exports.down = function(knex) {
  return knex.schema.dropTable('messages')
}
```
- Run the table: `npm run knex migrate:latest`
- Make seeds: `npm run knex seed:make [1_your_seed_data_name]`
```
'use strict'

exports.seed = function(knex) {
  return knex('messages').del()
  .then(() => {
    return knex('messages').insert([{
      id: 1,
      name: 'Criminal',
      message: 'What Are You?',
      created_at: new Date('2016-06-26 14:26:16 UTC'),
      updated_at: new Date('2016-06-26 14:26:16 UTC')
    }, {
      id: 2,
      name: 'Batman',
      message: 'I\'m Batman',
      created_at: new Date('2016-06-26 14:26:16 UTC'),
      updated_at: new Date('2016-06-26 14:26:16 UTC')
    }])
  })
  .then(() => {
    return knex.raw(
      "SELECT setval('messages_id_seq', (SELECT MAX(id) FROM messages))"
    )
  })
}
```
- Run the seeds: `npm run knex seed:run`


# knex migrations

$ npm run knex migrate:make [table name]
  * edit migration file to create table
$ npm run knex migrate:latest
  *this creates the table in the database*
$ npm run knex seed:make 1_[table name]
  * edit the seed file to insert data, make sure to delete the data before inserting data
$ npm run knex seed:run
  *seeds the table with your data*


# setting routes

* set route with a method and attach a named function
* code function to return a simple message
  - test
* build html template to render and have function render that template
  - test
* hard-code variables in function to pass through to template
  - test
* refactor function to include logic and dynamic variables
  - test


# HEROKU deployment

* add a production key to `knexfile.js`
  - set connection: `process.env.DATABASE_URL`
  - everything else should be the same as development
* set a ternary in `db/knex.js`
  - `const env = process.env.NODE_ENV || 'development';`
* if you've done user authorization/authentication/registration... add this to your server file (app.js, server.js, etc.): app.enable('trust proxy');
if you don't add it, heroku won't let you sign in or register for a new acct on your site

$ heroku create [app name]
$ heroku apps:rename [new name] *to rename:*
$ git commit -m 'prep for deployment' *check git status and commit any changes*
$ git push heroku master

$ heroku open
$ heroku logs *for finding errors*
$ heroku config
$ heroku addons:create heroku-postgresql
  *creates a postgres server on heroku*
$ heroku run bash
  *opens up a console on heroku server for your app*
* run migrations and seed your db as you would locally

------ When you need to push up new changes -------- 
  git push heroku master
  heroku run bash
    *** run migrations and seeds
  heroku config:set SESSION_SECRET=6452aa4b0fd8c6afa8f9f9a5cb67803eefb58326ebe5cf9cdd67911bcc7731b4410a1a13a1e03f0a28cf6c289acb703719be8284da21fece658f5aceb4ab61d8

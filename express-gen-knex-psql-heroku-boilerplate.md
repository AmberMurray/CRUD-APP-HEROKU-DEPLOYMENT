*** from project root directory ****

# express and knex

$ express --hbs --git
$ npm install *installs boilerplate dependencies*
$ npm install --save pg knex method-override
$ npm install --save-dev nodemon  
  * add nodemon start script in package.json
$ git init
$ touch knexfile.js  
  * setup knexfile.js config
  ```
  const path = require('path');

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
  };
  ```
$ mkdir db
$ touch db/knex.js
  * setup knex.js config
  ```
  const env = process.env.NODE_ENV || 'development';
  const config = require('../knexfile.js')[env];
  const knex = require('knex')(config);


  module.exports = knex;
  ```
  * add knex start script in package.json
$ createdb [database name]
$ psql [database name]
  * in new shell tab to confirm database creation


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

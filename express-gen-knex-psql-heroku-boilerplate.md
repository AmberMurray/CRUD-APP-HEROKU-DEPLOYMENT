## Start in project root directory

# Without Express Generator:
- `npm init -y`
- `npm install --save express body-parser pg knex`
- `npm install --save-dev morgan nodemon`
- `touch app.js knexfile.js .gitignore`
- Add node_modules/ to the .gitignore
- Add connection information to the knexfile.js
- Require necessary packages into app.js and write code to get your server started

# With Express Generator:
- `npm install -g express-generator` (this is a global install)
- `express --hbs --git`
- `npm install` (installs boilerplate dependencies)
- `git init` (establish your repo... make sure to do it on github, as well)
- `git commit -m "initial commit"`
- make sure node_modules/ and DS Store files are added to .gitignore (DS Store is important if it's a group project)
- Create knexfile.js: `touch knexfile.js`
- Set up knexfile.js config
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
- package.json add: 
  * dev script with nodemon `"dev": "nodemon -e js,hbs ./bin/www"` (gets nodemon to watch for changes in partials as well as js files)
  * knex script for knex
  * start script (with or without nodemon, this step is necessary for heroku deployment)  
  
- In app.js make sure you have the following: 
```
var express = require('express')
var path = require('path')
var favicon = require('serve-favicon')
var logger = require('morgan')
var bodyParser = require('body-parser')
var methodOverride = require('method-override')
var hbs = require('hbs') (or some kind of server side templat-er)

var app = express()

// view engine setup
app.set('views', path.join(__dirname, 'views'))
app.set('view engine', 'hbs')
// this tells hbs where to find your partials
hbs.registerPartials(__dirname + '/views/partials')

app.use(logger('dev'))
app.use(methodOverride('_method'))
app.use(bodyParser.json())
app.use(bodyParser.urlencoded({ extended: false }))
app.use(express.static(path.join(__dirname, 'public')))

```  
  
# Database
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

# To make changes to tables
- `npm run knex migrate:rollback`
  * run this command as many times as needed to go back to where things were correct/good
- Once things are good: `npm run knex migrate:latest`

# To make changes to seeds
- Make whatever changes you want in the seed file(s) & then run: `npm run knex seed:run`


# Setting up views
- Make the views files: layout.hbs, index.hbs, show.hbs, edit.hbs, new.hbs
- layout.hbs:  
  * Add bootstrap (or the framework of your choice): 
    * CDN: 
      * `<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css" integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u" crossorigin="anonymous">`
  * List your css stylesheet after the CDN link so you can override bootstrap
    * `<link rel='stylesheet' href='/css/main.css' />` (no need to list public folder)
- Use {{things in here}} to dynamically generate data

# Setting up the routes
- Set route with a method (get, put, post, delete) and attach a named function
- Code function to return a simple message
  * test
- Render the route (pass in a template and object)
- OR redirect (pass in the path for the page you want to show)
- Hard-code variables in function to pass through to template
  - test
- Refactor function to include logic and dynamic variables
  - test
  
    |English |Type | URL Path |
    |---|---|---|
    |Index |get all | get/ |
    |Index (all resources) |get all by resource | get/resources |
    |Show (one resource) |get one resource | get/resources/:id |
    |Update |update one entire resource | put/resources/:id |
    |Updage |update part of one resource | patch/resources/:id |
    |Create |create a new resource | post/resources |
    |Destroy |delete a resource | delete/resources/:id |
    | | |
    |Create |get the form to add new | get/resources/new |
    |Update |get the from to edit | get/resources/:id/edit |  
    

# HEROKU deployment

- If you haven't already:
  * add a production key to knexfile.js
    - set connection: `process.env.DATABASE_URL`
  * everything else should be the same as development
  * set a ternary in db/knex.js
    - `const env = process.env.NODE_ENV || 'development';`
    
- If you've done user authorization/authentication/registration:
  * add this to your server file (app.js, server.js, etc.): `app.enable('trust proxy')`
    * if you don't add it, heroku won't let you sign in or register for a new acct on your site (your app won't work)
  * `heroku config:set`
    - `SESSION_SECRET=[your session secrect code here from your .env file]`

- From the command line:
  * git status, git add -A, git commit, git push origin master
  * `heroku create [app name]` (you can also not name it and let Heroku give you a wacky name)
    * `heroku apps:rename [new name]` (to rename)
  * `git commit -m 'prep for deployment'`
  * `git push heroku master`
  * `heroku addons:create heroku-postgresql`
    - creates a postgres server on heroku (needed if you have a db to upload)
  * `heroku run bash`
    - opens up a console on heroku server for your app
      - run migrations and seed your db as you would locally
      - type exit to get out of heroku console
  * you can also skip opening up bash and just do this:
    - `heroku run npm run knex migrate:latest`
    - `heroku run npm run knex seed:run`
  * `heroku config` (to check configuration)
  * `heroku open`
  * `heroku logs -t` (for finding errors)

- When you need to push up new changes:
  * `git push heroku master`
  * `heroku run bash`
    - run migrations and seeds so changes are reflected
    
- NOTE: heroku, by default, looks for a favicon in your code. 
  If you don't have one, on inspect, you may see a 404 error, which you can ignore.    

# GOOD LUCK - HAPPY CODING!


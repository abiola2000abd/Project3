# PROJECT 3: MERN STACK IMPLEMENTATION
## SIMPLE TO-DO APPLICATION ON MERN WEB STACK
### STEP 1 – BACKEND CONFIGURATION

### Updating the Ubuntu Server

`sudo apt update`
![update](./images/apt-update.png)

![update1](./images/apt-update1.png)


### Upgrade Ubuntu Server after downloading Updates

`sudo apt upgrade`
![upgrade](./images/upgrade.png)


Upgrade completed

![comp](./images/upgrade-comp.png)

Getting the location of Node.js from UBUNTU repositories.

`curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -`

![reposit](./images/repo.png)

![reposit1](./images/repo1.png)


Installing Node.js on the Ubuntu Server
Using this command

`sudo apt-get install -y nodejs`

![install-nojs](./images/node-install.png)

>Note: The command above installs both nodejs and npm. NPM is a package manager for Node like apt for Ubuntu, it is used to install Node modules & packages and to manage dependency conflicts.

Verifying the node version installed with the command below

`node -v `
![verify-node](./images/nod-v.png)

Verifying the npm version  installed with the command below

`npm -v `
![verify-npm](./images/npm-v.png)

## Application Code Setup
We need to Create a new directory for your To-Do project:

`mkdir Todo`
![new-dir](./images/mkdir.png)

Verifying that the Todo directory is created with `ls` command

`ls`
![veri-todo](./images/veri.png)

Run the command below to verify that the Todo directory is created with ls command

`ls`

Now we change our current directory to the newly created one:

`cd Todo`

Next,  we use the command npm init to initialise our project, so that a new file named package.json will be created. This file will normally contain information about our application and the dependencies that it needs to run. We follow the prompts after running the command. We can press Enter several times to accept default values, then accept to write out the package.json file by typing yes.

`npm init`

![npm-int](./images/todo.png)

We run the command ls to confirm that we have package.json file created.

`ls`
![veri](./images/json.png)


## INSTALLING EXPRESSJS
 Express is a Framework for Node.js.

 We will install it using npm:

 `npm install express`

 ![express](./images/express.png)

 We need to create a file index.js inside our Todo directory and verify that it was created sucessfully.

  ![crt](./images/index-creat.png)

 We also need to Install  dotenv module

 `npm install dotenv`

 ![env](./images/env.png)

 We need to edit the index.js file. 
 
 `vim index.js`

 Copy and Paste the below code into the index.js file and save.


 ```
 {
const express = require('express');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use((req, res, next) => {
res.send('Welcome to Express');
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
 }
```

The code is copied and pasted.

 ![vim](./images/paste.png)

Now it is time to start our server to see if it works.

![running](./images/server-running.png)

Adding port 5000. We created an inbound rule to open TCP port 5000 on our EC2 Instance.

![port5000](./images/port5000add.png)

Accessing our Server's Public IP

http://13.40.138.244:5000
![acc500](./images/webacc.png)

## Routes
There are three actions that our To-Do application needs to be able to do:

1. Create a new task
2. Display list of all tasks
3. Delete a completed task

Each task will be associated with some particular endpoint and will use different standard HTTP request methods: POST, GET, DELETE.

For each task, we need to create routes that will define various endpoints that the To-do app will depend on. So let us create a folder routes

`mkdir routes`

Change directory to routes folder.

`cd routes`

Now, create a new file called api.js inside routes directory

`touch api.js`

![all](./images/all.png)

We need to edit our api.js file. Copy and paste the below code in to the api.js file and save.


```
{
const express = require ('express');
const router = express.Router();

router.get('/todos', (req, res, next) => {

});

router.post('/todos', (req, res, next) => {

});

router.delete('/todos/:id', (req, res, next) => {

})

module.exports = router;
}
```
To edit the api.js file

`vim api.js` 

![cmdvim](./images/vim-api-edit.png) 


## MODELS

A Model is at the heart of JavaScript based applications, and it is what make it interactive.

To create a Schema and a model ,We need to install mongoose which is a Node.js package that makes working with mongodb easier.

This is installed into the Todo directory.

`Todo> npm install mongoose`

![mongoos](./images/mongoos-install.png)

Create a new folder models:

`Todo> mkdir models`

![modelfolder](./images/model-folder.png)

Changing directory into the new created 'model' folder

`Todo> cd models`

Inside the model folder ,create a file and name it todo.js

`touch todo.js`

![todo.js](./images/todo.js.png)

We will edit the todo.js file created.

`vim todo.js`

 Copy and paste the code below in the file
```
{
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

//create schema for todo
const TodoSchema = new Schema({
action: {
type: String,
required: [true, 'The todo text field is required']
}
})

//create model for todo
const Todo = mongoose.model('todo', TodoSchema);

module.exports = Todo;
}
```
![vim-todo.js](./images/vim-todo.png)


We need to update our routes from the file api.js in the 'routes' directory to make use of the new model.

We need to open api.js with vim.

![cd-api](./images/vimapi.png)

Delete the code inside and paste the below code into it then save and exit

{
```    
const express = require ('express');
const router = express.Router();
const Todo = require('../models/todo');

router.get('/todos', (req, res, next) => {

//this will return all the data, exposing only the id and action field to the client
Todo.find({}, 'action')
.then(data => res.json(data))
.catch(next)
});

router.post('/todos', (req, res, next) => {
if(req.body.action){
Todo.create(req.body)
.then(data => res.json(data))
.catch(next)
}else {
res.json({
error: "The input field is empty"
})
}
});

router.delete('/todos/:id', (req, res, next) => {
Todo.findOneAndDelete({"_id": req.params.id})
.then(data => res.json(data))
.catch(next)
})

module.exports = router;

}
```
Code copy and Pasted.
![api.js](./images/vim-api-edit.png)



### MONGODB DATABASE

After Signing up for free acount on cloud.mongodb and setting up.

Using the free service 
![free](./images/select-plan.png)

Using AWS option for our project

![plan](./images/select-aws-.png)
Setting up our database connection user name and password.

![image1](./images/cloud-database-login.png)

Databass Access : Setting the ip access , we choose allow from anywhere for testing purpose.

![anywhere](./images/database-acess.png)

We need to create our database name also.

![lo](./images/db-collections.png)

![name](./images/creat-db-name.png)

![clust](./images/db-name-created.png)
Copying the connection string needed for the .env file to be able to connect our project to the database.

![strin](./images/connecting-to-db.png)

In the index.js file, we specified process.env to access environment variable, but we have not yet created this file. So we need to do that now.

The file will be created in the Todo directory and named .env

We need to edit the .env file also.

`touch .env`

`vi .env`

![cret.env](./images/vi-.env.png)

We then add the connection string we copied from our database into it.

`mongodb+srv://abiola:d3v0ps2022@cluster0.ipmteuz.mongodb.net/abiola1?retryWrites=true&w=majority` 

Save and exit

![connect-strin](./images/env-login-details-pwd.png)

Updating our index.js file to reflex the use of .env so that Node.js can connect to the database.
using vim to edit.
We delete the content of the file and update it with the following code.

`vim index.js`

Delete the content in the file ,Copy and paste the code below into the index.js file and save.
```
{
    const express = require('express');
const bodyParser = require('body-parser');
const mongoose = require('mongoose');
const routes = require('./routes/api');
const path = require('path');
require('dotenv').config();

const app = express();

const port = process.env.PORT || 5000;

//connect to the database
mongoose.connect(process.env.DB, { useNewUrlParser: true, useUnifiedTopology: true })
.then(() => console.log(`Database connected successfully`))
.catch(err => console.log(err));

//since mongoose promise is depreciated, we overide it with node's promise
mongoose.Promise = global.Promise;

app.use((req, res, next) => {
res.header("Access-Control-Allow-Origin", "\*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
next();
});

app.use(bodyParser.json());

app.use('/api', routes);

app.use((err, req, res, next) => {
console.log(err);
next();
});

app.listen(port, () => {
console.log(`Server running on port ${port}`)
});
}
```
After saving , exit.
![index-edit](./images/new-index.js-content.png)


We Start our server using the command.

`node index.js`

![ser-run](./images/server-running.png)

Database connected successfully

Testing backend code without Frontend using RESTful API
we need a ReactJS code to achieve that.But during development, we will need a way to test our code using RESTfulL API. Therefore, we will need to make use of some API development client to test our code.

In this project, we will use Postman to test our API

We will create a POST request to the API ,using the public ip address.

http://18.168.50.95:5000/api/todos

we set the header key Content-Type as application/json

![post-header](./images/postman-post-header.png)

For the body , choosing the raw option with json format , we enter some data into the body and Post.

{
    "action":"Finished project 1 and 2"
}

![post-body](./images/postman-post-sent.png)

Using the GET to retrieve data sent .

![get](./images/postman-get-outpout.png)

Also using the DELETE to delete entries. To delete any entry , you will need to use the the _id to delete the item.

![delete](./images/postman-delete-order.png)


# STEP 2 - FRONTEND CREATION
 Now that we are done with the functionallity we want from our backend and API.It is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.

In the same root directory as our backend code, which is the Todo directory, we run:

 `npx create-react-app client`

 ![npx](./images/react-install-complete.png)

This will create a new folder in your Todo directory called client, where we will add all the react code.

Running a React App
Before testing the react app, there are some dependencies that need to be installed.

Install concurrently. It is used to run more than one command simultaneously from the same terminal window.

`npm install concurrently --save-dev`

Install nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes.

`npm install nodemon --save-dev`

![con](./images/concurrently-nodeman-install.png)

In Todo folder open the package.json file. Change the highlighted part of the below screenshot and replace with the code below.

"scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
![script](./images/script-replace.png)

Code Replaced 
![script1](./images/script-replaced-edit.png)

We need to configure Proxy in package.json file

1. Change directory back to 'client'

     `cd client`

2. Open the package.jsaon file

      `vi package.json`

![json](./images/client-package-json-vi.png)

 3. Add the key value pair inthe package.json file 
 "proxy": "http://localhost:5000".

 ![5000](./images/localhost5000-added.png)

 The whole purpose of adding the proxy configuration in number 3 above is to make it possible to access the application directly from the browser by simply calling the server url like http://localhost:5000 rather than always including the entire path like http://localhost:5000/api/todos

Now, we change directory back to the Todo directory, and simply do:

`npm run dev`

Our app should open and start running on localhost:3000
![run dev](./images/npm-run-dev-succefful.png)

Important note: In order to be able to access the application from the Internet we have to open TCP port 3000 on EC2 by adding a new Security Group rule. 
![ec2](./images/port-3000-added.png)

### Creating our react Components .
One of the advantages of react is that it makes use of components, which are reusable and also makes code modular. For our Todo app, there will be two stateful components and one stateless component.
From our Todo directory run

`cd client`

move to the src directory

`cd src`

Inside our src folder create another folder called components

`mkdir components`

Move into the components directory with

`cd components`

Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.

![inp](./images/input-todo-list.png)

touch Input.js ListTodo.js Todo.js

Open Input.js file

![input](./images/input-vi.png)

`vi Input.js`

Copy and Paste the code below into the `Input.js` file
```
{
    import React, { Component } from 'react';
import axios from 'axios';

class Input extends Component {

state = {
action: ""
}

addTodo = () => {
const task = {action: this.state.action}

    if(task.action && task.action.length > 0){
      axios.post('/api/todos', task)
        .then(res => {
          if(res.data){
            this.props.getTodos();
            this.setState({action: ""})
          }
        })
        .catch(err => console.log(err))
    }else {
      console.log('input field required')
    }

}

handleChange = (e) => {
this.setState({
action: e.target.value
})
}

render() {
let { action } = this.state;
return (
<div>
<input type="text" onChange={this.handleChange} value={action} />
<button onClick={this.addTodo}>add todo</button>
</div>
)
}
}

export default Input
}
```
Copied and Pasted.

![cp](./images/input-pasted.png)

To make use of Axios, which is a Promise based HTTP client for the browser and node.js, We need to cd into our client directory from our terminal and run yarn add axios or npm install axios.

Move to the src folder

cd ..
Move to clients folder

cd ..

Install Axios

`npm install axios`

![axios](./images/axios-install.png)

Go to 'components' directory

`cd src/components`

We open our ListTodo.js file and edit it.

`vi ListTodo.js`

copy and paste the below code into the LilstTodo.js file
```
{
    import React from 'react';

const ListTodo = ({ todos, deleteTodo }) => {

return (
<ul>
{
todos &&
todos.length > 0 ?
(
todos.map(todo => {
return (
<li key={todo._id} onClick={() => deleteTodo(todo._id)}>{todo.action}</li>
)
})
)
:
(
<li>No todo(s) left</li>
)
}
</ul>
)
}

export default ListTodo
}
```
![list](./images/ListTodo-edited.png)

Then in our Todo.js file we write the following code

```{
import React, {Component} from 'react';
import axios from 'axios';

import Input from './Input';
import ListTodo from './ListTodo';

class Todo extends Component {

state = {
todos: []
}

componentDidMount(){
this.getTodos();
}

getTodos = () => {
axios.get('/api/todos')
.then(res => {
if(res.data){
this.setState({
todos: res.data
})
}
})
.catch(err => console.log(err))
}

deleteTodo = (id) => {

    axios.delete(`/api/todos/${id}`)
      .then(res => {
        if(res.data){
          this.getTodos()
        }
      })
      .catch(err => console.log(err))

}

render() {
let { todos } = this.state;

    return(
      <div>
        <h1>My Todo(s)</h1>
        <Input getTodos={this.getTodos}/>
        <ListTodo todos={todos} deleteTodo={this.deleteTodo}/>
      </div>
    )

}
}

export default Todo;
}
```
![todo](./images/Todo-js-edited.png)

We need to make little adjustment to our react code. Delete the logo and adjust our App.js to look like this.

Move to the src folder

cd ..
Make sure that you are in the src folder and run

`vi App.js`

Copy and paste the code below into it

```
{
import React from 'react';

import Todo from './components/Todo';
import './App.css';

const App = () => {
return (
<div className="App">
<Todo />
</div>
);
}

export default App;
}
```
![app](./images/replace-app.js-src.png)

After pasting, exit the editor.

In the src directory open the App.css

`vi App.css`

Then paste the following code into App.css:

```
{
.App {
text-align: center;
font-size: calc(10px + 2vmin);
width: 60%;
margin-left: auto;
margin-right: auto;
}

input {
height: 40px;
width: 50%;
border: none;
border-bottom: 2px #101113 solid;
background: none;
font-size: 1.5rem;
color: #787a80;
}

input:focus {
outline: none;
}

button {
width: 25%;
height: 45px;
border: none;
margin-left: 10px;
font-size: 25px;
background: #101113;
border-radius: 5px;
color: #787a80;
cursor: pointer;
}

button:focus {
outline: none;
}

ul {
list-style: none;
text-align: left;
padding: 15px;
background: #171a1f;
border-radius: 5px;
}

li {
padding: 15px;
font-size: 1.5rem;
margin-bottom: 15px;
background: #282c34;
border-radius: 5px;
overflow-wrap: break-word;
cursor: pointer;
}

@media only screen and (min-width: 300px) {
.App {
width: 80%;
}

input {
width: 100%
}

button {
width: 100%;
margin-top: 15px;
margin-left: 0;
}
}

@media only screen and (min-width: 640px) {
.App {
width: 60%;
}

input {
width: 50%;
}

button {
width: 30%;
margin-left: 10px;
margin-top: 0;
}
}
}
```
Exit

![app.css](./images/vi-App-css-edit.png
)
In the src directory open the index.css

`vim index.css`

Copy and paste the code below:

```{
body {
margin: 0;
padding: 0;
font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
"Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
sans-serif;
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
box-sizing: border-box;
background-color: #282c34;
color: #787a80;
}

code {
font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
monospace;
}
}
```
![index.css](./images/vim-index-css-edit.png)
Go to the Todo directory

`cd ../..`

When you are in the Todo directory run:

`npm run dev`

![ser](./images/npm-run-dev-succefful.png)

Assuming no errors when saving all these files, our To-Do app should be ready and fully functional with the functionality discussed earlier: creating a task, deleting a task and viewing all your tasks.

![output](./images/final-output.png)
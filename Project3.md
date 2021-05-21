![Screenshot (53)](https://user-images.githubusercontent.com/83962622/119127432-3aa2b080-ba2c-11eb-84bb-d289f31532ad.png)
# Simple To-Do application on MERN Web Stack
step 0 - I created an instance in AWS.amazon.com following the below steps:
--chose instance type t2.micro
---configured instance
--connected the instance to an ssh session using mobaXterm

Step 1 - Backend configuration.
--updated the ubuntu server using (sudo apt update)
--Upgrade ubuntu using(sudo apt upgrade)
---installing node.js on the server with the command (sudo apt-get install -y nodejs)

Application Code Setup
--Installed ExpressJS with (npm install express)
--created an index.js file
--installed dotenv module
--inputed the below code in the index.js 
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

----created an inbound rule to open TCP port 5000.

--Routes
In Routes directory, i opened api.js with vim api.js, and deleted the code inside with :%d command and inputed the below code into it then save and exit
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

----created a mongodb 
As ilustrated in the picture below:
![Screenshot (53)](https://user-images.githubusercontent.com/83962622/119127463-455d4580-ba2c-11eb-8543-4c2e3c04fa66.png)
![Screenshot (54)](https://user-images.githubusercontent.com/83962622/119127485-4bebbd00-ba2c-11eb-81d0-769cabf7da36.png)
![Screenshot (55)](https://user-images.githubusercontent.com/83962622/119127499-4ee6ad80-ba2c-11eb-96d8-39daf9591cbd.png)

-----i added the connection string to access the database in it, with the below connection string:
DB = 'mongodb+srv://<username>:<password>@<network-address>/<dbname>?retryWrites=true&w=majority'
 
---Testing Backend Code without Frontend using RESTful API
  I installed Postman, and set header key Content-Type as application/json
  ![Screenshot (56)](https://user-images.githubusercontent.com/83962622/119130222-d41f9180-ba2f-11eb-8067-8863771fadf2.png)
![Screenshot (46)](https://user-images.githubusercontent.com/83962622/119130473-34163800-ba30-11eb-98d9-5d5186c18b3d.png)
![Screenshot (45)](https://user-images.githubusercontent.com/83962622/119130532-42645400-ba30-11eb-9913-7de235096ad0.png)

  -----Frontend creation
  Since we are done with the functionality we want from our backend and API, it is time to create a user interface for a Web client (browser) to interact with the application via API. To start out with the frontend of the To-do app, we will use the create-react-app command to scaffold our app.
  ----Installed concurrently. It is used to run more than one command simultaneously from the same terminal window(npm install concurrently --save-dev)
  ----Installed nodemon. It is used to run and monitor the server. If there is any change in the server code, nodemon will restart it automatically and load the new changes(npm install nodemon --save-dev)
  ----In Todo folder open the package.json file. Change the highlighted part of the below screenshot and replace with the code below
  "scripts": {
"start": "node index.js",
"start-watch": "nodemon index.js",
"dev": "concurrently \"npm run start-watch\" \"cd client && npm start\""
},
  
  ----Configured Proxy in package.json
  ----Creating your React Components
  ---cd client
---move to the src directory

---cd src
----Inside your src folder create another folder called components

----mkdir components
----Move into the components directory with

---cd components
---Inside ‘components’ directory create three files Input.js, ListTodo.js and Todo.js.

--touch Input.js ListTodo.js Todo.js
--Open Input.js file

--vi Input.js
  ---pasted the below code in input.js
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
  ----Move to the src folder

cd ..
Move to clients folder

cd ..
Install Axios

$ npm install axios
Go to ‘components’ directory

cd src/components
After that open your ListTodo.js

vi ListTodo.js
in the ListTodo.js copy and paste the following code

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
  
  -----Move to the src folder

cd ..
Make sure that you are in the src folder and run

vi App.js
Copy and paste the code below into it

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
After pasting, exit the editor.

In the src directory open the App.css

vi App.css
Then paste the following code into App.css:

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
Exit

In the src directory open the index.css

vim index.css
Copy and paste the code below:

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
Go to the Todo directory

cd ../..
When you are in the Todo directory run:

npm run dev
  
  ![Screenshot (48)](https://user-images.githubusercontent.com/83962622/119133196-89077d80-ba33-11eb-9c37-bc6f414c31ee.png)
![Screenshot (50)](https://user-images.githubusercontent.com/83962622/119133209-8b69d780-ba33-11eb-82de-bd4f800e33fb.png)
![Screenshot (51)](https://user-images.githubusercontent.com/83962622/119133225-8e64c800-ba33-11eb-898a-0e2ba7e7d263.png)
![Screenshot (52)](https://user-images.githubusercontent.com/83962622/119133232-90c72200-ba33-11eb-978e-7cc53f911fdb.png)

  
  
  




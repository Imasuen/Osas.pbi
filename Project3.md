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

---
label: Setting up your Development Environment
order: 1100
categories:
    - Tutorials
tags:
    - Developer Setup
    - Web
---
# Setting up your Dev Environment

## What you will learn
After this, you will:
- [x] Know how to connect to a Verus node locally over rpc

## Prerequisites: a locallly running Verus Node

!!! danger
This is only for illustrative purposes. Do not use this in production scenarios. 
!!!

To interact with the Verus Protocol you will need to have access to a running Verus Node. A quick way to do that is to install the Verus Desktop app on your machine, which installs the verus daemon under the covers. Once installed, add the test chain to your Wallet and let it sync. Visit the [Verus Wiki](https://wiki.verus.io/#!index.md) for how to do this. 

### Authorisation using rpcuser and rpcpassword

Locate the VRSC.conf file by opening **Show Verus data folder** under the Help menu in the Verus Desktop gui. Open the VRSC.conf file as you will need the rpcuser and rpcpassword values. 

!!!warning
Securing your Verus Node is critically important. Refer to the Verus Wiki on good practices. 
!!!

### Web App
For simplicity, we will create a simple node web app using Express that will send the `getinfo` command to the Verus Node, which will confirm we have a working Verus node able to interact with. We will use Visual Code manage this quick tutorial.  

Open Visual Studio Code and open a new folder to work in. Drop into your VS Code terminal:

```bash
mkdir verusnode-express-app
cd verusnode-express-app
node install
touch app.js
```

Now oepn index.html and copy this into it, note to replace your rpcuser and rpcpassword values taken from your locally instaleld Verus node's VRSC.conf as described above. 

```javascript
const express = require('express');
const axios = require('axios');
const bodyParser = require('body-parser');
const path = require('path');
const app = express();
const port = 3000;

app.use(bodyParser.urlencoded({ extended: false }));
app.use(bodyParser.json());

const rpcUser = '<your rpcuser value from VRSC.conf>';
const rpcPassword = '<your rpcpassword value from VRSC.conf>';
const rpcPort = 27486; // Replace with your RPC port

const rpcURL = `http://${rpcUser}:${rpcPassword}@localhost:${rpcPort}`;

app.get('/', (req, res) => {
  res.sendFile(path.join(__dirname + '/index.html'));
});

app.post('/', (req, res) => {
  const command = {
    method: req.body.command,
    params: [],
    id: req.body.command
  };

  axios.post(rpcURL, command)
    .then(response => {
      res.send(response.data.result);
    })
    .catch(error => {
      res.status(500).send(`Error: ${error}`);
    });
});

app.listen(port, () => {
  console.log(`App listening at http://localhost:${port}`);
});
```
# HTTPS localhost for Express.js

Credit to Tim Kamanin

[https://timonweb.com/posts/running-expressjs-server-over-https/](https://timonweb.com/posts/running-expressjs-server-over-https/)

The goal here is to generate self-signed certificates to your local project directory, then utilising node's native https and fs api, create a HTTPS signed express server.

## Steps

### 1. Generate a self-signed certificate

In the terminal:

```shell
openssl req -nodes -new -x509 -keyout server.key -out server.cert
```

### 2. Enable HTTPS in Express

```js
const express = require('express');
const fs = require('fs');
const https = require('https');
const cors = require('cors');
const app = express();
const port = 4069;

app.use(cors());

app.post('/gc-test', (req, res, next) => {
    console.log(req.body);
    res.status(200).send('Hello');
})

https.createServer({
    key: fs.readFileSync('server.key'),
    cert: fs.readFileSync('server.cert')
}, app)
    .listen(port, () => {
        console.log(`HTTPS-enabled app listening on port ${port}`)
    })
```
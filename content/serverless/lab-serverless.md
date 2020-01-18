# Lab: Serverless End-Point

## Aws

Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)

From the services select `Cloud9` and launch the environment previously used in the labs

Navigate to the folder with node.js end-point: `cd courseware-nodejs-container`

If the folder is not present you may need to clone the repo again:`git clone https://github.com/vkhazin/courseware-nodejs-container`

Using the terminal panel navigate to the `api` subfolder of the cloned repository: `cd ./environment/courseware-nodejs-container/api/`

Install node.js packages: `npm install`

Need to modify server.js file before deployment, modified content:

    const serverless = require('serverless-http');
    const express = require('express');
    const app = express();
    const morgan = require('morgan');

    const personRoutes = require('./routes/person');

    app.use(express.json());
    app.use(morgan('dev'));
    app.use(personRoutes);

    app.listen(process.env.PORT || 3001, () => {
        console.log("Server is running");
    })

    app.use(function(req, res, next) {
        return res.status(404).send({ error: `Route ${req.url} Not found.` });
    });
    module.exports.handler = serverless(app);

Need to add a new file for deployment:  `aws.yml` with the following content:

```
service: courseware-nodejs
provider:
  name: aws
  runtime: nodejs12.x
  stage: dev
  region: us-east-1
  memorySize: 128
functions:
  app:
    handler: server.handler
    events: 
      - http: 
          path: /
          method: ANY
          cors: true
      - http: 
          path: /{proxy+}
          method: ANY
          cors: true
```

Add serverless package: `npm i --save serverless-http`

Install serverless cli: `npm install -g serverless`






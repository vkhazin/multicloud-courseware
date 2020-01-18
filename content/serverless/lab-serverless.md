# Lab: Serverless End-Point

## Aws

1. Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)
2. From the services select `Cloud9` and launch the environment previously used in the labs
3. Navigate to the folder with node.js end-point: `cd courseware-nodejs-container`
4. If the folder is not present you may need to clone the repo again:`git clone https://github.com/vkhazin/courseware-nodejs-container`
5. Using the terminal panel navigate to the `api` subfolder of the cloned repository: `cd ./environment/courseware-nodejs-container/api/`
6. Install node.js packages: `npm install`
7. Need to modify server.js file before deployment, modified content:
8.     const serverless = require('serverless-http');
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
9. Need to add a new file for deployment:  `aws.yml` with the following content:
10. ```
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
11. Add serverless package: `npm i --save serverless-http`
12. Install serverless cli: `npm install -g serverless`
13. Deploy the function with API Gateway http end-point: `serverless remove --config ./aws.yml `
14. After deployment is complete a URL will be listed e.g. `https://hsfeejxnpj.execute-api.us-east-1.amazonaws.com/dev/`
15. Copy the URL to open in browser and don't forget to add the parameter: `/?name=John`




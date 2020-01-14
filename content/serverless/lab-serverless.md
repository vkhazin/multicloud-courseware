# Lab: Serverless End-Point

## AWS

1. We will use serverless npm pkg to deploy the nodejs app to Azure Lambda
2. Download the repo from [github](https://github.com/vkhazin/courseware-nodejs-container) and open the `cmd` in `/api` folder 
3. Modify server.js file to make it compatible for deployment
4. Server.js Before
5.  ```
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
    module.exports = app
    ```
5. Server.js After Modification 
6.  ```
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
    ```
7. Create a new file under same directory and name it 'serverless.yml' and paste this code
8.  ```
    service: sample-app-11
    provider:
      name: aws
      runtime: nodejs12.x
      stage: dev
      region: us-east-1
      memorySize: 128
    functions:
      app:
        handler: app/app.handler
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
9. Here, we are setting the service name 'stack name' which will be created in AWS, nodejs runtime, region, and routing.
10. Create a IAM user in AWS and save the key and secret
11. Now, run this script in cmd under '/api' directory
12. ```
    npm i serverless-http
    
    npm install -g serverless
    
    serverless ?version
    
    serverless config credentials --provider aws --key AWS_IAM_ACCESS_KEY ?secret AWS_IAM_SECRET_KEY
    
    serverless deploy
    ```
13. This will output a success message with endpoints to test the api
14. ```
    api keys:
      None
    endpoints:
      ANY - https://utcm2rshle.execute-api.us-east-1.amazonaws.com/dev/
      ANY - https://utcm2rshle.execute-api.us-east-1.amazonaws.com/dev/{proxy+}
    functions:
      app: sample-app-11-dev-app
    layers:
      None
    ```

## Azure 


## GCP

1. Open [GCP Console](https://console.cloud.google.com/)
2. Run this script to Deploy the Node Js API
3.  ```
    # Clone the Repository
    git clone https://github.com/vkhazin/courseware-nodejs-container.git
    
    # Change Directory to where we have server.js file
    cd courseware-nodejs-container/api
    ```
4. Modify server.js and change the module export syntax
5. ```
    # Before Modification
    module.exports = app
    
    # After Modification
    module.exports = {
        app
    };
    ```
6. Now, Deploy the API with this one command
    ```
    gcloud functions deploy <Function Name> --runtime nodejs8 --trigger-http --entry-point app
    ```
7. It will output with wait message like this
8. `Deploying function (may take a while - up to 2 minutes)`
9. Now, Navigate to the Cloud Functions and you must find your deployed app with green tick mark
10. To test the API with Cloud Function navigate to this url
11.  ```
    https://<region-name>-<projectId>.cloudfunctions.net/<functionAppName>
    
    # ex: https://us-central1-node-js-cloud-function.cloudfunctions.net/nodejsFuncs
    ```

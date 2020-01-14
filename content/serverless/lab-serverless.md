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
10. Modify server.js and change the module export syntax
11. ```
    # Before Modification
    module.exports = app
    
    # After Modification
    module.exports = {
        app
    };
    ```
12. Create a new file in the root directory where DockerFile is placed and name it `app.yaml` with following code:
13. ```
    runtime: custom
    env: flex
    ```
14. Now just deploy this container to App Engine
15. `gcloud app deploy`
16. This will start the deployment. Once it completes with a success message, to test it run this command
17. `gcloud app browse`
18. This will responf with url for the deployed API you can copy/paste in browser to access the endpoint
19. ```
    Did not detect your browser. Go to this link to view your app:
    https://nodejsocontainer.appspot.com
    ```

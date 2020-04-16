# Lab: Serverless End-Point

## Aws

1. Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)
2. From the services select `Cloud9` and launch the environment previously used in the labs
3. Navigate to the folder with node.js end-point: `cd courseware-nodejs-container`
4. If the folder is not present you may need to clone the repo again:`git clone https://github.com/vkhazin/courseware-nodejs-container`
5. Using the terminal panel navigate to the `api` subfolder of the cloned repository: `cd ./environment/courseware-nodejs-container/api/`
6. Install node.js packages: `npm install`
7. Need to modify server.js file before deployment, modified content:
8. ```text
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
       return res.status(404).send({ error: Route ${req.url} Not found. });
   });
   module.exports.handler = serverless(app);
   ```
9. We will use serverless framework to deploy function and to configure API Gateway
10. Will need to add a new file for serverless framework deployment:  `aws.yml` with the following content:
11. ```text
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
12. Add serverless framework package: `npm i --save serverless-http`
13. Install serverless framework cli: `npm install -g serverless`
14. Deploy the function with API Gateway http end-point: `serverless deploy --config ./aws.yml`
15. After the deployment is complete a URL will be listed e.g. `https://hsfeejxnpj.execute-api.us-east-1.amazonaws.com/dev/`
16. Copy the URL to open in browser and don't forget to add parameter: `/?name=John`

## Azure

1. Open a web browser to [https://shell.azure.com/](https://shell.azure.com/)
2. After the shell has been loaded, select `bash` shell
3. Select `Open editor` icon from the toolbar
4. Clone the repository with node.js end-point, if not present:
5. `git clone https://github.com/vkhazin/courseware-nodejs-container`
6. Change directory: `cd ~/courseware-nodejs-container/api`
7. Install npm packages: `npm install`
8. Run in the terminal panel to create a new resource group and storage account for our function:
9. ```text
   az group create \
     --name courseware-functionapp \
     --location centralus

   az storage account create \
     --name coursewarefunctionapp \
     --location centralus \
     --resource-group courseware-functionapp \
     --sku Standard_LRS
   ```
10. Authenticate to az cli: `az login`
11. Create a function:
12. ```text
    az functionapp create \
      --resource-group courseware-functionapp \
      --name your-name-courseware-nodejs \
      --consumption-plan-location centralus \
      --storage-account coursewarefunctionapp \
      --runtime node
    ```
13. Azure requires to add `~/courseware-nodejs-container/host.json` file to the same folder where the function code resides:
14. ```text
    {
        "version": "2.0",
        "extensionBundle": {
            "id": "Microsoft.Azure.Functions.ExtensionBundle",
            "version": "[1.*, 2.0.0)"
        }
    }
    ```
15. Also a `~/courseware-nodejs-container/api/function.json` file
16. ```text
    {
      "bindings": [
        {
          "authLevel": "Anonymous",
          "type": "httpTrigger",
          "direction": "in",
          "name": "req",
          "methods": [
            "get"
          ]
        },
        {
          "type": "http",
          "direction": "out",
          "name": "res"
        }
      ]
    }
    ```
17. Additionally, Azure Function App does not support Express.js, therefore, we add a new file to process requests: `index.js`:
18. ```text
    module.exports = async function (context, req) {
        context.log('JavaScript HTTP trigger function processed a request.');

        if (req.query.name) {
            context.res = {
                body: "Hello " + (req.query.name) + "!"
            };
        }
        else {
            context.res = {
                status: 400,
                body: "Invalid parameters!"
            };
        }
    };
    ```
19. Deploy the function code:
20. ```text
    func azure functionapp publish courseware-nodejs --node
    ```
21. After the deployment has been completed, the function endpoint will be listed e.g.:
22. ```text
    Invoke url: https://your-name-courseware-nodejs.azurewebsites.net/api/api
    ```
23. Double `api` in the URL is for the reasons that our folder called `api` and Azure Function Apps adds `api` as well :-\)
24. Selecting the URL and don't forget to add the `?name=John` parameter
25. The output will be slightly different as we are now using a completely different event handler compared to AWS lambda with Express.js
26. Realistically you will need to refactor the code keeping all the application logic separate from the handler as it is not possible \(yet\) to reuse the handlers across multiple cloud providers
27. There is a [serverless](https://serverless.com/framework/docs/providers/azure/guide/quick-start/) implementation for Azure Function Apps, does not feel ready to go mainstream

## GCP

1. Navigate to [https://console.cloud.google.com/](https://console.cloud.google.com/)
2. Select `Activate Cloud Shell` icon and select editor icon to open a new browser tab/window
3. We should have a folder `/courseware-nodejs-container` already
4. If it is not there, clone the repo: `git clone https://github.com/vkhazin/courseware-nodejs-container`
5. Change directory: `cd ./courseware-nodejs-container/api`
6. Modify `server.js`:
7. ```text
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
       return res.status(404).send({ error: "Route ${req.url} Not found." });
   });
   // module.exports = app
   module.exports = {
        app
    };
   ```
8. Install npm packages: `npm install`
9. Navigate to `Cloud Functions`
10. Select existing or create a new project using the Google Cloud Console to enable the Cloud Function API
11. Get the list of GCP projects: `gcloud projects list` and copy `PROJECT_ID` for the projects where cloud function API has been enabled
12. Replace the project id in the following command and execute using the terminal panel:
13. ```text
    gcloud functions deploy courseware-nodejs \
        --project {project_id} \
        --runtime nodejs8 \
        --trigger-http \
        --entry-point app \
        --allow-unauthenticated
    ```
14. After the deployment is complete you will find HTTP trigger URL listed to test in the browser or with a curl command
15. Don't forget to add a query string parameter: `/?name=John`


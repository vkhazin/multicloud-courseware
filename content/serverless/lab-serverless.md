# Lab: Serverless End-Point

## Aws

1. Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)
2. From the services select `Cloud9` and launch the environment previously used in the labs
3. Navigate to the folder with node.js end-point: `cd courseware-nodejs-container`
4. If the folder is not present you may need to clone the repo again:`git clone https://github.com/vkhazin/courseware-nodejs-container`
5. Using the terminal panel navigate to the `api` subfolder of the cloned repository: `cd ./environment/courseware-nodejs-container/api/`
6. Install node.js packages: `npm install`
7. Need to modify server.js file before deployment, modified content:
8. const serverless = require\('serverless-http'\);  
   const express = require\('express'\);  
   const app = express\(\);  
   const morgan = require\('morgan'\);

   const personRoutes = require\('./routes/person'\);

   app.use\(express.json\(\)\);  
   app.use\(morgan\('dev'\)\);  
   app.use\(personRoutes\);

   app.listen\(process.env.PORT \|\| 3001, \(\) =&gt; {  
       console.log\("Server is running"\);  
   }\)

   app.use\(function\(req, res, next\) {  
       return res.status\(404\).send\({ error: `Route ${req.url} Not found.` }\);  
   }\);  
   module.exports.handler = serverless\(app\);

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
13. Deploy the function with API Gateway http end-point: `serverless remove --config ./aws.yml`
14. After deployment is complete a URL will be listed e.g. `https://hsfeejxnpj.execute-api.us-east-1.amazonaws.com/dev/`
15. Copy the URL to open in browser and don't forget to add the parameter: `/?name=John`

## Azure

1. Open a web browser to [https://shell.azure.com/](https://shell.azure.com/) and login with Microsoft Credentials with access to Azure subscription
2. After the shell has been loaded, select `bash` shell
3. Select `Open editor` icon from the toolbar
4. Clone the repository with node.js end-point, if not present:
5. `git clone https://github.com/vkhazin/courseware-nodejs-container`
6. Change directory: `cd ~/courseware-nodejs-container/api`
7. Install npm packages: `npm install`
8. Run in the terminal panel to create a new resource group and storage account:
9. ```
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
12. ```
    az functionapp create \
      --resource-group courseware-functionapp \
      --name courseware-nodejs \
      --consumption-plan-location centralus \
      --storage-account coursewarefunctionapp \
      --runtime node
    ```
13. Azure requires to add `~/courseware-nodejs-container/host.json` file to the same folder where the function code resides:
14. ```
    {
        "version": "2.0",
        "extensionBundle": {
            "id": "Microsoft.Azure.Functions.ExtensionBundle",
            "version": "[1.*, 2.0.0)"
        }
    }
    ```
15. Also a `~/courseware-nodejs-container/api/function.json` file
16. ```
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
18. ```
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
19. Deploy function code:
20. ```
    func azure functionapp publish courseware-nodejs --node
    ```
21. After the deployment has been completed, the function endpoint will be listed e.g.:
22. ```
    Invoke url: https://courseware-nodejs.azurewebsites.net/api/api
    ```
23. Double `api` in the URL is for the reasons that our folder called `api` and Azure Function Apps adds `api` as well :-\)
24. Selecting the URL and don't forget to add the `?name=John` parameter
25. The output will be slightly different as we are now using a completely different event handler compared to AWS lambda with Express.js
26. Realistically you will need to refactor the code keeping all the logic separate from the handler as it is not possible yet to reuse the handlers across multiple cloud providers

## GCP

1. Navigate to [https://console.cloud.google.com/](https://console.cloud.google.com/)
2. Select `Activate Cloud Shell` icon and select editor icon to open a new browser tab/window
3. We should have a folder `/courseware-nodejs-container` 
4. If it is not there, clone the repo: `git clone https://github.com/vkhazin/courseware-nodejs-container`
5. Change directory: `cd ./courseware-nodejs-container/api`
6. Modify `server.js`:
7. 
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
    // module.exports = app
    module.exports = {
         app
     };

1. Install npm packages: `npm install`
2. Get list of GCP projects: `gcloud projects list` and copy `PROJECT_ID` for the projects where cloud function API has been enabled by navigating to the `Cloud Function` on cloud console
3. Replace the project id in the following command and execute using cloud shell:
4. ```
   gcloud functions deploy courseware-nodejs \
       --project {project_id} \
       --runtime nodejs8 \
       --trigger-http \
       --entry-point app \
       --allow-unauthenticated
   ```
5. After the deployment is complete you will find HTTP trigger URL listed to test in the browser or with a curl command
6. Don't forget to add a query string parameter: `/?name=John`




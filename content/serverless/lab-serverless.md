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

Open a web browser to [https://shell.azure.com/](https://shell.azure.com/) and login with Microsoft Credentials with access to Azure subscription

After the shell has been loaded, select `bash` shell

Select `Open editor` icon from the toolbar

Clone the repository with node.js end-point, if not present:

`git clone https://github.com/vkhazin/courseware-nodejs-container`

Change directory: `cd ~/courseware-nodejs-container/api`

Install npm packages: `npm install`

Run in the terminal panel to create a new resource group and storage account:

```
az group create \
  --name courseware-functionapp \
  --location centralus

az storage account create \
  --name coursewarefunctionapp \
  --location centralus \
  --resource-group courseware-functionapp \
  --sku Standard_LRS
```

Create a function:

```
az functionapp create \
  --resource-group courseware-functionapp \
  --name courseware-nodejs \
  --consumption-plan-location centralus \
  --storage-account coursewarefunctionapp \
  --runtime node
```

Azure requires to add `host.json` file to the same folder where the function code resides:

```
{
    "version": "2.0",
    "extensionBundle": {
        "id": "Microsoft.Azure.Functions.ExtensionBundle",
        "version": "[1.*, 2.0.0)"
    }
}
```

Also a `function.json` file

```
{
  "bindings": [
    {
      "authLevel": "function",
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

Additionally, Azure Function App do not support Express.js, therefore, we add a new file to process requests: `index.js`:

```
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

Deploy function code:

```
func azure functionapp publish courseware-nodejs
```




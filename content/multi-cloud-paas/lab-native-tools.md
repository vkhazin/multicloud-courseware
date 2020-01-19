# Lab: Native PaaS Tools

## Aws

1. Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)
2. From the services select `Cloud9` and launch the environment previously used in the labs
3. Navigate to the folder with node.js end-point: `cd courseware-nodejs-container`
4. If the folder is not present you may need to clone the repo again:`git clone https://github.com/vkhazin/courseware-nodejs-container`
5. Using the terminal panel navigate to the `api` sub-folder of the cloned repository: `cd ./environment/courseware-nodejs-container/api/`
6. Install node.js packages: `npm install`
7. Create a zip file: `zip -r courseware-nodejs.zip .`
8. Right-click on the zip file  and select `Download` to copy the file to your local machine
9. Navigate to Elastic Beanstalk
10. Select `Create New Applciation`
11. For application name type: `courseware-nodejs`
12. Select `Create`
13. Select `Actions` -> `Create one now` to create a new environment
14. Select `Web server environment`
15. Type an environment name
16. Leave domain blank for an auto-generated domain
17. Select a pre-configured platform: `Node.js` 
18. Upload the zip file we have downloaded from Cloud9 environment
19. Select `Create Environment` 
20. Wait for the environment to be created and provisioned as a few things are being created behind the scene
21. While you wait you can explore options under `Learn More` to reflect on the options available in addition to running node.js
22. Eventually, you are odd to see a page with `Ok` health status and a URL to navigate to
23. Don't forget our end-point expects parameter: `/?name=John`
24. Do not delete the application as we will need it in the next lab

## Azure

1. Open a web browser to [https://shell.azure.com/](https://shell.azure.com/)
2. After the shell has been loaded
3. Select `bash` shell
4. Select `Open editor` icon from the toolbar
5. Clone the repository with node.js end-point: `git clone https://github.com/vkhazin/courseware-nodejs-container`
6. Proceed to the App Service on Azure Portal and select `Create app service`
7. Select a subscription and a resource group
8. Type in a Name e.g. `courseware-nodejs`
9. Select code and run-time `Node 10 LTS` and `Linux`
10. Select `Free F1` for sku and size
11. Select `Review + create` and then `Create`
12. After the deployment is complete, select `Go to resource`
13. Using Azure Shell change directory: `cd ./courseware-nodejs-container/api`
14. Install npm packages: `npm install`
15. Create a zip file: `zip -r courseware-nodejs.zip .`
16. In the terminal panel run the deploy command:
17. ```
    az webapp deployment source config-zip \
      --resource-group <resource-group-name> \
      --name courseware-nodejs \
      --src ./courseware-nodejs.zip
    ```
18. After the deployment is complete access the URL of the deployment
19. Don't forget the end-point requires parameter `/?name=John`

## GCP

1. Navigate to [https://console.cloud.google.com/](https://console.cloud.google.com/)
2. Create a new project to make it possible to delete the App Engine after we are done
3. Select `Activate Cloud Shell` icon and select editor icon to open a new browser tab/window
4. We should have a folder `/courseware-nodejs-container` 
5. If it is not there, clone the repo: `git clone https://github.com/vkhazin/courseware-nodejs-container`
6. Change directory: `cd ./courseware-nodejs-container/api`
7. Install npm packages: `npm install`
8. Google App Engine requires a manifest for deployment to succeed
9. Create a new file under the `api` folder: `app.yaml` with the following content:
   ```
   runtime: nodejs10
   ```
10. And deploy the application: `gcloud app deploy`
11. Select the desired deployment region and confirm the deployment
12. After the deployment is complete, type `gcloud app browse`
13. It will list a URL of app engine, navigate to the URL provided
14. Don't forget the end-point requires parameter e.g. `/?name=John`




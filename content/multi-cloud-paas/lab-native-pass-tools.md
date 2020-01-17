# Lab: Native PaaS Tools

## Aws

1. Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)
2. From the services select `Cloud9` and launch the environment previously used in the labs
3. Navigate to the folder with node.js end-point: `cd courseware-nodejs-container`
4. If the folder is not present you may need to clone the repo again:`git clone https://github.com/vkhazin/courseware-nodejs-container`
5. Using the terminal panel navigate to the `api` subfolder of the cloned repository: `cd ./environment/courseware-nodejs-container/api/`
6. Install node.js packages: `npm install`
7. Create a zip file: `zip -r courseware-nodejs.zip .`
8. Right-click on the zip file  and select `Download` to copy the file to your local machine
9. Navigate to Elastic Beanstalk in the same region as the command below using the web console
10. Select `Create New Applciation`
11. For application name type: `courseware-nodejs`
12. Select `Create`
13. Select `Actions -> Create one now` to create a new environment
14. Select `Web server environment`
15. Type an environment name
16. Leave domain blank for an auto-generated domain
17. Select a preconfigured platform: `Node.js` 
18. Upload the zip file we have downloaded from Cloud9 environment
19. Select `Create Environment` 
20. Wait for the environment to be created and provisioned as a few things are being created behind the scene
21. While you wait you can explore options under `Learn More` to reflect on the options available in addition to running node.js code on AWS PaaS Elastic Beanstalk
22. Eventually, you are odd to see a page with `Ok` health status and a URL to select for the provisioned environment
23. Don't forget our end-point expect a parameter: `/?name=John`
24. Do not delete the application as we will need it in the next lab

## Azure

1. Open a web browser to [https://shell.azure.com/](https://shell.azure.com/) and login with Microsoft Credentials with access to Azure subscription
2. After the shell has been loaded
3. Select `bash` shell
4. Select `Open editor` icon from the tool bar
5. 6. 



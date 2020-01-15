# Lab: Native PaaS Tools

## Aws

1. Open a web browser to [https://console.aws.amazon.com](https://console.aws.amazon.com)
2. From the services select `Cloud9` and launch the environment previously used in the labs
3. We need to install eb cli: `pip install awsebcli --upgrade --user`
4. Navigate to the folder with nodejs end-point: `cd courseware-nodejs-container`
5. If the folder is not present you may need to clone the repo again:`git clone https://github.com/vkhazin/courseware-nodejs-container && cd ./courseware-nodejs-container/api && npm install`
6. Right-click on the name of the `api`folder and select `Download` to get a zip file of the complete folder
7. Rename the downloaded file to have the file extension `zip`
8. Navigate to Elastic Beanstalk in the same region as the command below using web console
9. Select `Create New Applciation`
10. For application name type: `courseware-nodejs`
11. Select `Create`
12. Select `Create one now` to create a new environment
13. Select `Web server environment`
14. Select a preconfigured platform: `Node.js` 
15. Upload the zip file we have downloaded from Cloud9 environment
16. Select `Create Environment` 




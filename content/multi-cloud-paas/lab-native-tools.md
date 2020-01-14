# Lab: Native Tools

## Build and Push Docker Image to Docker Hub

1. Open a web browser to [https://labs.play-with-docker.com/](https://labs.play-with-docker.com/). You may install and use Docker Daemon on your computer to work locally.
2. Start a New Session and run this script to build and push the docker image to Docker Hub
3.  ```
    # Login to docker hub account
    docker login --username=<username> --password=<password>

    # Clone GitHub repo
    git clone https://github.com/vkhazin/courseware-nodejs-container & cd courseware-nodejs-container

    # Create docker image
    docker build -t <username>/courseware-nodejs-example .

    # Publish docker image to Docker Hub
    docker push <username>/<docker Image Name>:latest
    ```

## AWS

### Pre-requesites

1. Setup an IAM user to be used with AWS EB CLI
2. Setup EB CLI
3.  ```
    # Required permissions: "AWSElasticBeanstalkFullAccess" policy
    # Download and install Python 3.6+
    
    # Install EB CLI
    pip install awsebcli --upgrade --user

    # Clone GitHub repo
    git clone https://github.com/vkhazin/courseware-nodejs-container & cd courseware-nodejs-container

    # Setup new Elastic Beanstalk application
    # Using courseware-nodejs-app as application name
    eb init courseware-nodejs-app --platform Docker

    # Setup AWS resources necessary to run the application
    # Using courseware-nodejs-app-dev as environment name
    eb create courseware-nodejs-app-dev --elb-type application --region <region>
    ```
4. For service role: Press enter so that CLI creates one for you
    ```
    # Please wait until the following message is shown.
    # INFO: Successfully Launched Environment

    # Test your application
    eb open
    ```

## Azure

1.  Open [Azure Portal](porta.azure.com) and Open Azure Cli to run the script
2.  This script will create all the requires resources to deploy the docker image on 
3.  ```
    # Variables
    resourceGroup=courseware-nodejs-app
    location="East US"
    plan=App-Service-Plan
    dockerHubUserName="CloudClient"
    
    # Create resource group
    az group create --name $resourceGroup --location $location

    # Create app service plan
    az appservice plan create --name $plan \
                              --resource-group $resourceGroup \
                              --is-linux --sku F1

    # Create app service
    az webapp create  --name mudassir-courseware-nodejs \
                      --plan $plan \
                      --resource-group $resourceGroup \
                      --deployment-container-image-name $dockerHubUserName/<docker imag name>:latest
    ```
    
## GCP

1. Open [GCP Console](https://console.cloud.google.com/)
2. Run this sc
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
9. Now, Deploy the API with this one command
    ```
    gcloud functions deploy <Function Name> --runtime nodejs8 --trigger-http --entry-point app
    ```
4. It will output with wait message like this
5. `Deploying function (may take a while - up to 2 minutes)`
6. Now, Navigate to the Cloud Functions and you must find your deployed app with green tick mark
7. To test the API with Cloud Function navigate to this url
8.  ```
    https://<region-name>-<projectId>.cloudfunctions.net/<functionAppName>
    
    # ex: https://us-central1-node-js-cloud-function.cloudfunctions.net/nodejsFuncs
    ```

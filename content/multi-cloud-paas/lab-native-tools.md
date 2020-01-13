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

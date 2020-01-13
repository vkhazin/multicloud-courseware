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
    docker push <username>/courseware-nodejs-example:latest
    ```

## Azure

1.  ```
    resourceGroup=courseware-nodejs-app
    location="East US"
    plan=App-Service-Plan

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
                      --deployment-container-image-name adeelnayyersheikh/courseware-nodejs-container:latest
    ```

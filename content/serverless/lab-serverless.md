# Lab: Serverless End-Point

## GCP

1. Open [GCP Console](https://console.cloud.google.com/)
2. Run this script to Deploy the Node Js API
3.  ```
    # Clone the Repository
    git clone https://github.com/vkhazin/courseware-nodejs-container.git
    
    # Change Directory to where we have server.js file
    cd courseware-nodejs-container/api
    
    # Now, Deploy the API with this one command.
    gcloud functions deploy <Function Name> --runtime nodejs8 --trigger-http --entry-point app
    ```
  

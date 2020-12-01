# Lab: Kubernetes Deployment

1. Log in to Google Cloud Console
2. As a reminder GCP API's are eventually consistent, we may need to wait here and there
3. Create a new project `gcp-k8s` where to launch new GKE Cluster
4. Select the newly created project from the drop-down list and navigate to `Kubernetes Engine`and wait for the API being enabled
5. Select `Create cluster` once the API has been enabled
6. Choose the `Standard cluster` and provide a name
7. For location type select `zonal` and select the desired location
8. Select the default version of the cluster from the drop-down list
9. Select `3` for `Number of nodes`
10. Select a node size with 2 vCPU's and 4GB of memory, the name of node type will vary between regions and over time
11. Select `Create` link and wait for the cluster to provision, it may take a few minutes
12. When the cluster is provisioned and ready, select `connect` link next to the cluster name
13. Select `Run in Cloud Shell` 
14. Select the `Launch Editor` icon to switch to a full browser window with a terminal panel
15. In the terminal panel test our connectivity to the cluster: `kubectl get node`
16. The result should list nodes few minutes old since we have created the cluster
17. To deploy our first application to the cluster we will reuse the node.js end-point from the previous lab
18. The docker image has been published to the docker hub: [https://hub.docker.com/repository/docker/vkhazin/courseware-nodejs-container](https://hub.docker.com/repository/docker/vkhazin/courseware-nodejs-container)
19. Create a new folder
20. Add a new file under the root of the new folder: `deployment.yml` with the following content:
21. ```text
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: nodejs-endpoint
      name: nodejs-endpoint
      namespace: default
    spec:
      replicas: 2
      selector:
        matchLabels:
          app: nodejs-endpoint
      template:
        metadata:
          labels:
            app: nodejs-endpoint
        spec:
          containers:
          - image: vkhazin/courseware-nodejs-container
            imagePullPolicy: Always
            name: nodejs-endpoint
    ```
22. To deploy the application to Kubernetes cluster: `kubectl apply --filename deployment.yml`
23. Expected outcome: `deployment.apps/nodejs-endpoint created`
24. To list cluster assets: `kubectl get all`
25. Add a new file `service.yml` under the root of the new folder with the following content:
26. ```text
    apiVersion: "v1"
    kind: "Service"
    metadata:
      name: "nodejs-endpoint-service"
      namespace: "default"
      labels:
        app: "nodejs-endpoint"
    spec:
      ports:
      - protocol: "TCP"
        port: 80
        targetPort: 3001
      selector:
        app: "nodejs-endpoint"
      type: "LoadBalancer"
      loadBalancerIP: ""
    ```
27. To deploy the load-balancer to Kubernetes cluster, run the following command:

    `kubectl apply --filename service.yml`

28. To list cluster assets: `kubectl get all`to confirm container and service are deployed
29. Back to Google Cloud Console, navigate to the Kubernetes clusters, select `Services & Ingress`
30. Under `Endpoints` column select `external IP address:80` link to test connectivity to the container deployed to the cluster
31. Don't forget to add query string parameter: `?name=John`
32. Alternatively, we could deploy the container and create a service using the Google Cloud Console
33. Deployment of Kubernetes on other cloud providers are similar
34. Deploying Kubernetes workloads is identical, almost...
35. At the end of the lab delete the created Kubernetes cluster to reduce monthly charges


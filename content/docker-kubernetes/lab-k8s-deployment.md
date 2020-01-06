# Lab: Kubernetes Deployment

1. Log in to Google Cloud Console
2. As a reminder GCP API's are eventually consistent, we may need to wait here and there
3. Create a new project `gcp-k8s` where to launch new GKE Cluster
4. Select the newly created project from the drop-down list and navigate to `Kubernetes Engine`and wait for the API being enabled
5. Select `Create cluster` once the API has been enabled
6. Choose the `Your first cluster` and provide a name
7. For location type select `zonal` and select the desired location
8. Select 1 for `Number of nodes`
9. Select the default version of the cluster from the drop-down list
10. Select `n1-standard-1` for the node size
11. Select `Create` link and wait for the cluster to provision - may take a few minutes
12. When the cluster is provisioned and ready, select `connect` link next to the cluster name
13. Select `Run in Cloud Shell` and then open the editor to switch to a full browser window
14. In the terminal panel test our connectivity to the cluster: `kubectl get node`
15. The result should list nodes with a few minutes age since we have created the cluster
16. To deploy our first application to the cluster we will reuse the node.js end-point from the previous lab
17. The docker image published to docker hub: [https://hub.docker.com/repository/docker/vkhazin/courseware-nodejs-container](https://hub.docker.com/repository/docker/vkhazin/courseware-nodejs-container)
18. Create a new folder
19. Add a new file under the root of the new folder: `deployment.yml` with the following content:
20. ```
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
21. To deploy the application to Kubernetes cluster: `kubectl apply --filename deployment.yml`
22. Expected outcome: `deployment.apps/nodejs-endpoint created`
23. To list cluster assets: `kubectl get all`
24. Add a new file `service.yml` under the root of the new folder with the following content:
25. ```
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
26. To deploy the load-balancer to Kubernetes cluster: `kubectl apply --filename service.yml`
27. To list cluster assets: `kubectl get all`to confirm container and service are deployed
28. Back to Google Cloud Console, navigate to the Kubernetes clusters, select `Services & Ingress`
29. Under `Endpoints` column select `external IP address:80` link to test connectivity to the container deployed to the cluster
30. Don't forget to add a query string parameter: `?name=John`




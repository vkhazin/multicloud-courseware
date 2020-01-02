# Lab: Kubernetes Deployment

Login to Google Cloud Console

As a reminder GCP API's are eventually consistent, we may need to wait here and there

Create a new project `gcp-k8s` where to launch new GKE Cluster

Select newly created project from the drop-down list and navigate to `Kubernetes Engine`and wait for the API being enabled

Select `Create cluster` once the API has been enabled

Choose the `Your first cluster` and provide a name

For location type select `zonal` and select a desired location

Select `3` for `Number of nodes`

Select the default version of the cluster from the drop-down list

Select `n1-standard-1` for the node size

Select `Create` link and wait for the cluster to provision

When the cluster is provisioned and ready, select `connect` link next to the cluster name

Select `Run in Cloud Shell` and then open editor to switch to a full browser window

In the terminal panel test our connectivity to the cluster: `kubectl get node`

The result should list 3 notes with a few minutes age since we have created the cluster

To deploy our first application to the cluster we will reuse the nodejs end-point

In the terminal window run: git clone `https://github.com/vkhazin/courseware-nodejs-container.git`

You can add the folder to the workspace in the editor explorer

In the terminal panel navigate to the new folder

Build docker image: `docker build ./ -t node/end-point`

Add a new file under the root of the cloned repository: `deployment.yml` with the following content:

```
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
      - image: node/end-point
        imagePullPolicy: Always
        name: nodejs-endpoint
```

To deploy the application to Kubernetes cluster: `kubectl apply --filename deployment.yml`

Expected outcome: `deployment.apps/nodejs-endpoint created`

To list cluster assets: `kubectl get all`

Add a new file `service.yml` under the root of the cloned repository with the following content:

```
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

To deploy the loadbalancer to Kubernetes cluster: `kubectl apply --filename service.yml`

To list cluster assets: `kubectl get all`


# Lab: K8s Native Monitoring

1. Navigate to [https://console.cloud.google.com/kubernetes/](https://console.cloud.google.com/kubernetes/workload)
2. Select the nodejs-endpoint workload we have created in the previous lab
3. You may want to hit the end-point using browser or curl command to generate some recent load and loads from the `Services & Ingress` navigation link
4. Notice the following:
   1. Container Logs
   2. Audit Logs
   3. CPU Load
   4. Memory Pressure
   5. Disk Utilization
   6. Time period selection
5. Proceed to `Services & Ingress` explore information available
6. Kubernetes comes with a Dashboard UI that is not deployed by default on GKE and is being [deprecated](https://cloud.google.com/kubernetes-engine/docs/concepts/dashboards) in favour of GKE Dashboards we've just explored
7. What can you gather from the native monitoring tools?


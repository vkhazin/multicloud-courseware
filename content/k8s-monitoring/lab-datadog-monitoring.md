# Datadog Kubernetes Monitoring

1. Using a web browser navigate to [https://www.datadoghq.com/](https://www.datadoghq.com/)
2. Sign-in or sign-up for a free account
3. When you are using a brand new datadog account you will see a message to install your first datadog agent, select `Kubernetes` on the left hand and an api-key will be displayed in the instructions
4. When you are using an existing datadog account navigate to `Integrations` -&gt; `API's` and copy your datadog API key
5. Install \`Datadog-Agent\` on our Kubernetes Cuslter as \`DaemonSet\`
6. The cluster we've created earlier is role-based access control \(RBAC\) enabled and we need to provide the \`DataDog-Agent\` necessary permissions for monitoring
7. Run the following command Google Cloud Shell connected to K8S cluster:
8. ```
   kubectl create -f "https://raw.githubusercontent.com/DataDog/datadog-agent/master/Dockerfiles/manifests/rbac/clusterrole.yaml"
   kubectl create -f "https://raw.githubusercontent.com/DataDog/datadog-agent/master/Dockerfiles/manifests/rbac/serviceaccount.yaml"
   kubectl create -f "https://raw.githubusercontent.com/DataDog/datadog-agent/master/Dockerfiles/manifests/rbac/clusterrolebinding.yaml"
   ```
9. Create datadog secret, make sure to use your API key copied in the previous steps:
10. ```
    kubectl create secret generic datadog-secret --from-literal api-key="your-api-key"
    ```
11. Create a new manifest file `datadog-agent.yml` with the content:
12. ```
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: datadog-agent
    spec:
      selector:
        matchLabels:
          app: datadog-agent
      template:
        metadata:
          labels:
            app: datadog-agent
          name: datadog-agent
        spec:
          serviceAccountName: datadog-agent
          containers:
          - image: datadog/agent:7
            imagePullPolicy: Always
            name: datadog-agent
            ports:
              - containerPort: 8125
                # Custom metrics via DogStatsD - uncomment this section to enable custom metrics collection
                # hostPort: 8125
                name: dogstatsdport
                protocol: UDP
              - containerPort: 8126
                # Trace Collection (APM) - uncomment this section to enable APM
                # hostPort: 8126
                name: traceport
                protocol: TCP
            env:
              - name: DD_API_KEY
                valueFrom:
                  secretKeyRef:
                    name: datadog-secret
                    key: api-key
              - name: DD_COLLECT_KUBERNETES_EVENTS
                value: "true"
              - name: DD_LEADER_ELECTION
                value: "true"
              - name: KUBERNETES
                value: "true"
              - name: DD_KUBERNETES_KUBELET_HOST
                valueFrom:
                  fieldRef:
                    fieldPath: status.hostIP
              - name: DD_APM_ENABLED
                value: "true"
            resources:
              requests:
                memory: "256Mi"
                cpu: "200m"
              limits:
                memory: "256Mi"
                cpu: "200m"
            volumeMounts:
              - name: dockersocket
                mountPath: /var/run/docker.sock
              - name: procdir
                mountPath: /host/proc
                readOnly: true
              - name: cgroups
                mountPath: /host/sys/fs/cgroup
                readOnly: true
            livenessProbe:
              exec:
                command:
                - ./probe.sh
              initialDelaySeconds: 15
              periodSeconds: 5
          volumes:
            - hostPath:
                path: /var/run/docker.sock
              name: dockersocket
            - hostPath:
                path: /proc
              name: procdir
            - hostPath:
                path: /sys/fs/cgroup
              name: cgroups
    ```
13. Deploy the daemon set execute in terminal: `kubectl create --filename datadog-agent.yml`
14. To validate the deployment execute in terminal: `kubectl get daemonset datadog-agent`
15. Expected result:
16. ```
    NAME            DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
    datadog-agent   3         3         3       3            3           <none>          74s
    ```
17. For more detailed info: `kubectl describe daemonset.apps/datadog-agent`
18. Using browser Navigate to [https://app.datadoghq.com/containers](https://app.datadoghq.com/containers)
19. You should see Kubernetes cluster containers and node.js end-point on the list, may take a moment to populate
20. Select the node.js end-point container and you should see the container metrics




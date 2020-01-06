# Datadog Kubernetes Monitoring

Using a web browser navigate to [https://www.datadoghq.com/](https://www.datadoghq.com/)

Sign-in or sign-up for a free account

When you are using a brand new datadog account you will see a message to install your first datadog agent, select `Kubernetes` on the left hand and an api-key will be displayed in the instructions

When you are using an existing datadog account navigate to `Integrations` -&gt; `API's` and copy your datadog API key

Install \`Datadog-Agent\` on our Kubernetes Cuslter as \`DaemonSet\`

The cluster we've created earlier is role-based access control \(RBAC\) enabled and we need to provide the \`DataDog-Agent\` necessary permissions for monitoring

Run the following command Google Cloud Shell connected to K8S cluster:

```
kubectl create -f "https://raw.githubusercontent.com/DataDog/datadog-agent/master/Dockerfiles/manifests/rbac/clusterrole.yaml"
kubectl create -f "https://raw.githubusercontent.com/DataDog/datadog-agent/master/Dockerfiles/manifests/rbac/serviceaccount.yaml"
kubectl create -f "https://raw.githubusercontent.com/DataDog/datadog-agent/master/Dockerfiles/manifests/rbac/clusterrolebinding.yaml"
```

Create datadog secret, make sure to use your API key copied in the previous steps:

```
kubectl create secret generic datadog-secret --from-literal api-key="your-api-key"
```

Create a new manifest file `datadog-agent.yml` with the content:

```
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

Deploy the daemon set execute in terminal: `kubectl create --filename datadog-agent.yml`

To validate the deployment execute in terminal: `kubectl get daemonset datadog-agent`

Expected result:

```
NAME            DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
datadog-agent   3         3         3       3            3           <none>          74s
```

Using browser Navigate to [https://app.datadoghq.com/containers](https://app.datadoghq.com/containers)

You should see Kubernetes cluster containers and node.js end-point on the list

Select the node.js end-point container and you should see the container metrics 


# kubernetes-tutorial

a# Kubernetes Learning Path

This document outlines a comprehensive 30-section learning path for Kubernetes, focusing on commands and practical usage. Each section introduces a new concept or command, building on the previous ones. By the end of this journey, you'll be well-versed in Kubernetes and ready to tackle advanced projects.

## Section 1: Introduction to Kubernetes

**What is Kubernetes?**

Kubernetes is an open-source platform for automating the deployment, scaling, and management of containerized applications. It groups containers that make up an application into logical units for easy management and discovery.

## Section 2: Setting Up Kubernetes on Docker Desktop

**How to set up Kubernetes on Docker Desktop?**

1. **Install Docker Desktop**: Download and install Docker Desktop from the official website.
2. **Enable Kubernetes**: Go to Docker Desktop settings, navigate to the Kubernetes tab, and check the "Enable Kubernetes" option.
3. **Verify Installation**: Open a terminal and run the following command to verify the installation:
   ```bash
   $ kubectl version
   ```

## Section 3: Understanding Kubernetes Architecture

**What are the main components of Kubernetes?**

Kubernetes architecture includes:

- **Master Node**: Manages the cluster and includes components like API Server, Scheduler, Controller Manager, and etcd.
- **Worker Nodes**: Run the containerized applications and include components like kubelet, kube-proxy, and container runtime.

## Section 4: Basic kubectl Commands

**How to interact with Kubernetes using kubectl?**

`kubectl` is the command-line tool used to interact with Kubernetes clusters. Here are some basic commands:

- **List Nodes**: `kubectl get nodes`
  - Example:
    ```bash
    $ kubectl get nodes
    NAME       STATUS   ROLES    AGE   VERSION
    node-1     Ready    <none>   7d    v1.21.3
    node-2     Ready    <none>   7d    v1.21.3
    ```

- **List Pods**: `kubectl get pods`
  - Example:
    ```bash
    $ kubectl get pods
    NAME        READY   STATUS    RESTARTS   AGE
    mypod       1/1     Running   0          1h
    ```

- **Describe Node**: `kubectl describe node <node-name>`
  - Example:
    ```bash
    $ kubectl describe node node-1
    ```

## Section 5: Creating a Kubernetes Cluster

**How to create a local Kubernetes cluster with Minikube?**

Minikube is a tool that lets you run Kubernetes locally. Here are the steps to create a cluster:

1. **Install Minikube**: Follow the installation instructions for your operating system from the Minikube documentation.
2. **Start a Cluster**: Use the `minikube start` command to start a local Kubernetes cluster.
   - Example:
     ```bash
     $ minikube start
     ```
3. **Verify the Cluster**: Check the status of the cluster with `kubectl`.
   - Example:
     ```bash
     $ kubectl get nodes
     NAME       STATUS   ROLES    AGE   VERSION
     minikube   Ready    master   1m    v1.21.3
     ```

## Section 6: Deploying Your First Application

**How to deploy a simple application?**

Deploying an application in Kubernetes involves creating a deployment and exposing it as a service:

1. **Create a Deployment**: Use `kubectl create deployment` to create a deployment.
   - Example:
     ```bash
     $ kubectl create deployment nginx --image=nginx
     ```

2. **Expose the Deployment**: Use `kubectl expose deployment` to expose the deployment as a service.
   - Example:
     ```bash
     $ kubectl expose deployment nginx --port=80 --type=NodePort
     ```

3. **Verify the Deployment**: Check the status of the deployment and service.
   - Example:
     ```bash
     $ kubectl get deployments
     $ kubectl get services
     ```

## Section 7: Understanding Pods

**What is a Pod?**

A Pod is the smallest deployable unit in Kubernetes, representing a single instance of a running process in your cluster. Pods can contain one or more containers, which share the same network namespace and storage.

## Section 8: Managing Pods

**How to manage Pods?**

Pods are the smallest deployable units in Kubernetes. Here are some commands to manage them:

- **Create a Pod**: Use `kubectl run` to create a pod.
  - Example:
    ```bash
    $ kubectl run mypod --image=nginx
    ```

- **Delete a Pod**: Use `kubectl delete pod` to delete a pod.
  - Example:
    ```bash
    $ kubectl delete pod mypod
    ```

- **Get Pod Details**: Use `kubectl describe pod` to get detailed information about a pod.
  - Example:
    ```bash
    $ kubectl describe pod mypod
    ```

## Section 9: Using Namespaces

**What are Namespaces?**

Namespaces are a way to divide cluster resources between multiple users. They provide a mechanism for isolating groups of resources within a single cluster.

## Section 10: Working with Namespaces

**How to create and use Namespaces?**

- **Create a Namespace**: Use `kubectl create namespace`.
  - Example:
    ```bash
    $ kubectl create namespace mynamespace
    ```

- **Use a Namespace**: Set the current namespace for `kubectl` commands.
  - Example:
    ```bash
    $ kubectl config set-context --current --namespace=mynamespace
    ```

## Section 11: Deployments and ReplicaSets

**What is a Deployment?**

A Deployment provides declarative updates to applications, managing ReplicaSets to ensure the desired number of Pods are running. It allows you to define the desired state of your application and Kubernetes will manage the rest.

## Section 12: Creating and Managing Deployments

**How to create and manage Deployments?**

Deployments manage the deployment and scaling of a set of Pods. Here are some commands:

- **Create a Deployment**: Use `kubectl create deployment`.
  - Example:
    ```bash
    $ kubectl create deployment myapp --image=myimage
    ```

- **Scale a Deployment**: Use `kubectl scale` to change the number of replicas.
  - Example:
    ```bash
    $ kubectl scale deployment myapp --replicas=3
    ```

- **Update a Deployment**: Use `kubectl set image` to update the image of a deployment.
  - Example:
    ```bash
    $ kubectl set image deployment/myapp myapp=myimage:v2
    ```

## Section 13: Services and Networking

**What is a Service?**

A Service is an abstraction that defines a logical set of Pods and a policy to access them. Services enable network access to a set of Pods.

## Section 14: Creating Services

**How to create a Service?**

Services expose your application to the network. Here are some commands:

- **Expose a Deployment**: Use `kubectl expose deployment`.
  - Example:
    ```bash
    $ kubectl expose deployment myapp --type=LoadBalancer --port=80
    ```

- **Create a Service**: Use `kubectl create service`.
  - Example:
    ```bash
    $ kubectl create service clusterip myapp --tcp=5678:8080
    ```

- **Get Service Details**: Use `kubectl get services` to list services.
  - Example:
    ```bash
    $ kubectl get services
    ```

## Section 15: ConfigMaps and Secrets

**What are ConfigMaps and Secrets?**

ConfigMaps and Secrets are used to manage configuration data and sensitive information, respectively. ConfigMaps store non-confidential data in key-value pairs, while Secrets store sensitive data.

## Section 16: Using ConfigMaps and Secrets

**How to create and use ConfigMaps and Secrets?**

- **Create a ConfigMap**: Use `kubectl create configmap`.
  - Example:
    ```bash
    $ kubectl create configmap myconfig --from-literal=key1=value1
    ```

- **Create a Secret**: Use `kubectl create secret`.
  - Example:
    ```bash
    $ kubectl create secret generic mysecret --from-literal=password=mysecretpassword
    ```

- **Use in Pods**: Reference ConfigMaps and Secrets in Pod specifications.
  - Example:
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
      - name: mycontainer
        image: nginx
        envFrom:
        - configMapRef:
            name: myconfig
        - secretRef:
            name: mysecret
    ```

## Section 17: Persistent Storage

**What is Persistent Storage in Kubernetes?**

Persistent Volumes (PVs) and Persistent Volume Claims (PVCs) provide storage resources in a Kubernetes cluster. PVs are a piece of storage in the cluster that has been provisioned by an administrator, while PVCs are a request for storage by a user.

## Section 18: Using Persistent Volumes

**How to create and use Persistent Volumes?**

- **Create a PV**: Define a PV in a YAML file and apply it.
  - Example:
    ```yaml
    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: mypv
    spec:
      capacity:
        storage: 1Gi
      accessModes:
        - ReadWriteOnce
      hostPath:
        path: /mnt/data
    ```
    ```bash
    $ kubectl apply -f pv.yaml
    ```

- **Create a PVC**: Define a PVC in a YAML file and apply it.
  - Example:
    ```yaml
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 1Gi
    ```
    ```bash
    $ kubectl apply -f pvc.yaml
    ```

- **Use in Pods**: Reference the PVC in Pod specifications.
  - Example:
    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
      - name: mycontainer
        image: nginx
        volumeMounts:
        - mountPath: "/mnt/data"
          name: myvolume
      volumes:
      - name: myvolume
        persistentVolumeClaim:
          claimName: mypvc
    ```

## Section 19: StatefulSets

**What is a StatefulSet?**

StatefulSets manage the deployment and scaling of a set of Pods, providing guarantees about the ordering and uniqueness of these Pods. They are used for stateful applications that require stable, unique network identifiers and persistent storage.

## Section 20: Creating StatefulSets

**How to create a StatefulSet?**

- **Create a StatefulSet**: Define a StatefulSet in a YAML file and apply it.
  - Example:
    ```yaml
    apiVersion: apps/v1
    kind: StatefulSet
    metadata:
      name: web
    spec:
      serviceName: "nginx"
      replicas: 3
      selector:
        matchLabels:
          app: nginx
      template:
        metadata:
          labels:
            app: nginx
        spec:
          containers:
          - name: nginx
            image: k8s.gcr.io/nginx-slim:0.8
            ports:
            - containerPort: 80
              name: web
            volumeMounts:
            - name: www
              mountPath: /usr/share/nginx/html
      volumeClaimTemplates:
      - metadata:
          name: www
        spec:
          accessModes: [ "ReadWriteOnce" ]
          resources:
            requests:
              storage: 1Gi
    ```
    ```bash
    $ kubectl apply -f statefulset.yaml
    ```

## Section 21: Ingress Controllers

**What is an Ingress Controller?**

An Ingress Controller manages external access to the services in a cluster, typically HTTP. It provides load balancing, SSL termination, and name-based virtual hosting.

## Section 22: Setting Up Ingress

**How to set up an Ingress?**

- **Create an Ingress Resource**: Define an Ingress in a YAML file and apply it.
  - Example:
    ```yaml
    apiVersion: networking.k8s.io/v1
    kind: Ingress
    metadata:
      name: example-ingress
    spec:
      rules:
      - host: example.com
        http:
          paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: example-service
                port:
                  number: 80
    ```
    ```bash
    $ kubectl apply -f ingress.yaml
    ```

- **Verify Ingress**: Check the status of the Ingress.
  - Example:
    ```bash
    $ kubectl get ingress
    ```

## Section 23: Monitoring and Logging

**How to monitor and log in Kubernetes?**

Monitoring and logging are essential for managing Kubernetes clusters. Tools like Prometheus and Fluentd are commonly used.

- **Install Prometheus**: Use Helm to install Prometheus.
  - Example:
    ```bash
    $ helm install prometheus stable/prometheus
    ```

- **Install Fluentd**: Use Helm to install Fluentd.
  - Example:
    ```bash
    $ helm install fluentd stable/fluentd
    ```

- **Check Metrics**: Use `kubectl top` to check resource usage.
  - Example:
    ```bash
    $ kubectl top nodes
    $ kubectl top pods
    ```

## Section 24: Using Helm

**What is Helm?**

Helm is a package manager for Kubernetes, simplifying the deployment of applications. It uses charts, which are packages of pre-configured Kubernetes resources.

## Section 25: Installing and Using Helm

**How to install and use Helm?**

- **Install Helm**: Follow the installation instructions from the Helm documentation.
- **Add a Repository**: Add a Helm chart repository.
  - Example:
    ```bash
    $ helm repo add stable https://charts.helm.sh/stable
    ```

- **Install a Chart**: Use `helm install` to deploy an application.
  - Example:
    ```bash
    $ helm install myapp stable/nginx
    ```

- **List Releases**: Use `helm list` to list installed releases.
  - Example:
    ```bash
    $ helm list
    ```

## Section 26: RBAC (Role-Based Access Control)

**What is RBAC?**

RBAC is a method of regulating access to resources based on the roles of individual users. It allows you to define roles and assign them to users or groups.

## Section 27: Setting Up RBAC

**How to set up RBAC?**

- **Create a Role**: Define a Role in a YAML file and apply it.
  - Example:
    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: Role
    metadata:
      namespace: default
      name: pod-reader
    rules:
    - apiGroups: [""]
      resources: ["pods"]
      verbs: ["get", "watch", "list"]
    ```
    ```bash
    $ kubectl apply -f role.yaml
    ```

- **Bind the Role**: Define a RoleBinding in a YAML file and apply it.
  - Example:
    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: RoleBinding
    metadata:
      name: read-pods
      namespace: default
    subjects:
    - kind: User
      name: jane
      apiGroup: rbac.authorization.k8s.io
    roleRef:
      kind: Role
      name: pod-reader
      apiGroup: rbac.authorization.k8s.io
    ```
    ```bash
    $ kubectl apply -f rolebinding.yaml
    ```

## Section 28: Autoscaling

**What is Autoscaling?**

Autoscaling automatically adjusts the number of Pods in a deployment based on resource usage. Kubernetes supports Horizontal Pod Autoscaling (HPA) to scale the number of pods.

## Section 29: Implementing Autoscaling

**How to implement Autoscaling?**

- **Enable Autoscaling**: Use `kubectl autoscale` to set up autoscaling.
  - Example:
    ```bash
    $ kubectl autoscale deployment myapp --cpu-percent=50 --min=1 --max=10
    ```

- **Check Autoscaler**: Use `kubectl get hpa` to check the status of the Horizontal Pod Autoscaler.
  - Example:
    ```bash
    $ kubectl get hpa
    ```

## Section 30: Rolling Updates and Rollbacks

**How to perform rolling updates and rollbacks?**

- **Update a Deployment**: Use `kubectl set image` to update the image of a deployment.
  - Example:
    ```bash
    $ kubectl set image deployment/myapp myapp=myimage:v2
    ```

- **Check Rollout Status**: Use `kubectl rollout status` to check the status of the rollout.
  - Example:
    ```bash
    $ kubectl rollout status deployment/myapp
    ```

- **Rollback a Deployment**: Use `kubectl rollout undo` to rollback to a previous version.
  - Example:
    ```bash
    $ kubectl rollout undo deployment/myapp
    ```


Citations:
[1] https://blog.stackademic.com/blue-green-deployment-using-kubernetes-4fbd023a19c5?gi=36542b52b934
[2] https://dev.to/pavanbelagatti/kubernetes-deployments-rolling-vs-canary-vs-blue-green-4k9p
[3] https://codefresh.io/learn/software-deployment/what-is-blue-green-deployment/
[4] https://kubernetes.io/blog/2018/04/30/zero-downtime-deployment-kubernetes-jenkins/
[5] https://semaphoreci.com/blog/continuous-blue-green-deployments-with-kubernetes
[6] https://github.com/ianlewis/kubernetes-bluegreen-deployment-tutorial
[7] https://www.youtube.com/watch?v=er0JTGnryZ4
[8] https://www.geeksforgeeks.org/what-is-kubernetes-blue-green-deployment/











```markdown
# Kubernetes Final Projects

## Project 1: Blue-Green Deployment

**Objective**: Implement a blue-green deployment strategy to minimize downtime and reduce risk.

1. **Create a Namespace**:
   ```bash
   $ kubectl create namespace blue-green-deployment
   ```

2. **Create Blue Deployment**: Deploy the initial version of the application.
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: myapp-blue
     namespace: blue-green-deployment
     labels:
       app: myapp
       version: blue
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: myapp
         version: blue
     template:
       metadata:
         labels:
           app: myapp
           version: blue
       spec:
         containers:
         - name: myapp
           image: myapp:v1
           ports:
           - containerPort: 80
   ```
   ```bash
   $ kubectl apply -f blue-deployment.yaml
   ```

3. **Expose Blue Deployment**: Expose the blue deployment as a service.
   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: myapp-service
     namespace: blue-green-deployment
   spec:
     selector:
       app: myapp
       version: blue
     ports:
     - protocol: TCP
       port: 80
       targetPort: 80
   ```
   ```bash
   $ kubectl apply -f blue-service.yaml
   ```

4. **Create Green Deployment**: Deploy the new version of the application.
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: myapp-green
     namespace: blue-green-deployment
     labels:
       app: myapp
       version: green
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: myapp
         version: green
     template:
       metadata:
         labels:
           app: myapp
           version: green
       spec:
         containers:
         - name: myapp
           image: myapp:v2
           ports:
           - containerPort: 80
   ```
   ```bash
   $ kubectl apply -f green-deployment.yaml
   ```

5. **Switch Traffic to Green Deployment**: Update the service to point to the green deployment.
   ```bash
   $ kubectl patch service myapp-service -n blue-green-deployment -p '{"spec":{"selector":{"app":"myapp","version":"green"}}}'
   ```

6. **Verify Deployment**: Ensure the service is now pointing to the green deployment.
   ```bash
   $ kubectl get services -n blue-green-deployment
   $ kubectl get pods -n blue-green-deployment -l app=myapp,version=green
   ```

7. **Clean Up**: Optionally, delete the blue deployment if everything is working fine.
   ```bash
   $ kubectl delete deployment myapp-blue -n blue-green-deployment
   ```

## Project 2: Small Web Application (Python/Flask)

**Objective**: Deploy a small web application using Python and Flask.

1. **Create a Flask Application**:
   - `app.py`:
     ```python
     from flask import Flask
     app = Flask(__name__)

     @app.route('/')
     def hello_world():
         return 'Hello, World!'

     if __name__ == '__main__':
         app.run(host='0.0.0.0')
     ```

   - `requirements.txt`:
     ```
     Flask==2.0.1
     ```

2. **Create a Dockerfile**:
   ```Dockerfile
   FROM python:3.8-slim
   WORKDIR /app
   COPY requirements.txt requirements.txt
   RUN pip install -r requirements.txt
   COPY . .
   CMD ["python", "app.py"]
   ```

3. **Build Docker Image**:
   ```bash
   $ docker build -t myflaskapp .
   ```

4. **Push Docker Image to a Registry** (Optional):
   ```bash
   $ docker tag myflaskapp <your-dockerhub-username>/myflaskapp
   $ docker push <your-dockerhub-username>/myflaskapp
   ```

5. **Create Kubernetes Deployment**:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: myflaskapp
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: myflaskapp
     template:
       metadata:
         labels:
           app: myflaskapp
       spec:
         containers:
         - name: myflaskapp
           image: <your-dockerhub-username>/myflaskapp
           ports:
           - containerPort: 80
   ```
   ```bash
   $ kubectl apply -f flask-deployment.yaml
   ```

6. **Expose Deployment**:
   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: myflaskapp-service
   spec:
     selector:
       app: myflaskapp
     ports:
     - protocol: TCP
       port: 80
       targetPort: 80
     type: LoadBalancer
   ```
   ```bash
   $ kubectl apply -f flask-service.yaml
   ```

7. **Verify Deployment**:
   ```bash
   $ kubectl get services
   $ kubectl get pods
   ```

## Project 3: Large Application with Multiple Containers and Monitoring

**Objective**: Deploy a large application with multiple containers, including a monitoring container to monitor the cluster.

1. **Create Dockerfiles for Each Service**:
   - Example for a web service (Node.js):
     ```Dockerfile
     FROM node:14
     WORKDIR /app
     COPY package*.json ./
     RUN npm install
     COPY . .
     CMD ["node", "web.js"]
     ```

   - Example for a database service (MySQL):
     ```Dockerfile
     FROM mysql:5.7
     ENV MYSQL_ROOT_PASSWORD=rootpassword
     ENV MYSQL_DATABASE=mydatabase
     ```

   - Example for a monitoring service (Prometheus):
     ```Dockerfile
     FROM prom/prometheus
     COPY prometheus.yml /etc/prometheus/
     ```

2. **Build Docker Images**:
   ```bash
   $ docker build -t mywebapp -f Dockerfile.web .
   $ docker build -t mydb -f Dockerfile.db .
   $ docker build -t mymonitoring -f Dockerfile.monitoring .
   ```

3. **Push Docker Images to a Registry** (Optional):
   ```bash
   $ docker tag mywebapp <your-dockerhub-username>/mywebapp
   $ docker tag mydb <your-dockerhub-username>/mydb
   $ docker tag mymonitoring <your-dockerhub-username>/mymonitoring
   $ docker push <your-dockerhub-username>/mywebapp
   $ docker push <your-dockerhub-username>/mydb
   $ docker push <your-dockerhub-username>/mymonitoring
   ```

4. **Create Kubernetes Deployments**:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: webapp
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: webapp
     template:
       metadata:
         labels:
           app: webapp
       spec:
         containers:
         - name: webapp
           image: <your-dockerhub-username>/mywebapp
           ports:
           - containerPort: 80
   ```
   ```bash
   $ kubectl apply -f webapp-deployment.yaml
   ```

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: db
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: db
     template:
       metadata:
         labels:
           app: db
       spec:
         containers:
         - name: db
           image: <your-dockerhub-username>/mydb
           ports:
           - containerPort: 3306
   ```
   ```bash
   $ kubectl apply -f db-deployment.yaml
   ```

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: monitoring
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: monitoring
     template:
       metadata:
         labels:
           app: monitoring
       spec:
         containers:
         - name: prometheus
           image: <your-dockerhub-username>/mymonitoring
           volumeMounts:
           - name: config-volume
             mountPath: /etc/prometheus
           volumes:
           - name: config-volume
             configMap:
               name: prometheus-config
   ```
   ```bash
   $ kubectl apply -f monitoring-deployment.yaml
   ```

5. **Expose Services**:
   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: webapp-service
   spec:
     selector:
       app: webapp
     ports:
     - protocol: TCP
       port: 80
       targetPort: 80
     type: LoadBalancer
   ```
   ```bash
   $ kubectl apply -f webapp-service.yaml
   ```

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: db-service
   spec:
     selector:
       app: db
     ports:
     - protocol: TCP
       port: 3306
       targetPort: 3306
   ```
   ```bash
   $ kubectl apply -f db-service.yaml
   ```

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: monitoring-service
   spec:
     selector:
       app: monitoring
     ports:
     - protocol: TCP
       port: 9090
       targetPort: 9090
     type: LoadBalancer
   ```
   ```bash
   $ kubectl apply -f monitoring-service.yaml
   ```

6. **Verify Deployments**:
   ```bash
   $ kubectl get services
   $ kubectl get pods
   ```

7. **Create a ConfigMap for Prometheus**:
   ```yaml
   apiVersion: v1
   kind: ConfigMap
   metadata:
     name: prometheus-config
   data:
     prometheus.yml: |
       global:
         scrape_interval: 15s
       scrape_configs:
         - job_name: 'kubernetes'
           kubernetes_sd_configs:
             - role: pod
   ```
   ```bash
   $ kubectl apply -f prometheus-config.yaml
   ```

8. **Update Monitoring Deployment to Use ConfigMap**:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: monitoring
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: monitoring
     template:
       metadata:
         labels:
           app: monitoring
       spec:
         containers:
         - name: prometheus
           image: prom/prometheus
           volumeMounts:
           - name: config-volume
             mountPath: /etc/prometheus
           volumes:
           - name: config-volume
             configMap:
               name: prometheus-config
   ```
   ```bash
   $ kubectl apply -f monitoring-deployment.yaml
   ```

By following these steps, you can deploy a blue-green deployment, a small web application using Python and Flask, and a large application with multiple containers, including monitoring, in Kubernetes.
```

# Integrating Kubernetes in CICD Pipeline

- Deployment YAML file: regapp-deployment.yaml

```yaml

apiVersion: apps/v1 
kind: Deployment
metadata:
  name: ranadurlabh-regapp
  labels: 
     app: regapp

spec:
  replicas: 2 
  selector:
    matchLabels:
      app: regapp

  template:
    metadata:
      labels:
        app: regapp
    spec:
      containers:
      - name: regapp
        image: ranadurlabh/regapp
        imagePullPolicy: Always
        ports:
        - containerPort: 8080
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1

```

- Service YAML file: regapp-service.yaml

```yaml

apiVersion: v1
kind: Service
metadata:
  name: ranadurlabh-service
  labels:
    app: regapp 
spec:
  selector:
    app: regapp 

  ports:
    - port: 8080
      targetPort: 8080
      nodePort: 31234

  type: NodePort # We need to use LoadBalancer when running the services and deployments on Cloud

```

- Apply both the files
  ```bash
    kubectl apply -f regapp-deployment.yaml -f regapp-service.yaml
  ```

  # Integrate Kubernetes Cluster with Ansible

  1. On Bootstarp Server
     1. Create user: ansadmin
     2. Add user 'ansadmin' to sudoers
     3. Enable password based login
  2. On Ansible Server
     1. Add Bootstarp server to /etc/ansible/hosts
     2. copy ssh keys to Bootstrap Server
     3. Test the connection
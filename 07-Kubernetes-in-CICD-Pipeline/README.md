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

  1. On Bootstarp Server (The one which is running kubectl)
     1. Create user: ansadmin
     2. Add user 'ansadmin' to sudoers
     3. Enable password based login (edit: /etc/ssh/sshd_config)
  2. On Ansible Server
     1. Add Bootstarp server to /etc/ansible/hosts
     2. copy ssh keys to Bootstrap Server
     3. Test the connection

- On Ansible Server create deployment and service file
  - `vi kube_deploy.yaml`
  ```yaml
  ---
  - hosts: kubernetes
  #  become: trur # to become root
    user: root

    tasks:
      - name: deploy regapp on kubernetes
        command: kubectl apply -f /home/ubuntu/cicd-with-jenkins-ansible-kubernetes/regapp-deployment.yaml

  ```

  - `vi kube_service.yaml`
```yaml
---
- hosts: kubernetes
#  become: true
  user: root

  tasks:
    - name: Deploy regapp Service on Kubernetes
      command: kubectl apply -f /home/ubuntu/cicd-with-jenkins-ansible-kubernetes/regapp-deployment.yaml
```

> add ip address of the kubernetes-controller in /etc/ansible/hosts
```console
[kubernetes]
192.168.1.10

```

- Execute Ansible Playbook
  - ` ansible-playbook kube_deploy.yaml `

# Creating Jenkins Deployment Job for Kubernetes

## Continous Deployment
- Create a Jenkins Job to execute commands on Ansible-Server to run the Ansible-Playbook
  - GOTO Jenkins Dashboard -> New Item: Artifact_on_Kubernetes -> FreeStyle -> Ok
  - Under Build Action -> Send build artifacts over SSH -> SSH server name: Ansible-Server from Drop Down
  -> exec commands: 
  ```console
  ansible-playbook /opt/docker/kube_deploy.yaml;
  ansible-playbook /opt/docker/kube_service.yaml;
  
  ```
  -> Apply and Save


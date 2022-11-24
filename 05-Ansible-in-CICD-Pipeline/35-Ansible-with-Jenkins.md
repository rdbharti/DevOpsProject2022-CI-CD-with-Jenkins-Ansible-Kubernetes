# Integrate Ansible with Jenkins

- Jenkins dashboard -> configure system -> publish over ssh
  - ADD: 
    - Name: Ansible-Server
    - Hostname: 192.168.1.13
    - Username: ansadmin
    - Clock Advanced and provide Password of user 'ansadmin'
    - Click Test configuration -> Success

# Copy Jenkins Artifact to Ansible System

1. Setup on Ansible-Server:
   1. ` sudo mkdir /opt/docker`
   2. ` sudo chown ansadmin:ansadmin /opt/docker`
2. New Item: Job Name:Copy_Artifacts_onto_Ansible
3. Copy From: BuildAndDeployContainer
4.  Post-build Actions 
    1.  SSH Server
        1.  Name: Ansible-server
        2.  Source Files: webapp/target/*.war
        3.  Remove prefix: webapp/target
        4.  Remote directory: //opt//docker
        5.  Exec Command: Blank (for this demo, later replace it with ansible playbook)

# Build an Image and Create Container on Ansible.

1. Install docker on Ansible-Server
2. ADD user ansadmin to docker-group ` usermod -aG docker ansadmin `
3. Create a Dockerfile
   ```console
    FROM tomcat:latest
    RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
    COPY ./*.war /usr/local/tomcat/webapps
   ```
4. Build image from Dockerfile: ` docker build -t <img-name> <path-docker-file> `
   ```console
    docker build -t regapp:v1 .
   ```

    Got Error
   ```bash
    Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/build?buildargs=%7B%7D&cachefrom=%5B%5D&cgroupparent=&cpuperiod=0&cpuquota=0&cpusetcpus=&cpusetmems=&cpushares=0&dockerfile=Dockerfile&labels=%7B%7D&memory=0&memswap=0&networkmode=default&rm=1&shmsize=0&t=regapp%3Av1&target=&ulimits=null&version=1": dial unix /var/run/docker.sock: connect: permission denied

    # We will give permission to /var/run/docker.sock file
    chmod 777 /var/run/docker.sock

    # Or run the command with sudo 
   ```

# Ansible Playbook to Craete Image and Container

- On ansible-server
  - Add Ansible-Server ip in /etc/ansible/hosts
  - On ansible-server as ansadmin run ` ssh-copy-id 192.168.1.13` # ip-add of ansible-server so that ansible can access ansible server 
  - Creating Ansible-playbook
  ```yaml

   # vi regapp.yaml
   
   ---
   - hosts: 192.168.1.13
   tasks:
   - name: create docker image
      command: docker build -t regapp:latest .
      args:
         chdir: /opt/docker

   ```

  - Run Ansible Playbook `ansible-playbook regapp.yaml` 
  - Above playbook will create a docker image
    - docker images -> regapp:latest

# Copy Image Onto Docker-Hub

- Login to docker-hub from ansible-server
  ```console

  docker login
   username:
   password:
  ```
- TO Push image to docker hub the format of the image-name should be `<username>/image-name:tag ` i.e ranadurlabh/regapp:latest
  - ` docker tag <image-id> ranadurlabh/regapp:latest `
  - ` docker tag aac8ca8677f8 ranadurlabh/regapp:latest `
  ```bash
   docker push ranadurlabh/regapp:latest
  ```

# Jenkins Job To Build an Image onto Ansible


## On Ansible-Server

```yaml

---
- hosts: ansible
  tasks:
  - name: create docker image
    command: docker build -t regapp:latest .
    args:
      chdir: /opt/docker

  - name: Create Tag to Push Image onto Docker Hub
    command: docker tag regapp:latest ranadurlabh/regapp:latest

  - name: Push docker image
    command: docker push ranadurlabh/regapp:latest

```

## On Jenkins

- Dashboard -> Copy_Artifacts_Onto-Ansible -> Configure
  - Under Post-build Actions -> Send build artifacts over SSH -> exec-code
    - ` ansible-paybook regapp.yaml `

# Using Ansible to Create containers.

- Write an Ansible Playbook to create a container on Docker-Host
- Docker-Host will pull Image from Docker-hub and create a container.
- On Ansible-Server ` vi deploy_regapp.yaml`
  ```yaml

   ---

   - hosts: docker-host
      
      tasks:
      - name: Create Container
         command: docker run -d --name regapp-server -p 8082:8080 ranadurlabh/regapp:latest

  ```
- On Docker Server execute following command
  - ` sudo su - ansadmin`
  - ` chmod 777 /var/run/docker.sock`
- On Ansible Server
  - ` ansible-palyboon deploy_regapp.yaml`

> The above command will create container only once, if that command is executed again then it will throw an error, because multiple containers with same name can't be created.

- We will have to delete the Old container to create a new container with the same name.




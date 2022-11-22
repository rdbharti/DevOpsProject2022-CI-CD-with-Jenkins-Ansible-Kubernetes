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

   
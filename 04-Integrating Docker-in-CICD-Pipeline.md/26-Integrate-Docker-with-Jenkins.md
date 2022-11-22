# Integrate Docker with Jenkins

1. Create a dedicated user: dockeradmin
2. Add the 'dockeradmin' user to docker group
3. Install 'Publish Over SSH' plugin
4. Add Dockerhost to Jenkins 'Configure System'

- Create user and Add that user to docker group
```bash
useradd dockeradmin
passwd dockeradmin # add password for the user
# password:

usermod -aG docker dockeradmin
```

- Install Plugin on Jenkins
  - Publish Over SSH (This Plugin may have depricated, instead we can use SSH plugin)
  - Configure for docker host: dasboard -> Manage Plugins -> Configure Syatem -> Publish Over SSH
    - ADD SSH Server
    - Name: Docker-Server
    - Hostname: ip-addr of docker server (in my case local ubuntu-vm 192.168.1.10)
    - Username: dockeradmin
    - check Use Password Authentication:
      - Enter Password:  \<of dockeradmin\>
    - Test Configuration -> Success
    - Apply and Save

# Create A Jenkins Job

1. Create a Job: BuildandDeployOnContainer
2. Copy the configuration: BuildAndDeploy
3. Delete  Post-build Actions
4. Add Post Build Action:
   1. SSH Server Name: Drop-down list
   2. Transfer Set -> Source files webapp/target/*.war
   3. Remove prefix: webapp/target
   4. Remote Directry: Blank (Default Home)
5. Apply and Save
   
The Above build will create a folder with war files in Home directory on the server.
We will now copy these .war files to docker container.

# We will create docker-image with artifacts(.war files)

1. Create a directory in /opt
   ```bash
    mkdir /opt/docker
    chown -R dockeradmin:dockeradmin /opt/docker # giving ownership to dockeradmin
   ```
2. change Remote directory (in Post-Build Action): //opt//docker (remember to put double /)
3. Edit Dockerfile to copy .war files to container working dir

```console
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
COPY ./*.war /usr/local/tomcat/webapps
```

4. Create Image: ` docker build -t tomcat:v1 . ` 
5. 
   Output: 

   | REPOSITORY | TAG | IMAGE ID | CREATED | SIZE |
   |------------|-----|----------|---------|------|
   |tomcat | v1 | da4582a84f6c | 34 | seconds | ago | 478MB

6. Create a container from the image

```
docker run -d --name tomcatv1 -p 8086:8080 tomcat:v1

```
7. Access the webapp by viewing in browseer: http://192.168.1.10:8086/webapp/

> We need to find a way so that all the manual docker image build or container build is done automatically on Jenkins Build.

# Automate Build and Deployment on Docker Container
# Create a Docker Container

1. Get Image URL of TOMCAT on http://hub.docker.com
2. Execute Code: ` docker pull tomcat `
3. Create Container with tomcat image
   ```bash
   # Using Port 8082 coz all my services are running on UBUNTU vm
   # 8080 -> Jenkins; 8081 -> Tomcat Service; 8082 -> Tomcat Container
   docker run -d --name tomcat-conatiner -p 8082:8080 tomcat:latest
   ```

## Fixing Tomcat Container Issue

1. Go into the tomcat container
   ```bash
   # Access bash shell of tomcat container
   # docker exec -it container-name /bin/bash

   docker exec -it tomcat-container /bin/bash

   ```
2. When we do listing in webapp (inside container), we find there is no file there.
3. The contents of webapp are present in webapp.dist directory.
4. Copy the contents of webapp.dist to webapp/
   ```bash
    cp -R webapps.dist/* webapps
   ``
5. Reload the Browser for working tomcat container
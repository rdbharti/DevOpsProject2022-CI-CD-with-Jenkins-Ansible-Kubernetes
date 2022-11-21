# Create a Docker COntainer

1. Get Image URL of TOMCAT on http://hub.docker.com
2. Execute Code: ` docker pull tomcat `
3. Create Container with tomcat image
   ```bash
   # Using Port 8082 coz all my services are running on UBUNTU vm
   # 8080 -> Jenkins; 8081 -> Tomcat Service; 8082 -> Tomcat Container
   docker run -d --name tomcat-conatiner -p 8082:8080 tomcat:latest
   ```
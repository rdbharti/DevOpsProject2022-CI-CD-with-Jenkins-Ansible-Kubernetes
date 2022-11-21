# Integrate Tomcat With Jenkins

1. Install Plugin Deploy to Container
2. Configure Tomcat Server with Credentials
   1. Dashboard -> Manage Jenkins -> Manage Credentials -> Jenkins/System -> Global Credentials -> ADD
3. Deploy Code To Tomcat Server
   1. Dashboard -> New Item: BuildAndDeploy -> Click Maven -> OK
   2. Description: Build and Deploy
   3. Source Code Management: Git
      1. RepoURL
   4. Post Build Action:
      1. Deploy war/ear to a container
      2. Add Containers:
         1. Tomcat <version>
         2. Credential dropdown
         3. Tomcat URL: http://<ip-address>:<port>
   5. Apply and Save
4. Goto http://<tomcat-ip-address>:<port>/manager/webapp

# Deploy Artifacts On a Tomcat Server

1. Clone the repo on local machine
2. Make Changes
3. Push the code
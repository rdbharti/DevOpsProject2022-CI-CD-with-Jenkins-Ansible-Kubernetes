# Customized Dockerfile for Tomcat

```console
FROM tomcat:latest
RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
```

Build
```bash
docker build -t demotomcat .
```
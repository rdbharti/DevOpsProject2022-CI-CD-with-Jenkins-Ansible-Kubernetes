# Integrating Ansible with CICD Pipeline

- Till previous section, we were using jenkins as a Deployment tool.
- In this section we will use Ansible as a Deployment tool.
- Jenkins is primary a Build tool.
- Jenkins will send artifacts to Ansible, and then Ansible will deploy the artifact, create docker image and deploy the application onto a container.


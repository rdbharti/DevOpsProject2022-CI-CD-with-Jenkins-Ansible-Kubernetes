# Integrating Ansible with CICD Pipeline

- Till previous section, we were using jenkins as a Deployment tool.
- In this section we will use Ansible as a Deployment tool.
- Jenkins is primary a Build tool.
- Jenkins will send artifacts to Ansible, and then Ansible will deploy the artifact, create docker image and deploy the application onto a container.

# Ansible Installation

1. Setup EC2 Instance
2. Setup hostname
3. Create user: ansadmin
4. Add user 'ansadmin' to sudoers file # visudo
5. Generate SSH keys
6. Enable Password based login on ec2-instance # vi /etc/ssh/sshd_config; uncomment PasswordAuthentication yes 
7. Install Ansible.
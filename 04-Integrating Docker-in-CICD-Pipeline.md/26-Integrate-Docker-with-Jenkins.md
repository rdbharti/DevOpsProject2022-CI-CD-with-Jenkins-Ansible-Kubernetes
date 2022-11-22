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
  - Publish Over SSH
  - Configure for docker host: dasboard -> Manage Plugins -> Configure Syatem -> Publish Over SSH
    - ADD SSH Server
    - Name: Docker-Server
    - Hostname: ip-addr of docker server (in my case local ubuntu-vm 192.168.1.10)
    - Username: dockeradmin
    - check Use Password Authentication:
      - Enter Password:  \<of dockeradmin\>
    - Test Configuration -> Success
    - Apply and Save
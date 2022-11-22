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
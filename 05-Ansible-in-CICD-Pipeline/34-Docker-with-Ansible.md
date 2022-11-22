# Integrate Docker with Ansible

1. On Docker Host
   1. Create user: ansadmin
   2. ADD ansdadmin to sudoers
   3. Enable Password based login
2. On Ansible Node
   1. Add docker Host IP to hosts file
      1. edit /etc/ansible/hosts
      2. ADD docker-host ip ` 192.168.1.10 `
   2. copy ssh keys
      1. ` ssh-keygen `
      2. ` ssh-copy-id docker-host-ip ` -> ` ssh-copy-id 192.168.1.10`
      3. This will create '.ssh/authorized_keys' on docker-host
   3. Test the connection
   
    ```console
        ansible all -m ping
        192.168.1.10 | SUCCESS => {
        "changed": false,
        "ping": "pong"
        }
    ```
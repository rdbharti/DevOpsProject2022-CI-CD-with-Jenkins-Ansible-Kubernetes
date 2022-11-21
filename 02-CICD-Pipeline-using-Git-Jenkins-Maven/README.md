# CICD Pipeline using Git Jenkins Maven
----

# Setup Jenkins Server
- On AWS
  1. Setup Linux EC2 instance
     1. Use Amazon Linux 2 
     2. t2.micro
     3. Security Group: Open Port 22(SSH) and 8080(Jenkins)
     4. Use Key pair for SSH connection to ec2 instance. \
    [Follow Jenkins Official website for install instructions](https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/)
  2. Install Java
     1. Ensure that your software packages are up to date on your instance by uing the following command to perform a quick software update:
     ```bash
     [ec2-user ~]$ sudo yum update –y
     ```
     2. Add the Jenkins repo using the following command:
        ```bash
        [ec2-user ~]$ sudo wget -O /etc/yum.repos.d/jenkins.repo \
        https://pkg.jenkins.io/redhat-stable/jenkins.repo        
        ```
     3. Import a key file from Jenkins-CI to enable installation from the package:
        ```bash 
        [ec2-user ~]$ sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

        [ec2-user ~]$ sudo yum upgrade
        ```
     4. Install Java:

        ```bash
        [ec2-user ~]$ sudo amazon-linux-extras install java-openjdk11 -y
        ```


  3. Install Jenkins
     1. ```bash 

        [ec2-user ~]$ sudo yum install jenkins -y

        ```
  4. Start Jenkins on Port 8080
     1. Enable the Jenkins service to start at boot:

        ```bash
        [ec2-user ~]$ sudo systemctl enable jenkins
        ```
      2. Start Jenkins as a service:
        ```bash
        [ec2-user ~]$ sudo systemctl start jenkins
        ```
      3. You can check the status of the Jenkins service using the command:

        ```bash
        [ec2-user ~]$ sudo systemctl status jenkins
        ```
- On Ubuntu
  - Run the following commands
  ```bash
  # Long Term Support Release

    curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
    /usr/share/keyrings/jenkins-keyring.asc > /dev/null
    echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update
    sudo apt-get install jenkins
    ```

    - Install Java
    
    ```bash
      $ sudo apt install openjdk-11-jre
      $ java -version

    ```
    - Open firewall port 8080 (To access jenkins from outside of VM)
    ``` bash
      sudo ufs allow 8080
    ```

# First Jenkins Job

- New Itm -> Enter item name: "Hello World" -> click freestyle -> 
    - General
        - description: HelloWorldJob
    - Build
        - Execute Shell
            - Commad: echo "Hello World $( date )"
    - Save
- Build Now (To execute HelloWorld job)
    - Output 
    
    ```
    Started by user admin
        Running as SYSTEM
        Building in workspace /var/lib/jenkins/workspace/HelloWorld
        [HelloWorld] $ /bin/sh -xe /tmp/jenkins10938345257485883820.sh
        + date
        + echo Hello World Mon Nov 21 10:50:23 IST 2022
        Hello World Mon Nov 21 10:50:23 IST 2022
        Finished: SUCCESS
      ```
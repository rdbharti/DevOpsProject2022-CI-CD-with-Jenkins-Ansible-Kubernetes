# Integrate Maven With Jenkins

1. Setup Maven On the Jenkins Server
2. Setup Environment Variable
3. Install Java (If not installed)
   1. JAVA_HOME
   2. M2 (Maven Environment Varibale)
   3. M2_HOME (Maven Environment Varibale)
4. Install Maven Plugin on Jenkins GUI
5. Configure Maven and Java on jenkins GUI

[Refer Maven Official Documentation](https://maven.apache.org/install.html)

1. Download Maven: [apache-maven-3.8.6-bin.tar.gz](https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.tar.gz)
```bash
cd /opt
wget https://dlcdn.apache.org/maven/maven-3/3.8.6/binaries/apache-maven-3.8.6-bin.tar.gz
tar -xvzf apache-maven-3.8.6-bin.tar.gz
mv apache-maven-3.8.6 maven
cd maven/bin
cd .mvn -v

# Output
# Maven home: /opt/maven
# Java version: 11.0.17, vendor: Ubuntu, runtime: /usr/lib/jvm/java-11-openjdk-amd64
# Default locale: en_IN, platform encoding: UTF-8
# OS name: "linux", version: "5.4.0-132-generic", arch: "amd64", family: "unix"

```

2. Set Environment Variable

   - To find java path
   - find / -name jvm : output /usr/lib/jvm
   - ls -l /usr/lib/jvm : java-11-openjdk-amd64

```bash
# edit /root/.bash_profile else /root/.profile
M2_HOME=/opt/maven
M2=/opt/maven/bin
JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:$M2_HOME:$M2:$JAVA_HOME

export PATH
```
- Check the Path: echo $PATH
- output: ` /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/opt/maven:/opt/maven/bin:/usr/lib/jvm/java-11-openjdk-amd64 `
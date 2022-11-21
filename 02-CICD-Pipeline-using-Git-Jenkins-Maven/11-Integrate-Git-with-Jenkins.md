# Integrate Git With Jenkins

1. Install Git on Jenkins Instance
2. Install GitHub Plugin on Jenkins GUI
3. Configure Git on jenkins GUI

1.  Install Git
  ```bash
  yum install git -y
  ```

2. 
   1. Install github plugin: 
      1. Dashboard -> Manage Jenkins -> Manage Plugins -> Available -> Search: github -> Install without restart
   2. Configure Jenkins:
      1. Dashboard -> Manage Jenkins -> Global Tool Configuration -> Under Git
         1. Name: Git
         2. Path to git executable: If git is installed in PATH then "Git"; \ IF git not installed in the PATH then run  on Linux Server run "Whereis git" -> /usr/bin/git
   3. Save.

# Pull Code From GitHub
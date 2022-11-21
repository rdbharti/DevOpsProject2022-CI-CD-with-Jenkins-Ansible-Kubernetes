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
1. Dashboard -> new item -> JobName/ItemName: "PullCodeFromGithub" -> Freestyle project
   1. Discription: Pull Code From Github
   2. Source Code Management: Git
      1. Repository URL: https://github.com/rdbharti/hello-world.git
   3. Apply and Save
2. DashBoard -> Build Now -> ConsoleOutput:
   
```bash
Started by user admin
Running as SYSTEM
Building in workspace /var/lib/jenkins/workspace/PullCodeFromGithub
The recommended git tool is: NONE
No credentials specified
Cloning the remote Git repository
Cloning repository https://github.com/rdbharti/hello-world.git
 > git init /var/lib/jenkins/workspace/PullCodeFromGithub # timeout=10
Fetching upstream changes from https://github.com/rdbharti/hello-world.git
 > git --version # timeout=10
 > git --version # 'git version 2.17.1'
 > git fetch --tags --progress -- https://github.com/rdbharti/hello-world.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git config remote.origin.url https://github.com/rdbharti/hello-world.git # timeout=10
 > git config --add remote.origin.fetch +refs/heads/*:refs/remotes/origin/* # timeout=10
Avoid second fetch
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
Checking out Revision 2bdda1eb6fa3d90ae8a5c09e0d5e1f9f8f46d59e (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 2bdda1eb6fa3d90ae8a5c09e0d5e1f9f8f46d59e # timeout=10
Commit message: "Update README.md"
First time build. Skipping changelog.
Finished: SUCCESS
```

Note the 3rd line of Output: `Building in workspace /var/lib/jenkins/workspace/PullCodeFromGithub`
its the workspace where git repo has been pulled.
# Automatic Build and Deploy using Poll SCM

Goto 
- Dashboard -> Job Name -> Configure
- Build Triggers: 
  - Check Poll SCM: Periodically check the repository for changes, If change is detected then only Build executes.
  - Put: * * * * * 
  - This will check every minute for a change.

Edit index.jsp file and Push it to repo.
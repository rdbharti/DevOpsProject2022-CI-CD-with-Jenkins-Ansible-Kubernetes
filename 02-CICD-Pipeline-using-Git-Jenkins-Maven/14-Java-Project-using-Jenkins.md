# Java Project Using Jenkins

- Dashboard -> New Item: FirstMavenProject -> Click Maven Project -> OK
- Genereal: 
  - Description: First Maven Project
  - SourceCode:
    - Git:
      - RepositoryURL: https://github.com/rdbharti/hello-world.git
      - Build:
        - Root POM: pom.xml
      - Goals : [Refer](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html)
        - clean install
      - Apply and Save
- Build
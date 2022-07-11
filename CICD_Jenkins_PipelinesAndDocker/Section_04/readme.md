# Section04: Infrastructure as code and automation

## 14. Introdution to Infrastructure as code and automation

- Jenkins allows you to use a web UI to input all the build parameters
- This leads to:
    - No proper audit trail
    - No easy history of changes
    - Segregation between Jenkins admins and developers
        - Users (often developers) will have to contact a Jenkins administrator to make changes
        - Long lead times for changes
    - Difficult to backup and restore (e.g how to restore one setting to how it was the day before)

- The solution is to completely write jobs as code and save it in version control
    - Version Control (git/ subversion/ mercurial) gives you a history and audit trail
    - Easy roll back to older versions of jobs and build instructions
    - Allows operations to give more control to the developers
        - Developers can bundle the jenkins build instructions with their own project repository (e.g. a Jenkinsfile in their project directory)
        - This is what a company that wants to embrace DevOps should do: allow developers to control their own builds

## Job DSL and Jenkins Pipelines

- In the next sections, I'll cover 2 Jenkins capabilities to enable Jenkins to be a true DevOps tool within your software organization:
    - Jenkins Job DSL
    - Write code that creates and modifies jenkins jobs automatically
- Jenkins Pipeline (Jenkinsfile)
    - Bundle the build parameters within the project
    - Allow developers to change jenkins build parameters
    - Enable audit trail, history, rollbacks using version control





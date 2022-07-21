# Section 06: Jenkins Pipelines

## 18. Jenkins Pipelines Introduction

- Jenkins Pipelines allow you to write the Jenkins build steps in code

    - 1) Build steps allow you to write build (compile), test, deploy in code
    - 2) Code means you can put this code in version control
    - It's about automating this cycle:
    i.e: Developer --> Build --> Test --> Release --> Provision & Deploy --> Customer

## 19. Jenkins Pipelines vs Jenkins Job DSL

- How are Jenkins Pipelines different than the Jenkins Job DSL?

    - They both have the capability to write all your CI/CD in code.
    - The difference is in implementation in Jenkins
    - Jenkins Job DSLs create new jobs based on the code you write
    - The Jenkins Pipelines is a job type, you can create a Jenkins pipeline job that will handle the build/test/deployment of one project

### Jenkins Pipelines vs Job DSL

- Can I now using pipelines and forget everything about Jenkins Job DSL?
    - You could use Jenkins Job DSLs to create new pipeline jobs
    - Until now we've only created freestyle projects with the Jenkins Job DSL
    - Another possibility would be to use an "Organization folder", which is a feature of Jenkins Pipelines to detect the project repositories,
    the need to add new jobs.

### Jenkins Pipelines

- The pipeline is a specific job type that can be created using the Jenkins UI or the Jenkins Job DSLs
- You can choose to write the pipeline in Jenkins DSL (declarative pipeline) or in groovy (scripted pipeline)
    - Groovy is a scripting language for the java platform, syntactically very similar to Java and it runs in the JVM (Java Virtual Machine)
    - Under the hood the Jenkins DSL is interpreted by groovy.

### Jenkins Pipelines Example

Code Example

node {
   def commit_id
   stage('Preparation') {
     checkout scm
     sh "git rev-parse --short HEAD > .git/commit-id"                        
     commit_id = readFile('.git/commit-id').trim()
   }
   stage('test') {
     nodejs(nodeJSInstallationName: 'nodejs') {
       sh 'npm install --only=dev'
       sh 'npm test'
     }
   }
   stage('docker build/push') {
     docker.withRegistry('https://index.docker.io/v2/', 'dockerhub') {
       def app = docker.build("wardviaene/docker-nodejs-demo:${commit_id}", '.').push()
     }
   }
}

- Node: influence on what jenkins worker node the job will be run (here: any node)
- def: allows you to declare variables
- Stage: defines a building stage: build, test, or deploy
    - Conceptually distinct step
    - Used by other plugins in Jenkins later on to visualize the stage of a job.
        - e.g: clean -> build -> test -> publish

## 21. Demo: Jenkins pipelines with NodeJS and Docker

Pre-Requiste: https://github.com/marodrigues20/docker-demo 

Description: The idea we have one repo project and in the project we have the pipeline file. Often called Jenkins File.
And that Jenkins file will then have the steps to build this project in Jenkins. So then you only need one project, one git repository,
that will have the code of your project and also your code to build your project. We put the Jenkins File in the root of Project or in
misc folder in root of Project.

Note: https://github.com/marodrigues20/docker-demo/tree/master/misc

Object: Build NodeJs project, package it in Docker and publish it to docker hub.


How to Create a Pipeline.

Main Scenario:

    1) Open the Jenkins http://46.101.41.106:8080
    2) Click on "New Item" menu item.
    3) Fill in the "Enter an item name" field. Add this name: "nodejs docker pipeline".
    4) Select the Pipeline option.
    5) Click on "OK" button.
    5) The Jenkins open a new web page to complete the form.
    6) Go under "Pipeline" section.
    7) Select "Pipeline script from SCM" in SCM dropbox
    8) Select "Git" in "SCM" dropbox.
    9) Add the repo path in "Repository URL" field.
        i.e: https://github.com/marodrigues20/docker-demo.git
    10) Add the path where is the jenkinsfile in "Script Path" field.
        i.e: misc/Jenkinsfile
    11) Click on "Save".
    12) Jenkins show a new web page that contain a new Jenkins Job Pipeline.
    12) Click on "Build Now" menu item.
    13) Click on "Jenkins Job Id" 
    14) Click on "Console Output".
    15) Jenkins Console Output show a msg: SUCCESS
    16) End.

## 22. Build, test, and run everything in Docker

### Docker Pipeline plugin

- The docker plugin, let's you spin up any container within your pipeline.
  - I just used the same plugin in the previous demo, to build/push images.
- You can not only build new containers, but also run existing containers
  - This is useful for when you don't want to bundle development tools with your production container, but you still want to run all stages in an isolated environmenet.
  - You can build/test your application first with an existing container with all the development tools.
  - As a next step, you can build a new container, much tinier, with only the runtime environment.


### Docker Pipeline plugin 2

- Spinning up new docker containers lets you bring in any new tool, easily.
  - You can specify exactly what dependencies you want, at any stage in the job.
  - You can start a database during the test stage to run tests on.
  - After the database tests have been concluded, the container can be removed, together with all the data.
  - Next time you run a new database container during tests, you'll have a brand new container again.
  - This also works for multiple builds at the same time (think multiple git branches), every build has its own database container.

## 23. Demo: Build, test, and run everything in Docker containers.





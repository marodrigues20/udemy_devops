# Section 05: Jenkins Job DSL

## 15. Introduction to Jenkins Job DSL

- The Jenkins Job DSL is a plugin that allows you to define jobs in a programmatic form with minimal effort.
- DSL stands for Domain Specific Language
    - Think about Groovy as a scripting language for the Java platform.
    - It's similar to java, but simpler, because it's much more dynamic

- The Jenkins Job DSL plugin was designed to make it easier to manage jobs.
    - If you don't have a lot of jobs, using the UI is the easiest way.
    - When the jobs grow, maintaining becomes difficult and a lot of work.
- The Jenkins Plugin solves this problem, and you get a lot of extra benefits:
    - Version control, history, audit log, easier job restore when something goes wrong.


- Jenkins Job DSL

job('NodeJS example')
    scm{
        git('git://github.com/marodrigues20/docker-demo.git') { node -> 
            node / gitConfigName('DSL User')
            node / gitConfigEmail('jenkins-dsl@newtech.academy')
        }
    }

    triggers {
        scm('H/5 * * * *')
    }

    wrappers {
        nodejs('nodejs') // This is the name of the NodeJS installation in 
    }                    // Manage Jenkins -> Configure Tools -> NodeJS Installations -> Name

    steps {
        shell("npm install")
    }


## 16. Demo: Jenkins Job DSL with NodeJS application

Scenario:
    1) Open the Jenkins http://46.101.41.106:8080
    2) Go to Dashboard
    3) Click on Manage Jenkins
    4) Click on Manage Plugins
    5) Select "Available" tab
    6) Search by "Job DSL"
    7) Check the checkbox.
    8) Click on "Download now and install after restart"
    9) Jenkins will restart
    10) Click on "New Item" menu item
    11) Type in "Enter an item name" field and type "seed project".
    12) Select "Freestyle project"
    13) Click on "OK" Button
    14) Click on "Source Code Management"
    15) Select "Git" radio button
    16) Type in "Repository URL" the repo where contains the DSL.
    17) Go to "Build" Section
    18) Click on "Add Build Step" dropbox.
    19) Select "Process Job DSLs"
    20) Select "Look on Filesystem"
    21) Copy from github repo the subfolder and your groovy file.
    22) Paste "DSL Script" like this "job-dsl/nodejs.groovy".
    23) Click on "Save" button.
    24) Click on "Run Now"
    25) Go to Console Output
    26) Jenkins shows the msg: "ERROR: script not yet approved for use"
    27) Go to "Manage Jenkins"
    28) Click in "In-Process Script Approval"
    29) Click on the button "Approve".
    30) Go to "seed project".
    31) Click on "Build Now" 
    32) Go to "Console Output"
    33) Jenkins shows msg: "Finished: SUCCESS"
    34) End.

## 17. Demo: Jenkins Job DSL with docker build and publish

In this demo we will add this new DSL: https://github.com/marodrigues20/jenkins-course/blob/master/job-dsl/nodejsdocker.groovy
in "seed project" created previously.

All documentation related to: Job DSL Plugin is here: https://jenkinsci.github.io/job-dsl-plugin/

Scenario:

    1) Open the Jenkins http://46.101.41.106:8080
    2) Open "seed project" created.
    3) Click on "Configure" item menu.
    4) Click on "Build" Tab.
    5) Copy bellow: job-dsl/nodejs.groovy the following code job-dsl/nodejsdocker.groovy copied from the same git repo: https://github.com/marodrigues20/jenkins-course/blob/master/job-dsl/
    6) Click on "Save" Button.
    7) Go to Home Jenkins console.
    8) Open "Manage Jenkins" in Item Menu
    9) Click on "Manage Credentials" option.
    10) Jenkins open a Credentials web page.
    11) Click on: Jenkins under Credentials -> Global Credentials under System -> Add Credentials link
    12) Jenkins open a form:
        Scope: 
        UserName:
        Password:
        ID:
        Description:
    13) Fill in the Form in step 12.
        UserName: marodrigues20
        Password: <My Docker Hub passoword>
        ID: dockerhub --> // get the value from registryCredentials('dockerhub') in nodejsdocker.groovy
        Description: dockerhub
    14) Click on "Save" Button.
    15) Go to "seed project" project
    16) Click on "Build Now".
    17) Go to Console Output
    18) Jenkins shows the msg: "ERROR: script not yet approved for use"
    19) Go to "Jenkins Dashboard"
    20) Click on "Manage Jenkins"
    21) Click on "In-process Script Approval"
    22) Click on "Approve" button.
    23) Go to "seed project" project
    24) Click on "Build Now".
    25) Go to Console Output.
    26) Jenkins shows the msg: "Added items: GeneratedJob{name='NodeJS Docker example'}"
    27) Jenkins created a new project "NodeJS Docker example".
    28) Open the Project "NodeJS Docker example".
    29) Open "Configure" item menu.
    30) Change the account docker in all dockerhub project address. (This need because the instructor didn't do correctly. It's using instructor account in groovy file).
    31) Click on "Build Now"
    32) Go to the Console Output:
    33) Jenkins print out the msg: Finished: SUCCESS
    34) End.
    
    


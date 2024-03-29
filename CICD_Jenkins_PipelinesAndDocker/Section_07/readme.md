# Section 07: Jenkins Integrations

## 24. Email Integration

- The goal is to alert the developer of a broken build as soon as possible.
    - The earlier you give a developer feedback of something that is wrong, the better.
    - This increases the productivity of the developer team tremendously.
        - The longer it takes for a developer to know he needs to fix a bug, the more time it'll take to resolve.
        - The developer will able to fix the code the quickest, when the code a developer wrote is still fresh in his memory.

- Every time when a developer commits a change in version control, the build in jenkins should start
- Changes from version control can either be pulled or pushed
    - Pull: Jenkins pools the version control every x minutes
    - Push: The version control system (e.g. Github, Bitbucket) will send a notification (push) to Jenkins (using http requests)
        - You can use Github Plugin and Bitbucket to configure the push notification


## 25. Email Demo

Pre-Requirement: Register mailtrap.io 

Main Scenario:

    1) Login Jenkins http://<ip_address>:8080
    2) Jenkins shows the Dashboard
    3) Click on "Manage Jenkins".
    4) Jenkins shows the "Manage Jenkins" screen.
    5) Clink on "Manage Plugin"
    6) Click on tab "Installed"
    7) Type "Email" in filter field
    8) Jenkins shows "Email Extension" (A-01)
    9) Click on "Manage Jenkins"
    10) Jenkins shows "Manage Jenkins" page
    11) Click on "Configure System"
    12) Jenkins open the forms
    13) Go Under "Extended E-mail Notification". (Plugin)
    14) Fill in "SMTP server" field.
    15) Add email credentials (Username & password)
    16) Click on "Save" Button.
    17) Jenkins shows "Dashboard" page.
    18) Click on "New Item".
    19) Jenkins shows a new web page.
    20) Fill in "Enter an item name" field with "email test".
    21) Select "Pipeline" job
    22) Click on "OK" button.
    23) Jenkins shows a new "Pipeline" webpage.
    24) Click on "Pipeline" tab.
    25) Select "Pipeline script from SCM" in "Definition" dropbox.
    26) Select "Git" in SCM dropbox.
    27) Fill in "Repository URL" with "https://github.com/marodrigues20/jenkins-course.git"
    28) Fill in "Script Path" field with "email-notifications/Jenkinsfile"
    29) Under "Build Triggers" check "Poll SCM".
    30) Fill in "Schedule" with "H/5 * * * *". (every 5 minutes)
    31) Click on "Save" button.
    32) Jenkins shows "email test" web page.
    33) Click on "Build Now"
    34) Jenkins send an email.
    35) End.

Alternative Scenario


A-01 - Plugin Not Installed
    1) Jenkins doesn't show "Email Extension"
    2) Click on "Available" tab.
    3) Type "Email" filter.
    4) Jenkins shows "Email Extension"
    5) Check it
    6) Click on "Install and Restart" button.


## 26. Slack Integration

- Slack is a chat and collaboration tool, comparable with Atlassian's HipChat
- It's a new and popular team communication tool, that is much better than using Skype or older enterprise chat tools for collaboration
- Slack (or hipchat) is better than its competitors, because it allows you to integrate all your tools within slack, to avoid switching between apps.
    - It can give you a realtime "war room" to collaborate on problems
    - This is also called "ChatOps"
    - If you're familiar with IRC (Internet Relay Chat), It's possible that you already do this since the nineties.

- ChatOps is a collaborative mode that connects people, tools, process, and automation in a transparant workflow.
- It allows you to do conversation driven collaboration, while having your tools integrated and keeping you up to date of the state of your systems.

- For example:
    - You're in a team chat with 5 people, when the following happens:
        - 10:05 AM: You receive a message in the chat that a backend service is malfunctioning.
        - 10:05 AM: OpsGenie (a paper tool) puts a message on the chat that Bob has been paged.
        - 10:10 AM: OpsGenie sends a message that Chris has been pages, because Bob hasn't been responding.
        - 10:11 AM: OpsGenie puts another message on the chat that Chris has acknowledged the problem.
        - 10:15 AM: You tell Chris on the chat that the backend service has been misbehaving since yesterday and that it might be because
          of a new experimental feature that has been pushed late last nigh.
        - 10:15 AM: Chris acknowledges and says he'll look for the cause.
        - 10:45 AM: Chris finds a solution for the problem and commits his changes
        - 10:45 AM: Bitbucket puts a message in the room that a code change has been commited and pushed to the git repository.
        - 10:55 AM: Jenkins notifies the channel that a new build has been pushed to the staging server.
        - 10:56 AM: Jenkins notifies the channel that the staging version was promoted to production.
        - 11:00 AM: The monitoring system puts a message on the channel that all system are green again.
        - 11:30 AM: Bob comes back from a meeting and sees the history of the chat, the actions that have been taken, the commit that been done and
          that all systems are OK again.
        - 12:00 PM: The rest of the enginners see the same history, everyone is up-to-date with what happened that morning.


- This is an example of how ChatOps could work, relieving the team of a lot of inefficient emails that would've been send around.
- If you want to use ChatOps, your tasks now is to integrate Jenkins with your collaboration tool, in a way that you can see the relevant messages in your channel.
- I'll show you how to do this with Slack


## 27. Demo: Slack Integration

Pre-Requisites:
    1) You have to have a slack account
    2) You have to have a slack Team

Jenkins Main Scenario:
    1) Open Jenkins Dashboard (http://46.101.41.106:8080)
    2) Click on Manage Jenkins
    3) Click on Manage Plugins
    4) Click on Tab Available
    5) Select on Filter field
    6) Type "Slack"
    7) Select "Slack Notification Plugin"
    8) Click on "Download and Install after restart"
    9) Jenkins shows a new page.
    10) Select "Restart Jenkins when installation is complete and no jobs are running"
    11) Jenkins redirect to another page. Msg = Please wait while Jenkins is restarting.
    12) Please wait while Jenkins is restarting
    13) End.

Slack Main Scenario:

    1) Open Slack
    2) Click on "Browser Slack" menu.
    3) Click on "Apps"
    4) Slack open Apps page
    5) Search for "Jenkins"
    6) Click on "Add" button in Jenkins App.
    7) Slack open on the web browser "Slack App Directory"
    8) Click on "Add to Slack"
    9) Back to Slack open Apps page
    10) Search for "Incoming WebHooks"
    11) Click "Add" Button
    12) Slack open on the web browser "Slack App Directory"
    13) Click on "Add to Slack"
    14) Select in Post to Channel the channel you'd like to receive a message.
    15) Click on "Add Incoming WebHooks integration"
    16) Slack shows "Slack app directory" for Incomming WebHooks.

Jenkins Main Scenario 2:
    
    1) Open Jenkins Dashboard (http://46.101.41.106:8080)
    2) Click on "Manage Jenkins" menu item.
    3) Jenkins shows "Manage Jenkins" web page
    4) Select "Configure System"
    5) Jenkins open a new page where contains forms
    6) Search for "Slack" Form.
    7) Click "Add Credentials"
    8) Jenkins open a pop up form
    9) Select in "Kind" drop box the value "Secret text"
    10) Fill in "ID, Description" using any value.
    11) Copy from (Slack Main Scenario) on step 16 the secret. Like this:
        "T03UNQSEL9Y/B03ULH5B4LV/NSUug6yhfkAxdnXXGXJ8OSGq" in Webhook URL field.
    12) In step 10 add as a secrete the value retrieved from previous step.
    13) Copy the base Url in Workspace field. Like this: https://hooks.slack.com/services/ from step 16.
    14) Create a new pipeline
    15) Use this this script: https://github.com/wardviaene/jenkins-course/blob/master/slack-notifications/Jenkinsfile

Note: This script is out-of-date because the "Incoming WebHooks" is depricated.


## 28. GibHub and BitBucket integration

- Up until now I've always added git repositories manually
- If you have a lot of respositories, you don't want to add every single repository manually.
- It is then more interesting to have Jenkins auto-detect new repositories
- A developer could then create a new repository for a new (micro) service, add a Jenkinsfile,
and the project will automatically be built in Jenkins.

- The implementation differs from which version control provider you're using.
    - When using GitHub: the GitHub Branch Source plugin will scan all the branches and repositories
    in a GitHub organization and build them via Jenkins Pipelines.
    - When using Bitbucket: the Bitbucket branch source plugin can scan team/project folders and 
    automatically projects.

## 29. Demo: GitHub integration with Gradle +  Java Project


Main Scenario:

    1) Open Jenkins Dashboard
    2) Click on "Manage Jenkins" menu item.
    3) Jenkins open a new screen.
    4) Click on "Plugin Manager"
    5) Click on "Available" tab.
    6) Filter by filter field. Type: github
    7) Select "Github Branch Source Plutin"
    8) Click on "Download and Install button"
    9) Jenkins restart.
    10) Click on "New Item" menu item.
    11) Type "my github repo" in "Enter an item name" field.
    12) Select "GitHub Organization".
    13) Click on "Ok" button.
    14) Jenkins open a new form page.
    15) Go Under Project "GitHub Organization".
    16) Click on "Add Credentials" button.
    17) Jenkins open a pop up form.
    18) Main Scenario GitHub. 
    19) Type the github username.
    20) Type the github password. Use the token from step 18.
    21) Click on "Add" Button.
    22) Select Scan Credentials dropbox.
    23) Type your github login in "Owner" field.
    23) Click on "Save" Button.
    24) Jenkins Scan the Organization from Github checking for Jenkinsfile. (https://github.com/marodrigues20/gs-gradle)
    25) Open the jenkins terminal machine.
    26) Create a directory typing: $ mkdir -p $HOME/.m2
    27) Type: $ chown 1000:1000 $HOME/.m2
    28) Go to Jenknins Web Page
    29) Go to "my github repo" folder
    30) Click on "gs-grandle" repository
    31) Click on "Master" Branch.
    32) Click on "Build Now" menu item.
    33) Click on "Console Output"
    34) Jenkins prints out "Build Successful" msg.
    35) End.


Main Scenario GitHub
    1) Open GitHub web site.
    2) Click on "Settings" item menu.
    3) Click on "Personal Access Tokens" menu item.
    4) GitHub open a new page.
    5) Click on "Generate a new token" button.
    6) Check "Repo" checkbox
    7) Type: "jenkins" in "Token Description" field.
    8) Click on "Generate Token"
    9) Jenkins open a new page with the token generated.
    10) Copy the token.
    11) End.


## 30. Demo: Bitbucket integration

Pre-Requisite: 29. Demo: GitHub integration with Grandle + Java Project

Main Scenario:

    1) Open Jenkins Dashboard
    2) Click on Plugin Manager
    3) Click on Tab "Available"
    4) Type in Filter field: bitbucket
    5) Jenkins shows the filter result on screen.
    6) Select "Bitbucket Branch Source Plugin"
    7) Click on "Download now and Install after restart"
    8) Jenkins shows "Installing Plugin/Upgrandes"
    9) Select "Restart Jenkins when installing is complete and no jobs are running"
    10) Jenkins returns from restart process.
    11) Click on "New Item".
    12) Jenkins shows a new screen
    13) Select "Bitbucket Team/Project"
    14) Type "myTeam" in "Enter an item name" field.
    15) Click on "OK" Button.
    16) Jenkins shows a new form page.
    17) Type in "Owner" field your teamID from Bitbucket.
    18) Click on add button.
    19) Select dropbox scope: Global(Jenkins, nodes, items, all child items, etc)
    20) Main Scenario Bitbucket. 
    21) Type your bitbucket username.
    22) Type your password copied from step 20.
    23) Type in ID field: bitbucket-api
    24) Click on "Save" Button.
    25) Click on "Configure System" menu item.
    26) Jenkins open "Configure System" page.
    27) Jenkins URL is set correctly usign the same jenkins ip.
    28) Click on "Save" button.
    29) Jenkins run automatically the pipeline.
    30) Jenkins shows "Success" msg.


Post-Conditions: Jenkins scam automatically any new repo in bitbucket.


Main Scenario Bitbucket:
    1) Open Bitbucket website.
    2) Go to settings.
    3) Click on "App Password" menu.
    4) Bitbucket open a new screen.
    5) Click on "Create app password" button.
    6) Bitbucket open a new screen.
    7) Type a label.
    8) Select all reads access on the page.
    9) Click on "Create" button.
    10) Copy the password
    11) End.


## 31. JFrog Artifactory integration

- In the previous demos I've always submitted the Docker image to the docker registry (Docker Hub)
- This resulting image is in Jenkins called "The artifact"
- It's the resulting binary from a build
- It can be a docker image, or a .jar file, a .tgz/.zip file, really anything.
- These artificats, the result of your build, you want to store somewhere.
- JFrog Artificatory is a product that can store for you the artifacts resulting from a build.

- You can either download Artificatory fro free and run it yourself, or you can use their hosted version.
- In the demo I'll use the hosted version
- It's bet practice to store all the artificts of the builds that are getting deployed.
    - If you need to roll back, you have the artifact already available
    - You are 100% sure about the binary if you're promoting the same version from dev to staging or from 
    staging to production.

- JFrog integration is done using a JFrog Plugin
- The JFrog plugin allows you to add extra steps to your Jenkinsfile
  - In this step, you can put a conditional, to only do this for develop/master branch, not for feature branches.


## 32. Demo: JFrog Artifactory integration

Pre-Requesites: 1) JFrog Account
                2) User Name and Password to be used in Jenkins 
                3) Set permissions for that particular user. Create a group and give permission to Deploy/Cache for that particular group.
                4) Set up a type of repository. (Gradle, Maven, Docker, npm and etc.) - This example is being used Grandle 

Main Scenario Jenkins - JFrog Plugin:

    1) Open the Jenkins Console.
    2) Click on "Manage Jenkins" menu item.
    3) Click on "PLugin Manager" menu item.
    4) Click on  "Available" tab.
    5) Type "artifactory Plugin" in Filter field.
    6) Jenkins shows the result of filter.
    7) Check "Artifactory Plugin" check box.
    8) Click on "Download now and Install after restart" button.
    9) Jenkins shows a new page.
    10) Select "Restart Jenkins when installation is completed and no jobs are running"
    11) Jenkins Restart and Install the Plugin.
    12) Go to JFrog and set up and get the data required.
    13) Go to Jenkins Console
    14) Click on "Manage Plugins"
    15) Jenkins open a new page.
    16) Go to Artifactory Section.
    17) Click "Add Artifactory Section"
    18) Check "Use the Crendentials Plugin" check box.
    19) Click on "Add" credentials button.
    20) Add credentials with kind "Username and password"
    21) Add user jfrog in username field.
    22) Add password from the user. (Create API Key in JFrog and paste in this field)
    23) Add the server id like: <repo>.jfrog.io
    24) Add URL liek https://<repo>.jfrog.io
    25) Select the credentials created in step 19 using dropbox.
    26) Click on "Test Connection" button.
    27) Jenkins Validate the Connection
    28) Click on "Save" button
    29) Jenkins display the Console page.
    30) Click on "Manage Jenkins" menu item.
    31) Click on "Global Tool Configuration".
    32) Jenkins shows a new page.
    33) Go to "Grandle" section.
    34) Type "granle" in name field.
    35) Select Grandle Version from dropbox field.
    36) Click on "Save" botton.
    37) Click on "Item Menu".
    38) Jenkins display a new page.
    39) Type "Grandle Publisher test" in "Enter an item name" field.
    40) Click on "Pipeline" item.
    41) Click on "OK" Button.
    42) Jenkins open a new page.
    43) Go to pipeline section.
    44) Select "Pipeline script from SCM" in "Definition" field.
    45) Select "Git" item in SCM field.
    46) Paste in "Repository URL" https://github.com/wardviaene/jenkins-course
    47) Paste in "Script Path" tree/master/jfrog-integration
    48) Click on "Save" Botton.
    29) Jenkins display a new page.
    30) Click "Build Now" menu item.
    31) Jenkins Run a new Job.
    32) Jenkins shows "Success" msg.
    33) End.

Post-Conditions: Artifact uploaded in JFrog


Main Scenario Jenkins - Docker:

    1) Open jenkins Console
    2) Click on "Menu Item"
    3) Type: "Gradle publisher test with docker"
    4) Select "Pipeline"
    5) Click "OK" Button
    6) Jenkins job shows a new page
    7) Go to Pipeline section.
    8) Select "Pipeline script from SCM"
    9) Select "Git" in SCM field.
    10) Paste the "Repository URL": https://github.com/marodrigues20/gs-gradle.git
    11) Paste in "Script Path" field: Jenkinsfile.withfrog
    12) Click "Save" button.
    13) Jenkins display a new page.
    14) Click on "Build Now" menu item.
    15) Jenkins display a new Job Jenkins
    16) Jenkins shows "Success" Message.

Post-Conditions: Artifact uploaded in JFrog


## 33. Custom API Integration

- Sometimes you want to integrate an API, but there is no plugin available
- Even if there is a plugin available, it can lack the features you want
    - You might want to get more information from an endpoint (e.g. the full JSON from the request)
    - You want to do a POST or PUT request on the API
- One solution is to write your own Jenkins Plugin
    - But this requires a lot of effort
    - And is not always feasible, as a developer, you might not have access to Jenkins Plugins.

- Another solution is to use functionality in the Jenkins pipelines to do http requests
- You only need one plugin: the http requests plugin
- It's up to you (the developer/SysOps/DevOps) to write the code to do the requests and interpret them
- This is where groovy becomes handy, because groovy allows you to do scripting within the Jenkinsfile

## 34. Demo: Custom API Integration

Main Scenario:
    1) Open Jenkins Dashboard
    2) Click on "Manage Jenkins" menu item
    3) Jenkins open "Manage Jenkins" web page.
    4) Click on "Plugin Manager" menu item
    5) Jenkins open "Plugin Manager" web page.
    6) Click on tab "Available"
    7) Type "http request" in filter field.
    8) Jenkins shows a list of plugins.
    9) Cleck "Http Request Plugin" 
    10) Click on "Download now and install after restart" button.
    11) Check "Restart Jenkins when installation is complete and no jobs are running" option.
    12) Jenkins start instaling the plugin
    13) Start Main Scenario - Create API Key in Bitbucket
    14) Open the Jenkins Dashboard
    15) Click on "Credentials" menu item.
    16) Click to add a new credential
    17) Add username and password retrieved from step 13.
    18) Type "bitbucket-oauth" in ID and Description field.
    19) Click "OK" Button.
    20) Go to "New Item" menu item.
    21) Type "custom api" in "Enter an item name" field.
    22) Select "Pipeline" option.
    23) Click on "OK" Button.
    24) Jenkins open a new web page.
    25) Go to "Pipeline" section
    26) Select "Pipeline script from SCM" dropbox definition field.
    27) Select "Git" item in SCM dropbox
    28) Type in "Script Path" field "/customapi-integration/Jenkinsfile"
    29) Type https://github.com/wardviaene/jenkins-course.git in "Repository URL" field.
    30) Click on "Save" button.
    31) Jenkins open "Pipeline custom api" pipeline web page.
    32) Click on "Build Now" menu item.
    33) Jenkins shows a error msg "Scripts not permitted to use ...."
    34) Got to Jenkins Dashboard.
    35) Click on "Manage Jenkins" menu item.
    36) Jenkins open "Manage Jenkins" page.
    37) Click on "In-Process Script Approval" menu item.
    38) Jenkins open "In-Process Script Approval" page.
    39) Click on "Approval" button.
    40) Go to Jenkins Dashboard.
    41) Go to Jenkins "Pipeline custom api" pipeline.
    42) Click on "Build Now"
    43) Go to console output
    44) Jenkins shows a error msg "Scripts not permitted to use ...."
    45) Repeat step 31 till 40.
    46) Jenkins shows the success msg.
    47) End.
    
    
Important: IF YOU WANT TO KNOW THE PIPELINE SINTAX. GO TO "PIPELINE IN ADVANCED PROJECT OPTIONS" IN "PIPELINE SECTION"
CLICK ON THE LINK: "Pipeline Sintax". YOU CAN FIND A SNIPPET GENERATOR.


Main Scenario - Create API Key in Bitbucket

    1) Open the Bitbucket console
    2) Login using your user.
    3) Bitbucket open the main page.
    4) Click on "Settings" menu item.
    5) Bitbucket open "Settings" web page.
    6) Click on "OAuth" menu item.
    7) Go to OAuth consumers
    8) Click "Add consumers" button.
    9) Bitbucket open a new web page.
    10) Type "jenkins" in Name field.
    11) Type "http://<Jenkins IP address>:<port> in Callback URL
    12) Type "http://<Jenkins IP address>:<port> in URL
    13) Go to "Permissions" and select read access in "Repositories" and "Pull Requests"
    14) Click "Save" button.
    15) Jenkins open the web page defined in step step 7.
    16) Copy the Key and Secret values generated by Bitbucket.
    17) End.



## 35. Sonarqube integration

- Sonaqube continuously inspect your software project on code quality.
- It can report on:
    - Bugs (code issues)
    - Vulnerabilities (security issues)
    - Code smells (maintainability related issues)
    - Techinical debt (estimated time required to fix)
    - Code coverage (test coverage of code)

- Sonarqube is very popular piece of software than is often integrated with Jenkins
- In Jenkins it's just a build step
    - the code neeeds to be scanned by the sonar-scanner
    - the sonar-scanner sends its results to the Sonarqube server
        - The sonarqube server needs to be installed 
        - Sonarqube server also uses a database to maintain its state

- One possible solution
    - Install database and sonarqube as a docker container on the master node
    - We can use docker-compose to manage the containers
    - Docker-compose is a handy tool from Docker that can spin up containers based on a container definition in yaml format.
    - We'll have 3 containers, a Jenkins container, a database container, and sonarqube

## 36. Demo: Sonarqube installation with docker-compose

Pre-Requisite:  Jenkins running using Docker image
                Docker Compose installed


    Main Scenario:
        1) Open Digital Ocean website
        2) Open the console
        3) Type: $ docker ps
        4) Type: $ docker stop <container>
        5) Copy the content of docker compose file here: https://github.com/wardviaene/jenkins-course/blob/master/docker-compose/docker-compose.yml
        6) Click on the "Raw" button.
        7) Copy the git URL
        8) Type in Digital Ocean console: $ wget https://raw.githubusercontent.com/wardviaene/jenkins-course/master/docker-compose/docker-compose.yml
        9) Wget command download the docker-compose.yml file
        10) Type: $ mkdir jenkins 
        11) Type: $ mv docker-compose.yml jenkins
        12) Type: $ sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
        13) Type: sudo chmod +x /usr/local/bin/docker-compose
        14) Type: $ docker-compose --version
        15) Go under jenkins folder created in step 10.
        16) Type: $ docker-compose up -d
        17) Type: $ docker ps //You see 3 containers running
        18) Type: $ docker-compose logs -f sonarqube
        19) Type in the browser: <Digital Ocean IP Machine/IP of Jenkins>:9000
        20) The Browser shows SonarQube Main page.
        21) Click on Login Button
        22) Type in UserName: "admin"
        23) Type in Password: "admin"
        24) SonarQube redirect to Home Page.
        25) End.


Note: After Main Scenario Jenkins can push some SonarQube reports to SonarQube Server.

## 37. Demo: SonarQube integration within Jenkins Pipelines


    Main Scenario:
    1) Open Jenkins DashBoard
    2) Click on "Manage Jenkins" menu item
    3) Jenkins open "Manage Jenkins" web page.
    4) Click on "Plugin Manager" web page.
    5) Jenkins open "Plugin Manager" web page.
    6) Type "sonarqube" Filter field.
    7) Jenkins shows the plugins availbles.
    8) Check "SonarQube Scanner for Jenkins" checkbox.
    9) Click on "Download now and install after restart" button.
    10) Jenkins open a new page and start download and install the plugin.
    11) Check "Restart Jenkins when installation is complete and no jobs are running"
    12) Jenkins restart.
    13) Main Scenario SonarQube.
    14) Open Jenkins Dashboard.
    15) Click on "Manage Jenkins" menu item.
    16) Jenkins open "Manage Jenkins" web page.
    17) Click on "Globa Tool Configuration" menu item.
    18) Jenkins open a new page.
    19) Go to SonarQube Scanner
    20) Click on "SonarQube Scanner".
    21) Jenkins shows a new form.
    22) Type in name field "sonar".
    23) Choose SonarQube version in Version dropbox.
    24) Click on "Save" Button.
    26) Jenkins shows a dashboard page.
    27) Click on "Credentials" menu item.
    28) Click in one of credentials narrow in domain collumn.
    29) Jenkins shows a button "Add credentials"
    30) Click on "Add credentials" Button.
    31) Jenkins shows a new page.
    32) Select "Secret file" in Kind field.
    33) Paste the token copied in step 13 in Secret field.
    34) Type "sonar" in ID and Description fields.
    35) Click on "OK" Button.
    36) Go to Jenkins DashBoard.
    37) Click on "New Item" menu item.
    38) Type "sonarqube" in "Enter an item name" field.
    39) Select "pipeline" option.
    40) Click on "OK" Button.
    41) Jenkins shows a new page.
    42) Go to "Pipeline" section.
    43) Select "Definition" drop box.
    44) Select in SCM field "Git".
    45) Type "https://github.com/marodrigues20/jenkins-course.git" in "Repository URL" field.
    46) Type "sonarqube/Jenkinsfile" in "Script Path" field.
    47) Click on "Save" Button.
    48) Jenkins open dashboard web page.
    49) Open "Pipeline sonarqube".
    50) Click on "Build Now".
    51) Jenkins starts a new Job.
    52) Click on new Job Id.
    53) Click on Console Output.
    54) Jenkins shows "Success".
    55) Go to "Main Scenario SonarQube Result".
    56) End.


    Main Scenario SonarQube:
    1) Open SonarQube
    2) Login using Administrator
    3) SonarQube open the home page.
    4) Go to My Account
    5) SonarQube shows My Account web page.
    6) Click on "Security" tab.
    7) Go to "Generate Tokens".
    8) Type "Jenkins" in "Generate Tokens" field.
    9) Click on "Generate" Button.
    10) SonarQube generate a token.
    11) Copy the Token generated.
    12) End.

    Main Scenario SonarQube Result:
    1) Open SonarQube Home page.
    2) SonarQube shows in "PROJECTS" gs-grandle project.
    3) Click on "gs-grandle" project
    4) SonarQube open a new page with metrics of the project.
    5) End.






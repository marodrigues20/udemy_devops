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



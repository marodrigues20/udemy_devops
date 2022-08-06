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

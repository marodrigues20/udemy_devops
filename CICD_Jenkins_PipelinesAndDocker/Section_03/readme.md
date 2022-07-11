# Section 03: Building a NodeJS app

## 2. Why a NodeJS application app

- Node.js is a javascript runtime environment which is:
    - Open Source
    - Cross-Platform
    - Used to execute javascript code server side instead of client side
    - Has an event-driven anquitecture capable of asynchronous I/O
        - This makes NojeJS capable of responding vary fast to requests
        - e.g. it can immediately give a client the result and asynchronous handle database updates which generally take a longer-time.
        - This is one of the reasons why Noje.js is used so much.

### Node.js is used by

- Paypal (using Noje.js for its customer-facing side of their web applications)
- Linkedin (to deal with scale)
- Uber (to process lots of data quickly, address errors on the fly - without restart, and active community that makes NojeJS better over time)
- Yahoo, Mozila, Netflix, New York Times, Medium


### Why Node.js

- We use a Node.js example project because it's relatively easy to understand, even when you never have used it before.
- It doesn't need to be compiled, so building the app doesn't take a lot of memory (unlike building java projects)
- Which is handy if you just want to do this course on small instances.
- Once you understand how to build a simple node.js project, you can use the knowledment to start building larger, complex projects.


## 11. How to build and deploy a NodeJS app

- Step 1: install dependencies (npm install)
    - Downloads and installs all the dependencies

- Step 2: run tests (npm tests)
    - Runs all the NojeJS tests
    - Typically if a tests fails, the build fails and the developer(s) should be notified.
    - I'll explain about slack/bitbucket integrations later on this course

- After step 1 and 2 you have an application that is ready to be started.

- The only thing you'll be missing is a package: how do you package and distribute this project so it can be deployed on a server instance?

- Step 3: package the app in docker 
    - You can create a container that includes the Noje.js binaries and your project.
    - Rather than creating a .zip, .jar, or .tgz package, you package a binary that includes all binaries and dependencies.
    - That way you're sure that the code will behave the same on the production system as your dev/test/staging/qa machines.
        - It gives you closer dev-prod parity.
    - Step 4: distribute the docker image
        - You can use a docker registry to upload your docker images
        - Those images can be set public (everyone can acess them) or private.
        - Examples: Docker Hub, Amazon AWS's Container Registry


    
## 12. Demo - Building the first application

- We already have a nodejs application developed in https://github.com/marodrigues20/docker-demo

- Scenario:

    1. Open the jenkins console
    2. Click on Manage Jenkins
    3. Jenkins will display a new screen.
    4. Click on Manage Plugin
    5. Click on Available Plugins
    6. Filter by NodeJS Plugin
    7. Select the plugin.
    8. Click on "Download now and Install after restart" button.
    9. Jenkins display anoter screen.
    10. Check the checkbox "Restart Jenkins when installation is complete and no jobs are running
    11. Wait Jenkins restarts
    12. Go to https://github.com/marodrigues20/docker-demo
    13. Click on the button "clone" on github.
    14. Select the http url.
    15. Open the Jenkins console again. http://46.101.41.106:8080/
    16. Click on "Create a new Job".
    17. Fill in "Enter an item name" field. (nodejs example app)
    18. Select "Freestyle project" option.
    19. Click "OK" button.
    20. Jenkins open a new form to fill in manually.
    21. Select "Git" option in "Source Code Management" section.
    22. Copy the url select in step 14 in "Repository URL" field.
    23. Select "Add build step" dropbox "Execute shell".
    24. Type: npm install
    25. Click "Save" button.
    26. Go to Jenkins console root.
    27. Click on "Manage Jenkins".
    28. Click on "Global Tool Configuration"
    29. Click on "Add NodeJS" button.
    30. Fill in "Name" field with "NodeJs"
    31. Select the nodejs version that you want to use.
    32. Click on "Save" button.
    33. Come back to Jenkins job "nodejs example app" screen.
    34. Cick on "Configure".
    35. Provide in "Build Environement" section the nodejs installation set up in step 32.
    36. Click on "Save" button.
    37. Click on "Build Now" link.
    38. Jenkins will create a Job Id.
    39. Click on "Job Id" link.
    40. Click on "Console Output".
    41. You see the build running.
    42. End.


## 13. Demo - building nodejs app with Docker

- Demo Package and push to docker repository

    - Step 3: package the app in docker
        - You can create a container that includes the Node.js binaries and your project
        - Rather than creating a .zip, .jar, or .tgz package, you package a binary that includes all binaries and dependencies.
        - That way you're sure that the code will behave the same on the production system as your dev/test/staging/qa machines.
            - It gives you closer dev-prod parity

    - Step 4: distribute the docker image
        - You can use a docker registry to upload your docker images
        - Those images can be set public (everyone can acess them) or private
        - Examples: Docker Hub, Amazon AWS's Container Registry


- Main Scenario:

    1. Go to Jenkins Dashboard
    2. Click on "Manage Jenkins" menu item.
    3. Click on "Plugin Manager".
    4. Click on "Avilable" Tab.
    5. Type Docker in Find fild.
    6. You are going to see a quite plugins available
    7. Select "CloudBees Docker Build and Publish plugin"
    8. Click on "Download now and install after restart"
    9. Wait Docker install and restart
    10. Jenkins restart.
    11. Jenkins show the Dashboard screen.
    13. Open the github https://github.com/marodrigues20/jenkins-docker
    14. Clone the repo in step 13 
    15. Go to the folder cloned.
    16. Build the dockerimage using: $ docker build -t jenkins-docker .
    17. Stop the old container: $ docker stop jenkins
    18. Delete the old image: $ docker rm jenkins
    19. Check that all the plugins, files of container deleted above is mapped to: $ ls /var/jenkins_home
    20. Run the new docker file cloned in steop 14.
        - $ docker run -p 8080:8080 -p 50000:50000 -v /var/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock  --name jenkins -d jenkins-docker
    21. Run: $ docker ps -a
    22. Run: $ docker exec -it jenkins bash
    23. Run: $ ls -ahl /var/run/docker.sock
    24. Copy the GID
    25. Exit Jenkins Container.
    26. Stop the container.
    27. Delete the images.
    28. Change Line 9 replacing the GID copied in step 24 in Dockerfile cloned in step 13.
    29. Build the image using the Dockerfile changed in step 28.
    30. Run the new docker file cloned in step 29.
        - $ docker run -p 8080:8080 -p 50000:50000 -v /var/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock  --name jenkins -d jenkins-docker
    31. Go into the new container. 
        - $ docker exec -it jenkins bash
    23. Run: $ docker ps
    24. Docker show the container running.
    25. Go to https://login.docker.com
    26. Create a new repository in docker hub using marodrigues20 account.
    27. Create a repos using the name docker-nodejs-demo and public restriction.
    28. Docker hub show you a new docker repo created like this: marodrigues20/docker-nodejs-demo
    29. Copy the docker repo created in step 28.
    30. Go to WebBrowser
    31. Access the Jenkins website: http://<serverip>:8080
    32. Go into your Jenkins Job: "nodejs example app"
    33. Click menu item "Configure"
    34. Click on dropbox "Add build step" under Build.
    35. Select "Docker Build and Publish"
    36. Jenkins will show a form.
    37. Fill:
        - Repository Name: marodrigues20/docker-nodejs-demo
        - Registry credentials: Type your docker username and password.
    38. Click on the Button "Save".
    39. Run the Jenkins Job.
    40. Jenkins starts the job.
    41. Jenkins executed successfuly.
    42. Go to Docker Hub in your account.
    43. Docker hub show your  marodrigues20/docker-nodejs-demo repo updated.
    44. Go to any machine where has docker installed and type:
        - $ docker pull marodrigues20/docker-nodejs-demo
        - $ docker run -p 3000:3000 -d --name my-nodejs-app marodrigues20/docker-nodejs-demo
        - $ docker ps
        - $ curl localhost:3000
        - You see: Hello World
        

    



Hint: If you're not using the same DigitalOcean-Ubuntu setup, make sure to change the "999" on line 9 with the GID (group ID) your docker 
group has on your system. To check run:

$ cat /etc/group | grep docker 
docker:x:998:
or 

$ getent group docker

to find the gid of docker on your system.




srw-rw---- 1 root 998 0 Jun 27 21:41 /var/run/docker.sock
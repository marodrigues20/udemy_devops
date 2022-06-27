# Section 2: Introduction to Jenkins

## 5. What is Jenkins (Part 1)

- Jenkins is an open source continuous integration (CI) and continuous delivery (CD) tool written in Java.
- It's a automation server used to build and deliver software projects
- Jenkins was forked from another project called Hudson, after a dispute with Oracle.
- A major benefit of using jenkins is that it has a lot of plugins available
- There is easier to use CI software avaialble, but Jenkins is open source, free and still very popular.


## What is CI / CD

- Continuous Integration (CI) is the practice, in sotware enginnering, of merging all developers working copies to a shared mainline several times a day.
(Wikipedia)

- Continuous delivery (CD) is a software engineering approach in which teams produce software in short cycles, ensuring that the software can be reliable
released at any time.

- In Practice: verify a publish work by triggered automated builds & tests
    - For every commit or at least once a day
    - All developers should push their changes to a version control, which should then be built and tested - which can be done by Jenkins.
    - Jenkins doesn't merge code nor it resolves code conflicts, that's still for the developer to do (using for instance  git and merge tools)

## Benefits

- Jenkins provides a feedback loop back to the developer to fix fuild errors.
- Research has shown that it's a lot quicker to have a developer fix the error immediately (when the code is still fresh in memory)
- Jenkins can publish every build of your software
    - This build already has gone through automated testing
    - When published and deployed to a dev/qa/stating server, you can advance the Software development lifecycle (SDLC) much quicker.
    - The quicker you can go through an iteration on the SDLC the better 


## 6. What is Jenkins (Part II)

- CI/CD within the SDLC
    image: CI:CD within she SDLC.png


### Jenkins Alternatives

- Self-hosted 
    - Drone CI (Continuous delivery platform written in Go)
    - TeamCity (by Jetbrains)
- Hosted (as a service)
    - Wercker
    - CircleCI
    - CodeShip
    - SemaphoreCI
    - Amazon AWS CI/CD tools


## 7. Jenkins Instation

- Installation methods
    - On the Cloud using docker (works on AWS, DigitalOcean, Gloogle Cloud, Azure)
    - www.virtualbox.org
    - www.vagrantup.com together with virtualbox can spin up a running ubuntu instance in minutes (see my other DevOps course for an introduction to Vagrat)
    - Docker for Windows can run docker on a mac (http://docs.docker.com/docker-for-mac)
    - Jenkins is written in java, so you can also run it without docker on any OS
        - Still I recommended docker for this course
    - Even when using linux and docker, you can still run jenkins slaves on Microsoft Windows to build applications that need a Microsoft Windows OS.


- Create a droples in DigitalOcean
    1) Get the IP
    2) Go to your terminal and type
        $ ssh root@<IPAddress>
    3) Type password: NeverTrust@123w
    4) wget https://github.com/wardviaene/jenkins-course/blob/master/scripts/install_jenkins.sh
    5) $ bash install_jenkins.sh
    6) bash install_jenkins.sh
    7) Select selected jenkins plugins
    8) Create the first admin user:
        - Username: marodrigues20
        - Password: NeverTrust@123w
        - marodrigues20@gmail.com

    9) Instance Configuration:
        - http://46.101.41.106:8080/


## 9. Introduction to Docker

- What is docker?
    - Docker is the most popular container software
    - Docker Engine
        - Docker runtime
        - Software to make run docker images
    - Docker Hub
        - Online service to store and fetch docker images
        - Also allows you to build docker images online
    - Isolation: you ship a binary with all the dependencies
        - No more "It works on my machine, but not in production"
    - Closer parity between dev, QA, and production environments
    - Docker makes development teams able to ship faster
    - You can run the same docker image, unchanged, on laptops, data center VMs, and Cloud providers
    - Docker uses Linux Containers (a kernel feature) for operating system-level isolation.
    - VM vs Containers.png
    - ContainerOnCloudProviders.png


## Why use docker to start Jenkins?

- Everyone will have the same container image of Jenkins
- Regardless of your Operating System
- You can run docker on an ubuntu on DigitalOcean, but also on a Mac, on Windows, etc.
- You can easily upgrade jenkins by pulling the latest container image.
- You can modify the jenkins container image by creating your own Dockerfile.
- Or modify the Dockerfile I have for jenkins in my github repository.








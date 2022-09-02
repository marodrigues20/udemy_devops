# Section 08: Advanced Jenkins usage

## 38. Introduction to Jenkins Slave

- Currently, only one node (one droplet) is hosting the jenkins web UI and doing all the builds.
- In production environments, you typically want to host the Jenkins web UI on a small master node, and have one
or more worker nodes (Jenkins Slaves)
- Using workder nodes, you can easly expand your build capacity
- Typically one worker has one or more build executors (building slots)
    - If a Jenkins node has 2 executors, only 2 builds can run in parallel
    - Other builds will get queued

### Jenkins Slaves

- Static or manual scaling:

    - You can have more workers during working hours (or no workers outside working hours)
    - You can add more workers ad-hoc, when necessary
        - During periods when a lot of code is created
        - During periods when developers have to wait long for their builds to be finished
            - i.e. the jobs stay a long time in the queue

### Jenkins Slaves

- Dynamic workers scaling
    - You have plugins that can scale Jenkins slaves for you

        - The Amazon EC2 Plugin: If your jenkins build cluster gets overloaded the plugin will start new slave nodes automatically using the AWS EC2 API. If after some time the nodes are idle, they'll automatically get killed

    - Docker plugin: This plugin uses a docker host to spin up a slave container, run a jenkins build in it, and tear it down.

    - Amazon ECS Plugin: Same as docker plugin, but the host is now a docker orchestrator, the EC2 Container Engine, which 
    can host docker container and scale out when necessary.

    - DigitalOcean Plugin: dynamically provisions droplets to be used as jenkins slaves

### Jenkins Slaves

- Builds can be executed on specific nodes

    - Nodes can be labeled
        - e.g. "windows64-node"
    - Builds can then be configured to only run on nodes with a specific label
        - This can be configured in the UI or using the Jenkinsfile
            
            ```json
            node(label: 'windows64-node'){
                stage('build'){
                    [...]
                }
            }
            ```


## 39. Jenkins Slaves benefits and best practices

### Jenkins Slaves Benefits

- Reduced cost: only have the capacity you really need
- Slaves are easily replaceable: If a slave crashes, in can be spun up again
- The master can run on a separate node that isn't affected by the CPU/Memory load the builds generate.
    - i.e: the UI will always be responsive
- You can respond to sudden surges in builds, capacity can be added on the fly.
    - Even with manual scaling, you can quickly spin up a new machine as a Jenkins slave.

### Jenkins Slaves

- It's important to standardize your Jenkins slaves
    - Don't manually install tools on the slaves
        - Use Plugins to provide tools (NodeJS, Docker, Java, Maven)
        - Use Docker to provide images that can buid jobs, and use the Docker pipeline plugin to execute builds in a specific docker image.
        - Slave should be disposable, you should be able to throw it away and start it again from scratch.
- There are 2 solutions I'll demo
    - Master node connects to slave over SSH
        - I'll set up a new machine and let the master connect to the slave
    - Slave node connects to master over JNLP
        - The slave will initiate the contact
        - Good solution for windows slaves
        - Useful if the slave is behind a firewall


## 40. Demo - Jenkins slave using SSH

TODO 

## 41. Demo: Jenkins slave using jnlp

TODO

## 42. Blue Ocean

- Blue Ocean is a new frontend for Jenkins

    - Built from the ground up for Jenkins Pipeline
    - Provided as plugin
    - Still compatible with freestyle projects
    - Should eventually replace the normal Jenkins UI over time

### Blue Ocean

- New features:
    - Sophisticated visualizations of the pipeline
    - A pipeline editor
    - Personalization
    - More precision to quickly find what's wrong during an interventation
    - Native integration for branch and pull requests

## 43. Demo - Blue Ocean

Nothing to Write here!

## 44. ssh-agent


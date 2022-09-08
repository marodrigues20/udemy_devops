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

- When you work with Jenkins Slaves, those slaves also need access to repositories.
    - A mistake I often see is that people customize the software on their slave.
    - Private keys and credentials often end up on the slaves.
    - This makes it more difficult to scale out slaves: adding another slaves suddenly means manually
    copying over credentials and private keys.

- For ssh keys, the solution is to use an ssh-agent
- The ssh-agent will run on the master, and will contain the private keys that are necessary to authenticate
to the external system you need access to
    - Predominantly, this is a GitHub/Bitbucket private key to get access to the repositories
- When you need access to a system or a git repository within the Jenkinsfile, you can wrap the ssh-agent around
your code, to be able to authenticate to the systems
    - ssh-agent uses the same keys stored within your credentials

## 45. demo: ssh agent

TODO

## 46. Security best practices

### Security

- Best practices for Jenkins:
    - Try to keep your Jenkins shielded from the internet: using firewall rules and behind a VPN
        - You'll need to whitelist the bitbucket/github IP addresses for pushing requests
        - In the past there were security vulnerabilities discovered that can be exploited without being
        logged in, so it's better to keep Jenkins shielded away from internet.

    - Keep your Jenkins up-to-date
        - Always upgrade to the latest version
            - Use the lts (long term support) edition if you want to be on a stable version.
            - If you're using docker, use the lts or latest tag, and do a docker (or docker-compose)
            pull, and restart the container
            - Also read the Changelog
        - Always keep the plugins up to date

    - Configure authentication / authorization
        - If you don't want to give administrator access to your users, make sure that they can't execute scripts
        that give them elevated permissions.
            - Keep in mind that administrator can decrypt credentials
        - Use Onelogin (SAML) / LDAP / centralized directory for users
        - Don't use weak passwords / logins

## 47. Authentication and authorization

- Authentication:

    - "The process or action of verifying the identity of user or process."
    - Basically, verifying the credentials of the user: the username (or email) and password

- Authorization:

    - "Authorization is the function of specifying access rights to resources
      related to information security and computer security in general and to access control in particular.
    - Once a user is authenticate, what does he have access to?

- I'll cover authentication in the next section, by using Onelogin as our "authentication / identity provider"

- For authorization, you have a few options:
    - Anyone can do anything (not recommended)
    - Anyone who is logged in can do anything
    - Matrix based authorization (just a big table to give users access)
    - Role Strategy plugin (for more granular control, like projects and slave access)

- What do to when you lock yourself out?

    ```
    $ docker stop jenkins
    $ # edit /var/jenkins_home/config.xml
    $ docker start jenkins


    Option 1:
    Make sure you have:
    <useSecurity>false</useSecurity>

    And remove all
    <authorizationStrategy> references

    Options 2:
    <authorizationStrategy
    class="hudson.security.ProjectMatrixAuthorizationStrategy">
    <permission>hudson.model.Hudson.Administer:YOUR-USER</permission>
    </authorizationStrategy>
    ```


## 48. Demo: authorizations

TODO

## 49. Authentication Providers for Jenkins

- Rather than managing users locally, it's best to manage users elsewhere
- All enterprise companies use a central user database, often a directory service like Active Directory or LDAP
    - If you already have this setup, then it's quite easy for you
    - At the "Configure Global Security" page, select LDAP and input the settings provided to you by the AD group
- If you're not in an enterprise environment, you'll either have a setup LDAP yourself, or go for hosted solution

### Security

- One of the best hosted solutions is onelogin.com
- It's used by a lot of companies to manage their users directory
- Onelogin can also be linked to Jenkins, to let onelogin handle the authentication process
    - You link through SAML
        - Security Assertion Markup Language (SAML, pronounced sam-el) is an XML-based, open-standard data format
        for exchanging authentication and authorization data between parties (wikipedia)

- Once SAML has been setup, Onelogin will handle the authentication part
- When you want to add more users, you'll have to add more users to Onelogin
    - Onelogin can provision users from sources, like Google
    - Onelogin also provides multi-factor authentication
- Unfortunately Onelogin is not cheap (and suffered from some data breaches in the past), but it's still a very
  straighfoward and secure way to manage your users
  

## 50. Demo: Onelogin integration with Jenkins using SAML

TODO


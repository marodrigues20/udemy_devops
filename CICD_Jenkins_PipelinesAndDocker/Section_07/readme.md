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


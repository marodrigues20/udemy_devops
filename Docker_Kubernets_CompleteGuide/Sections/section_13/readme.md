## Update an Object

When we modify a config file but we keep the name and the type of config file. Kubernetes will update 
the object and apply its respectives changes.

However, when we modify the name and type the Kubernetes will create a new object.

# How we make any change to the configuration in our kubernetes cluster
$ kubectl apply -f client-pod.yml

# Get detailed info about an object
$ kubectl describe <object type> <object name>
    |        |            |           |---------> Name of object (can be omitted)
    |        |            |---------------------> Specify the type of object we want to get info about
    |        |----------------------------------> We want to get detailed info
    |-------------------------------------------> CLI we use to change our Kubernetes cluster


# Check if the client-pod is running that updated
$ kubectl describe pod client-pod


# Quick note related update a kind of pod.
After creating a POD doesn't allowed update all attributes inside of pod object kubernetes.
Container; name; port can't be updated
image we can update


# Object Types
For updating a kind of service called Pod. We have to use Deployment object.


## Object Types
    - Pods -> Runs one or more closely related containers
    - Services -> Sets up networking in a Kubernetes Cluster
    - Deployement -> Maintains a set of identical pods, ensuring that they have the correct config and that the right number exists


## Pods vs. Deployement Objects

Pods is a very basic way to create a container with Kubernetes. But in reality, when start making use of Kubernetes for any serious propose, we make use 
of deployement's as opposed to individual pods.


    - Pods
        - Runs a single set of containers
        - Good for one-off dev purpose
        - Rarely used directly in production

    - Deployement
        - Runs a set of identical pods (one or more)
        - Monitors the state of each pod, updating as necessary
        - Good for dev
        - Good for production


# Remove an object - (Imperative command)

$ kubectl delete -f <config-file>
     |       |    |       |------> Path to config file that created this object
     |       |    |--------------> Specifies that we want to feed in a file to say what to delete
     |       |-------------------> We want to delete a running object
     |---------------------------> CLI we use to change our Kubernetes cluster


i.e:
  $ kubectl delete -f client-pod.yaml

## Retrieve pods information with more details
% kubectl get pods -o wide

## Why do we have to use Service in Kubernetes
We have to use Service in Kubernetes because they handle with PODs IP address. Everytime when a POD is created an IP is assined to it.
However, when we destroy one POD is possible that one new IP can be assigned or even more. We can scale our PODs and have a thousants different IPs running
inside our application in a container. How to manage this, or how to solve this kind of problem to access our application via Browser for example
In this scenario Service come into the picture.

## Imperative command(ugh) to update image
$ kubectl set image <object type> / <object name> <container name> = <new image to use>
    |      |    |          |               |            |
    |      |    |          |               |            |
    |      |    |          |               |            |--------------------------------> Name of the container we are updating (get this from config file)
    |      |    |          |               |---------------------------------------------> Name of object 
    |      |    |          |-------------------------------------------------------------> Type of object     
    |      |    |------------------------------------------------------------------------> We want to change the 'image' property
    |      |-----------------------------------------------------------------------------> We want to change a property
    |------------------------------------------------------------------------------------> CLI we use to change our Kubernetes cluster

i.e:
% kubectl set image deployment/client-deployment client=marodrigues20/multi-client:v5

Steps:

1) Build the Image adding a version label
$ docker build -t marodrigues20/multi-client:v5 .

2) Apply the command discribed above.
% kubectl set image deployment/client-deployment client=marodrigues20/multi-client:v5

3) The command above will force your deployment confi file to use the version v5



# Configure the VM to Use Your Docker Server installed in your localhost - Using Minicube

eval $(minikube docker-env)

Note: This only configures your current terminal window

# I can do somethings the same way that Docker does, Using Kubectl

$ kubectl get pods
$ kubectl logs <pod name> --> this is the same that $ docker log <container id>

$ kubectl exec -it <pod name> sh --> this is the same that $ docker 


Note: Many of Docker cli are available through kubectl

# How to remove all stop containers

$ docker system prune -a


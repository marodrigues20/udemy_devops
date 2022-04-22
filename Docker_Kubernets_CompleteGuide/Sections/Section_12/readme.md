# Section 12 - Onwards to Kubernetes

## Introduction
When you have a set of containers where you want to scale all containers together you don't need to use Kubernetes.
Just use Elastic beanstalk.

When you have to scale different containers into different machines or virtual machines having different numbers of instances 
for each container. You have to use Kubernetes.


## Minicube
It is used for creating virtual machines in your development environment. Just to create and run a Kubernetes cluster on your local machine.

## Kubectl
It is a program that is used to interact with Kubernetes cluster in general and manage what all different nodes are doing
and what different containers they are running.
We use Kubeclt on our local machine and production env.

## Docker Desktop's Kubernetes - Alternative for don't use Minicube on our localmachine.
We can use Docker Desktop's built-in Kubernetes instead of Minikube on macOS.
Docker Desktop will install Kubernetes Cluster Installation.
Docker Desktop have to use the context for docker-desktop and not something else like minikube or kind.
After the installation we can use kubectl.
Check the version using $ kubectl version

### Usage
Going forward, any minikube commands run in the lecture videos can be ignored. Also, you will be using localhost to access the services running in your cluster instead of the minikube IP address.

For example, in the first project where we deploy our simple React app, using minikube we would visit:

192.168.99.101:31515

Instead, when using Docker Desktop's Kubernetes, we would visit: localhost:31515

## Commands

$ kubectl version
$ kubectl cluster-info

## K8S
This means Kubernetes. ( Eight letters beteween K and S)

## Kubernetes
It doesn't create a container. It creates objects based on its types.
Object is a reference to a thing that exists inside of our kubernetes cluster.
Kubernetes didn't build our images - it got them from somewhere else


### Examples Object Types
- StatefulSet
- ReplicaController
- Pod -> Runs one or more closely related containers
- Service -> Set up networking in a Kubernetes Cluster
    - ClusterIP
    - NodePort -> Expose your container outside world. 
                  Allow you access your browser and acess your container on your local machine. 
                  (Only good for develop purpose)
                  There are two exception for this.
    - LoadBalancer
    - Ingress
- componentStatus
- configMap
- Endpoints
- Event
- Namespace
- ControllerRevision

Note: Objects serve different purposes - running a container, monitoring a container, setting up networking, etc.
Note 2: Objects link each other using labels and selector attributes inside the yml files.

# Kubernets in Action

When we start a Docker Desktop's Kubernetes / Minicube our Virtual machine run as a node. This node can be used by Kubernetes
to run some number of different objects. 
The most common object that we gonna use is the Pod.

Every single node where every single member of a Kubernetes cluster that we create has a program on it called the Kube-proxy.
The Kube-proxy is essencial the one single window to outside world. So any time that request comes into a node, it's going 
to flow throw this thing called the kube-proxy.
This proxy is going to inspect the request and decide how to route it to different services or different PODs that we may 
created of this node.

## What is a Pod Object?
It is a group of containers with a very common purpose.

## Why use a Pod Object?
In Kubernetes world there is no way to run a container without overhead, the smallest thing that you and I can deploy is a POD.
We need to have a POD to deploy one single container.
The purpose of POD is group containers with a very similiar purpose.
POD can contain only 1 or more containers inside of it.



# Feed a config file to Kubectl
$ kubectl apply -f <filename>
    |       |    |     |----> Path to the file with the config
    |       |    |----------> We want to specify a file that has the config changes 
    |       |---------------> Change the current configuration of our cluster 
    |-----------------------> CLI we use to change our Kubernetes cluster


# Print the status of all running pods
$ kubectl get pods
     |     |    |---> Specifies the object type that we want to get information about
     |     |--------> We want to retrieve information about running object 
     |--------------> CLI we use to change our Kubernetes cluster

NAME         READY   STATUS    RESTARTS   AGE
client-pod   1/1     Running   0          6m20s
     |       |          |      |            |------> The time that the POD is being running
     |       |          |      |-------------------> If the POD crash and needs to restart we have a counter. 
     |       |          |--------------------------> Status  
     |       |-------------------------------------> First number means the number of PODS. The second one means the numbers of copies that 
     |                                               we want to have as we start to scale our application.
     |---------------------------------------------> One POD created 

# The same example above but using type services
$ kubectl get services

# Kubernetes - Master
Master are individual machines (or vm's) that run containers
Kubernetes (the master) decided where to run each container - each node can run a dissimilar set of containers
To deploy something, we update the desired state of the master with a config file.

When we load the deployment file (Object types) we don't pass directly to worker nodes. Instead, the deployment file go to the master.
As a developer we work with the master. We don't work directly with the nodes. We gonna use kubectl to interact with the master.
The master always watch the worker nodes and try to automatically attempt to recreate that object inside of some given node until its list of responsibilities
is a 100% balanced out.

There are four programms that run inside Master:
    - Kube-apiserver -> 100% responsible for monitoring the current status of all the diffent nodes inside of your cluster and making sure that they are 
                        essentially doing the correct thing.





# Two ways to Approach Deployement

- Imperative Deployments 
    Do exactly these steps to arrive at this container setup

- Declarative Deployments
    Our container setup should look like this, make it happen
    Note: When we are working in any real production, deployment. Every engineer, everyone in the community is always going to advocate taking the declarative 
    approach.


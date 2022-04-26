# Section 14: A Multi-Container App with Kubernetes

## Introduction
In this section we will build our Multi-Container App with Kubernentes. It's important to remember that in the last section 13 we create object type for our 
docker-client using Service-NodePort. In this way, we could access through our browser the client. However, in this exercise, we will use the object type 
ClusterIP because it expose a set of pods to other objects in the cluster.
Service-NodePort is only good for dev purposes.
ClusterIP provide a way to the pod be connected by other pods inside the same cluster.

In this section we use Ingress Service to receive income traffic. Once the trafic received by Ingress Service, the request can access our pods in our cluster.


## Postgres Pod

When we create a postgres database we have to use "Volumes" on host machine because if the POD crashes our data will be lost.
The same way when we have to specify more than one replica. We have to add adicional configuration.

## Terminology

"Volume" in generica container terminology
    - Some type of mechanism that allows a container to access a filesystem outside itself.

"Volume" in Kubernetes
    - An object that allows a container to store data at the pod level
    - Persistence Volume -> Create a long term durable storage that is not tied to any specific pod or any specific container. Persistence Volume is outside the pod.
    - Persistence Volume Claim -> It's the advertisimente/option about one of those options inside of your pod config. It's going to give you a volume that's been created
    ahead of time or it's going to attempt to create one on the fly.


## Access Mode
  - ReadWriteOnce -> Can be used by a single node.
  - ReadOnlyMany -> Multiple nodes can read from this
  - ReadWriteMany -> Can be read and written to by many nodes

### Storage Classes Options
https://kubernetes.io/docs/concepts/storage/storage/storage-class/

## Object Type
    - Secret -> Securely stores a piece of information in the cluster, such as a database password.


### Creating a Secret

$ kubectl create secret <generic/docker-registry/tls> <secret_name> --from-literal key=value
             |     |                    |                   |             |           |--------------> Key-value pair of the secret information                                     
             |     |                    |                   |             | -------------------------> We are going to add the secret information into this command, as opposed 
             |     |                    |                   |                                          to  from file.
             |     |                    |                   |----------------------------------------> Name of secret, for later reference in a pod config
             |     |                    |------------------------------------------------------------> Type of Secret
             |     |
             |     |---------------------------------------------------------------------------------> Type of object we are going to create
             |---------------------------------------------------------------------------------------> Imperative command to create a new object


i.e: % kubectl create secret generic pgpassword --from-literal PGPASSWORD=12345

# Kubernetes commands used in this section:

$ kubectl get pods
$ kubectl get deployments
$ kubectl get services

### Delete the deployment created whose the name is client-deployment. The name of the deployment is retrieved by file name or apply the command $ kubectl deployments
$ kubectl delete deployment client-deployment

### Delete the service created whose the name is client-node-port. The name of the service is in config file or can be get apply the command $ kubectl get services
$ kubectl delete service client-node-port

### Apply the config files inside the directory whose the name is client-deployment.yaml
$ kubectl apply -f k8s/client-deployment.yaml

### Apply all config files inside the directory
$ kubectl apply -f k8s


### See all different options on your computer that Kubernetes has for creating a persistent volume.
$ kubectl get storageclass
NAME                 PROVISIONER          RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
hostpath (default)   docker.io/hostpath   Delete          Immediate           false                  4d9h

PROVISIONER is the one that will provide the way to provide a slice in your HD to save data discribed in PersistentVolumeClaim config file.
When you want to use cloud you have to describe in your PersistentVolumeClaim who will be the provider.

### Show who is the provider of storage
$ kubectl describe storageclass

### It's going to list out all the different persistent volumes that have been created inside our application
$ kubectl get pv

### It will list out all different claims that we have created as well. (Remeber: claim = adverstiment)
$ kubectl get pvc

### Create a secret password
% kubectl create secret generic pgpassword --from-literal PGPASSWORD=12345

### Check secrets 
$ kubectl get secrets

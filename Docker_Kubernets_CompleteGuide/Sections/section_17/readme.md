# Section 17: HTTPS Set up with Kubernetes



## Buy a Domain

ie.: https://domains.google


## Configure the domain resources inside domains.google website

## Kubernetes - Issuer Object Type

Create a file in k8s directory.

Issuer is included in Kubernetes by default. It is something that is defined specifically by the search manager that we 
just installed. Then we define this in apiVersion inside config file.

i.e: apiVersion: certmanager.k8s.io/v1alpha1
     kind: ClusterIssuer

Inside the declaration above. We're saying reach into the search manager bucket objects. We want to create an Issuer type object.


## Kubernetes - Certificate Object Type

Create a file in k8s directory. 

i.e: apiVersion: certmanager.k8s.io/v1alpha1
     kind: Certificate


## Ingress Service

Tell it to update itself to use the new certificate.

## After Commit the Issuer and Certificate Config Files in Github.

After you commit and Issuer and Certificate config files into Github. The travis-ci.com will run our .travis.yml file.
Two new Kubernetes Objects will be created. You can see them using the bellow commands inside gcloud shell.

$ kubectl get certificates -> show all certificates
$ kubectl describe certificates -> check events section inside output command
$ kubectl get secrets -> We can see data that represent the certificate


## Change Ingress-service

The ingress-service.yaml file have to be changed to serve https traffic. 
We have to specify how to get the certificate from the issuer.
Let's make that the Internet Server always forces users to make use of https traffic. Basically we always redirect http to https
We have to duplicate the rule related "/" and "/api/" to contemplate www.domain.com




## Local Environment Cleanup

### Deleting Pods, Deployments, Services from the Multi K8's project

In the root project directory, run:

$ kubectl delete -f k8s/

### Stopping Minikube

To stop Minikube, and the VM that it runs, run minikube stop .  You can bring your local cluster back online at any time by running minikube start

To fully delete the cluster, run 

$ minikube delete

### Stopping Running Containers

You might still have some containers running on your machine.  Try a 

$ docker ps .  

You can then run:

$ docker stop <container_id>


### Clearing the Build Cache

All the images that we built and ran during the course are cached on your local machine - they might be taking up to around 1GB of space.  You can clean these up by running 

$ docker system prune



## All Commands in this section


$ kubectl delete -f k8s/

$ docker ps .  

$ docker stop <container_id>

$ docker system prune

$ docker system prune -a






































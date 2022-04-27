# Section 15: Handling Traffic with Ingress Controllers

## Introduction
In this section will set up to allow traffic to get into our application.

## Services discussed in this section

Load Balancer - Legacy way of getting network traffic into a cluster
Ingress - Exposes a set of services to the outside world

## Why we're not going to use it
When we set up the Load Balance Service, we're going to essentially allow access to one specific set of pods inside of our application.
Load Balancer only is going to give you access to one set of pods.
In our case our application has two set of pods. So we want to expose to the outside world, which is 100% common.
So a load balancer would not be able to give us access to both the multi server set and the MultiClient as well.
When you make a load balancer service, you are not only getting access to a specific set of pods. Kubernetes is going to acutally do something in the background for you as well.
It's going to reach out to your cloud provider. So, could be AWS or Google Cloud or whoever else you might be using. And it's going to create a load balancer using their 
configuration or definition of what a load balancer is.

In AWS world yo might get a classic load balancer or an application load balancer.
Kubernetes going to tell AWS. I need an application load balancer or classic load balancer. So it's going to set up this external resource outside of your cluster and then it's going to automatically configure that load balancer to send traffic into your cluster and access the load balancer service that has been set up to given access to this very specific set of pods.


## Why use Ingress
We write some Ingress routing rules to get traffic to services.
Kubectl creates something called ingress controller. As a controller, it is going to look at our current state. It's going to look at the desired state and then create
some infrastructure that's going to make our desired state a reality.

## Kubernetes-ingress
We are not using kubernetes-ingress, a project led by the company nginx. github.com/nginxinc/kubernetes-ingress
It doesn't work in the same way of Ingress-ngixn. It is a totally separate project.

## Ingress-nginx
We are using ingress-nginx, a community led project. github.com/kubernetes/ingress-nginx
Set up ingress-nginx changes depending on your environment (local, GC, AWS, Azure)
We are going to set up ingress-nginx on local and GC (Google Cloud)

### Ingress Controller
Watches for changes to the ingress and updates the 'thing' that handles traffic.

### Ingress Config
Object that has a set of configuration rules describing how traffic should be routed.

### Why we don't use Nginx rather than Ingress-nginx
When you make use of the Engine Ingress project, rather than taking incoming traffic and sending it off to the Cluster IP and allowing IP to load balance that request off to some random port
inside of your Nginx Ingress project is not going to send traffic over to the cluster IP service and allow that to do some amount of load balancing instead. The cluster IP service 
does still is working to essentially keep track of all these different pods, but any time we get a incoming request to the entrace pod right here, the pod is going to actually route the request
directly to one of these different ports, completely by passing the Cluster IP service. The reason that this is done is to allow for features like, say, sticky session, which is a reference 
to the fact that we sometimes want to make sure that if one user sends to request to our application, we want to make sure that both those requests go to exact same server.

So that's just one example of why we are making use of this Ingress-Nginx project as opposed to kind of doing our own build it yourself solution. So you can certainly can do a build yourself solution.
It's just going to lose out on a couple of these extra features that you get for free when you make use of the Ingress Engine next project.

### Understand ingress-nginx a bit more
https://www.joyfulbikeshedding.com/blog/2018-03-26-studying-the-kubernetes-ingress-system.html

### Docker-drive
https://minikube.sigs.k8s.io/docs/drivers/docker/#known-issues

### Installing Ingress-nginx

https://kubernetes.github.io/ingress-nginx/deploy/#quick-start

### Installation - Docker Desktop (macOS and Windows)
$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.0/deploy/static/provider/cloud/deploy.yaml

GCE-GKE
First, your user needs to have cluster-admin permissions on the cluster. This can be done with the following command:
kubectl create clusterrolebinding cluster-admin-binding \
  --clusterrole cluster-admin \
  --user $(gcloud config get-value account)

$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.0/deploy/static/provider/cloud/deploy.yaml


## What is Deployment Type Object
Deployment is a type of 'controler'.
In Kubernetes a controller is any type of object that constantly works to make some desired state a reality inside of our cluster.
In the world of Ingress, it is the exact same strategy.











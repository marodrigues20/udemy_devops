# Section 16: Kubernetes Production Deployment 

In this section we gonna deploy our application built in Section 15 where we run Fibonati App (Dev-mode) in our local machine
using Kubernetes.


## Agenda:

    - Create Github Repo
    - Tie repo to Travis CI
    - Create Google Cloud project
    - Enable billing for the project
    - Add deployment scripts to the repo


## Why Google Cloud?

    - Google created Kubernetes!
    - Far, far easier to poke around Kubernetes on Google Cloud
    - Excellent documentation for beginners


## Encript service account key file

We create a service account in GCP and added a secret key for this account. We downloaded the file and we have to commit together with the project.
However, there are sensitive information in the file. We gonna encript the file using some commands that need ruby installed. Macos comes with ruby 
pre-installed but windows we have to install it. For executing this process, we gonna use a Docker Container.


### Installing Ruby Container

$ docker run -it -v $(pwd):/app ruby:2.4 sh

$ gem install travis


#### The Travis login now requires a Github Token.
https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token


#### This will also require setting the scope. Travis requires the permissions noted here:
https://docs.travis-ci.com/user/github-oauth-scopes/#repositories-on-httpstravis-cicom-private-and-public

#### The login command
$ travis login --github-token YOUR_PERSONAL_TOKEN --pro

#### When you encrypt the file, you must pass the same --com or --pro flag you used to log in:

$ travis encrypt-file multi-k8s-348609-28ed82e4ae1c.json -r marodrigues20/multi-k8s --com
or
$ travis encrypt-file service-account.json -r USERNAME/REPO --pro

### How to keep a postgres password in GCP Kubernetes Cluster

1) Open the GCP Cloud Shell
2) Run the below commands in their respectives order

$ gcloud config set project <GCP Project Id>

$ gcloud config set compute/zone <zone>

Note: i.e: <zone> = europe-west2-a

$ gcloud container clusters get-credentials <cluster name>


# This command is discribed in Section 15.
$ kubectl create secret generic pgpassword --from-literal PGPASSWORD=mypgpassword123


## What is Helm
Helm is a program that is used to administer third party software inside Kubernetes Cluster.
We can use kubectl apply or Helm. Helm is better for complex project.

When we install helm. We get two separate pieces of software. The first one is Helm but we reference it as a Helm Client.
The another one is Tiller Server. It run inside Kubernete Cluster. It is responsible to modify the cluster in some fashion 
and install aditionals softwares inside of it.

Link: https://helm.sh
      https://github.com/helm/helm
      https://helm.sh/docs/intro/install/


Note: Tiller Server in production in GCP for example need to have permissions to modify the Kubernetes Cluster using RBAC System.
Note2: A Service Account and Cluster Role Binding need to be created and tie those to Tiller Pod for change everything in our
        entire cluster.


## Install Helm And Tiller in GCP. Using Cloud Shell

- https://helm.sh -> Installing Helm -> From Script


## What is RBAC (Role Based Access Control)

The purpose of RBAC is:
    - Limits who can access and modify objects in our cluster
    - Enabled on Google Cloud by default
    - Tiller wants to make changes to our cluster, so it needs to get some permission set

Note: RBAC is NOT enabled by default. In our local environment we could access the cluster directly and arbitraly change configuration.
      In other words, we could create a pod inside of our cluster locally, the local cluster that tried to access the Kubernentes Cluster
      and arbitrarily create new sets of pods or new deployments or new secrets or delete stuff if it wanted to.

      However, in our Production Environment the RBAC system is all about making sure that we have the ability to limit who can do what 
      inside of our cluster.


### Some Definitions

    User Accounts => Identifies a *person* administering our cluster
    Service Account => Identifies a *pod* administering a cluster
    ClusterRoleBinding => Authorizes an account to do a certain set of actions accross the entire cluster
    RoleBinding => Authorizes an account to do a certain set of actions in a *single namespace*

## Install Ingress-Nginx

### Create a new service account called tiller in the kuber-system namespace

$ kubectl create serviceaccount --namespace kube-system tiller

### Create a new clusterrolebinding with the role 'cluster-admin' and assign it to service account 'tiller'

$ kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller


### In your Google Cloud Console run the following:

$ helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
$ helm install my-release ingress-nginx/ingress-nginx
 
IMPORTANT: If you get an error such as chart requires kubeVersion: >=1.16.0-0.....

You may need to manually upgrade your cluster to at least the version specified:

gcloud container clusters upgrade  YOUR_CLUSTER_NAME --master --cluster-version 1.16

This should not be a long term issue since Google Cloud should handle this automatically:

https://cloud.google.com/kubernetes-engine/docs/how-to/upgrading-a-cluster


Note: After installing the ingress-nginx. We gonna see in workload gcp a new controller created. The Ingress Controller is the POD  that is our in this case
, the deployment that manages the POD, that runs the actual controller that's going to read our Ingress config file and then set up nginx appropriately.

Note 2: In the Service menu inside Kubernetes Engine in GCP
        Two set of Type Object ( External Load Balance and Cluster IP )
        




































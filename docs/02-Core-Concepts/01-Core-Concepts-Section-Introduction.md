# Core Concepts Section Introduction

 - Take me to the [Video Tutorial](https://kodekloud.com/topic/core-concepts-section-introduction/)
 
In this section, we will take a look at the below
- Cluster Architecture  
- API Primitives
- Services & Other Network Primitives

k8s reference docs:
- https://kubernetes.io/docs/concepts/overview/kubernetes-api/
- https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/
- https://kubernetes.io/docs/concepts/architecture/
- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/services-networking/

# Core Concepts Section Command Cheatsheet

Reference (Bookmark this page for exam. It will be very handy):

**https://kubernetes.io/docs/reference/kubectl/conventions/**

#### Easy Create Resource / Generate YAML

- Create an NGINX Pod

`kubectl run nginx --image=nginx`

- Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

`kubectl run nginx --image=nginx --dry-run=client -o yaml`

- Create a deployment

`kubectl create deployment ${deployment_name} --image=nginx nginx`

- Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)

`kubectl create deployment ${deployment_name} --image=nginx nginx --dry-run=client -o yaml`

- Generate Deployment YAML file (-o yaml). Don’t create it(–dry-run) and save it to a file.

`kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml`

- Make necessary changes to the file (for example, adding more replicas) and then create the deployment.

`kubectl create -f nginx-deployment.yaml`

OR

- In k8s version 1.19+, we can specify the --replicas option to create a deployment with 4 replicas.

`kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml`

#### Easy Update Resource
- Opens up resource configuration in YAML format and edits it interactively
`kubectl edit ${resource_type} ${resource_name}`
- Or apply an edited YAML setting
`kubectl apply -f nginx-deployment.yaml`

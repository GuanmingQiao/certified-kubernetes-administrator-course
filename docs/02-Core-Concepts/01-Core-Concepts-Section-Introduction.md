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

- Create an NGINX Pod and expose it on container port 8080

`kubectl run nginx --image=nginx --port=8080`

- Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

`kubectl run nginx --image=nginx --dry-run=client -o yaml`

- Create a deployment with name nginx and image nginx and 2 replicas

`kubectl create deployment nginx ${deployment_name} --image=nginx --replicas=2`

- Create a service that exposes pod container port 6500 on service port 443 for the whole Cluster (ClusterIP)
  
`kubectl expose pod redis --target-port=6500 --port=443 --name redis-service`

- Generate Deployment YAML file (-o yaml). Don’t create it(–dry-run) and save it to a file.

`kubectl ${action} ${resource_type} ${resource_name} --image=nginx --dry-run=client -o yaml > ${config_name}.yaml`

- In general, can use declarative command (leave k8s to figure out what to do). K8s will create if does not exist

`kubectl apply -f nginx-deployment.yaml`

#### Easy Update Resource
- Opens up resource configuration in YAML format and edits it interactively

`kubectl edit ${resource_type} ${resource_name}`

- Or apply an updated yaml file

`kubectl replace -f nginx-deployment.yaml`

- Or delete then recreate a resource:

`kubectl replace --force -f nginx-deployment.yaml`

- In general, can use declarative command (leave k8s to figure out what to do). K8s will replace if exists

`kubectl apply -f nginx-deployment.yaml`

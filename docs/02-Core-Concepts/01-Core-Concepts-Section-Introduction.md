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

#### Easy Getting Resource
- Inspect the environment for all controlplane components: get all pods in kube-system
`kubectl get pods -n kube-system`
`kubectl get all -n kube-system`
`kubectl get pods/rcs/deployments -n kube-system`

- Basic pods for k8s to run: `etcd-controlplane`, `kube-apiserver-controlplane`, `kube-controller-manager-controlplane`, `kube-proxy`, `kube-scheduler-controlplane`

- Filter resources with metadata label selector: `kubectl get pods --selector key1=val1,key2=val2`

#### Easy Create Resource / Generate YAML

- Create an NGINX Pod and expose it on container port 8080

`kubectl run nginx --image=nginx --port=8080`

- Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

`kubectl run nginx --image=nginx --dry-run=client -o yaml`

- Generate Pod Manifest YAML file from existing Pod. This applies to all resources

`kubectl get pod webapp -o yaml > my-new-pod.yaml`

- Create a replicaset with name nginx and image nginx and 2 replicas

`kubectl create replicaset nginx ${rs_name} --image=nginx --replicas=2`

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

#### Networking Commands
- `ip link`: Analyze and manage network interface
- `ip address`: Find out the current network configuration
  - `ip address show type bridge`: Show all network interfaces of type `bridge`
- `ip addr add 192.168.1.10/24 dev eth0`
- `ip route`
- `ip route add 192.168.1.10/24 via 192.168.2.1`
- `cat /proc/sys/net/ipv4/ip_forward`
- `arp`
- `netstat -plnt`
- `route`

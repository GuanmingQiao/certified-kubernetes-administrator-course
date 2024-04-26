# Kube API Server
  - Take me to [Video Tutorial](https://kodekloud.com/topic/kube-api-server/)
  
In this section, we will talk about kube-apiserver in kubernetes

#### Kube-apiserver is the primary component in kubernetes.
- Kube-apiserver is responsible for **`authenticating`**, **`validating`** requests, **`retrieving`** and **`Updating`** data in ETCD key-value store. In fact kube-apiserver is the only component that interacts directly to the etcd datastore. The other components such as kube-scheduler, kube-controller-manager and kubelet uses the API-Server to update in the cluster in their respective areas.
  
  ![post](../../images/post.PNG)
- Specifically, when `kube-apiserver` receives a POST API call to create a Pod, it interacts with other components:
  - `kube-apiserver` authenticates user, validates request (by comparing with exisiting configuration in etcd)
  - `kube-apiserver` creates a pod object without assigning it to a worker node, updates ETCD
  - `kube-scheduler` continuously monitors `kube-apiserver`. Realizing that there's an unassigned Pod, assigns the Pod to a Worker Node
  - `kube-apiserver` receives Worker Node information from `kube-scheduler`, then updates ETCD
  - `kube-apiserver` updates kublet on the Worker Node. kublet creates the Pod, then report back to `kube-apiserver`
  - `kube-apiserver` updates ETCD for the last time with the Pod creation info
  
## Installing kube-apiserver

- If you are bootstrapping kube-apiserver using **`kubeadm`** tool, then you don't need to know this, but if you are setting up using the hardway then kube-apiserver is available as a binary in the kubernetes release page.
  - For example: You can downlaod the kube-apiserver v1.13.0 binary here [kube-apiserver](https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-apiserver)
    ```
    $ wget https://storage.googleapis.com/kubernetes-release/release/v1.13.0/bin/linux/amd64/kube-apiserver
    ```
 
 ![kube-apiserver](../../images/kube-apiserver.PNG)
  - Some important parameters:
    - `cafile`, `certfile`, `keyfile`, `kubelet-certificate-authority`, `kublet-client-certificate`, `kublet-client-key`, `kublet-https`: certificate / authentication-related parameters
    - `etcd-servers`: host and port of the ETCD server (see 05 notes for where it is defined in ETCD setup)
 
## View kube-apiserver - Kubeadm
- kubeadm deploys the kube-apiserver as a pod in kube-system namespace on the master node.
  ```
  $ kubectl get pods -n kube-system
  ```
   
  ![kube-apiserver1](../../images/kube-apiserver1.PNG)
   
## View kube-apiserver options - Kubeadm
- You can see the options with in the pod definition file located at **`/etc/kubernetes/manifests/kube-apiserver.yaml`**
  ```
  $ cat /etc/kubernetes/manifests/kube-apiserver.yaml
  ```
  
  ![kube-apiserver2](../../images/kube-apiserver2.PNG)
   
## View kube-apiserver options - Manual
- In a Non-kubeadm setup, you can inspect the options by viewing the kube-apiserver.service
  ```
  $ cat /etc/systemd/system/kube-apiserver.service
  ```
  
  ![kube-apiserver3](../../images/kube-apiserver3.PNG)
   
- You can also see the running process and effective options by listing the process on master node and searching for kube-apiserver.
  ```
  $ ps -aux | grep kube-apiserver
  ```
  ![kube-apiserver4](../../images/kube-apiserver4.PNG)

K8s Reference Docs:
- https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/
- https://kubernetes.io/docs/concepts/overview/components/
- https://kubernetes.io/docs/concepts/overview/kubernetes-api/
- https://kubernetes.io/docs/tasks/access-application-cluster/access-cluster/
- https://kubernetes.io/docs/tasks/administer-cluster/access-cluster-api/

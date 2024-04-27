# Pods
  - Take me to [Video Tutorial](https://kodekloud.com/topic/pods-2/)
  
In this section, we will take a look at PODS.
- POD introduction
- How to deploy pod?

#### Kubernetes doesn't deploy containers directly on the worker node.
Pod is the smallest unit of deployment you can control in Kubernetes

  ![pod](../../images/pod.PNG)
  
#### Here is a single node kubernetes cluster with single instance of your application running in a single docker container encapsulated in the pod.

![pod1](../../images/pod1.PNG)

#### Pod will have a one-to-one relationship with containers running your application.
You can save cost by deploying multiple Pod on a Node. In AWS, this will be each EC2 instance running multiple Pods of different applications.
  
## Multi-Container PODs 
A single pod can have multiple containers except for the fact that they are usually ** multiple containers of different kind ** -- like sidecar architecture. Not doing sidecar architecture means having to deploy main and helper containers seperately, making container management hard.
  
  ![pod3](../../images/pod3.PNG)
  
## Docker Example (Docker Link)
  
  ![pod4](../../images/pod4.PNG)
  
## How to deploy pods?
Lets now take a look to create a nginx pod using **`kubectl`**.

- To deploy a pod using default image from DockerHub.
  ```
  $ kubectl run nginx --image nginx
  ```

 ![kubectl](../../images/kubectl.PNG)

- To deploy a pod using definition from yaml file (`-f`)
  - apiVersion: version of `kubectl_api` to use
  - kind: Pod
  - metadata (dict)
    - name
    - labels (dict)
        - app
        - type
  - spec (dict)
    - containers (list)
      - name:
      - image:
  ```
  $ kubectl create -f pod-definition.yml
  ```
  ![image](https://github.com/GuanmingQiao/certified-kubernetes-administrator-course/assets/22064968/a47383d9-df30-424c-a76f-a5d850cae842)
  
- To get the list of pods
  ```
  $ kubectl get pods
  $ kubectl describe pod ${name}
  ```
  The `Ready` column notes `running containers / total containers` in a Pod
  
  ![image](https://github.com/GuanmingQiao/certified-kubernetes-administrator-course/assets/22064968/3b81180d-5890-4b31-ac88-9f92d5283868)



K8s Reference Docs:
- https://kubernetes.io/docs/concepts/workloads/pods/pod/
- https://kubernetes.io/docs/concepts/workloads/pods/pod-overview/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/explore/explore-intro/



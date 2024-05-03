# Resource Limits
  - Take me to [Video Tutorials](https://kodekloud.com/topic/resource-limits/)
  
In this section we will take a look at Resource Limits

#### Let us take a look at 3 node kubernetes cluster.
- Each node has a set of CPU, Memory and Disk resources available.
- If there is no sufficient resources available on any of the nodes, kubernetes holds the scheduling the pod. You will see the pod in pending state. If you look at the events, you will see the reason as insufficient CPU.
  
  ![rl](../../images/rl.PNG)

- Typically, the best comination is having `Resource Requirements` but not `Resource Limit`, so that all Pods are guaranteed minimum resources, and each Pod are free to consume the leftover availiable CPU.

## Resource Requirements
- By default, K8s assume that a pod or container within a pod requires **`0.5`** CPU and **`256Mi`** of memory. This is known as the **`Resource Request` for a container**.
  
  ![rr](../../images/rr.PNG)
  
- If your application within the pod requires more than the default resources, you need to set them in the pod definition file.

  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: simple-webapp-color
    labels:
      name: simple-webapp-color
  spec:
   containers:
   - name: simple-webapp-color
     image: simple-webapp-color
     ports:
      - containerPort:  8080
     resources:
       requests:
        memory: "1Gi"
        cpu: "1"
  ```
  ![rr-pod](../../images/rr-pod.PNG) 
   
## Resources - Limits
- By default, k8s sets resource limits to 1 CPU and 512Mi of memory
  
  ![rsl](../../images/rsl.PNG)
  
- You can set the resource limits in the pod definition file.
  
  ```
  apiVersion: v1
  kind: Pod
  metadata:
    name: simple-webapp-color
    labels:
      name: simple-webapp-color
  spec:
   containers:
   - name: simple-webapp-color
     image: simple-webapp-color
     ports:
      - containerPort:  8080
     resources:
       requests:
        memory: "1Gi"
        cpu: "1"
       limits:
         memory: "2Gi"
         cpu: "2"
  ```
  ![rsl1](../../images/rsl1.PNG)
  

  
## Exceed Limits
- what happens when a pod tries to exceed resources beyond its limits?

   ![el](../../images/el.PNG)
   
## Default Pod Resource Requirement: Namespace-level Applied
#### Note: Remember Requests and Limits for resources are set per container in the pod. All numbers are per-container
- LimitRange: all Pods created in namespace by default will request / limit
  
<img width="352" alt="image" src="https://github.com/GuanmingQiao/certified-kubernetes-administrator-course/assets/22064968/5df6f755-9a7b-45fc-b89d-98f8a7afdb2a">

<img width="351" alt="image" src="https://github.com/GuanmingQiao/certified-kubernetes-administrator-course/assets/22064968/c54f583c-b3c9-4b28-8f95-9c42aae33390">

- Resource Quotas: in total, all Pods in a namespace will request / limit

<img width="241" alt="image" src="https://github.com/GuanmingQiao/certified-kubernetes-administrator-course/assets/22064968/d45955f8-7ff0-4175-b359-083eb0f2f630">




#### K8s Reference Docs:
- https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
  
  

# Kubernetes Services
  - Take me to [Video Tutorial](https://kodekloud.com/topic/services-3/)
  
In this section we will take a look at **`services`** in kubernetes

## Services
- Kubernetes Services enables communication between various components within and outside of the application.

  ![srv1](../../images/srv1.PNG)
  
#### Let's look at some other aspects of networking

## External Communication

- How do we as an **`external user`** access the **`web page`**?

  - From the node (Able to reach the application as expected)
  
    ![srv2](../../images/srv2.PNG)
    
  - From outside world (This should be our expectation, without something in the middle it will not reach the application)
  
    ![srv3](../../images/srv3.PNG)
   
    
 ## Service Types
 
 #### There are 3 types of service types in kubernetes
 
 1. NodePort
    - Where the service makes an internal port accessible on a port on the NODE.
      ```
      apiVersion: v1
      kind: Service
      metadata:
       name: myapp-service
      spec:
       types: NodePort
       ports:
       - targetPort: 80
         port: 80
         nodePort: 30008
      ```
     ![srvnp](../../images/srvnp.PNG)
      
      #### To connect the service to the pod
      ```
      apiVersion: v1
      kind: Service
      metadata:
       name: myapp-service
      spec:
       type: NodePort
       ports:
       - targetPort: 80
         port: 80
         nodePort: 30008
       selector:
         app: myapp
         type: front-end
       ```
    - You need to configure both networking (**`targetPort`, `port`, `nodePort**`) and **selector** correctly
      - **targetPort**: The internal port on the Pod. Called targetPort from Service's perspective.
      - **port**: Service's internal port listening to the Pod.
      - **nodePort**: Node's external port that is made availiable and linked to internal ports.
      - **selector**: Pod labels that the Service links to.

    ![srvnp1](../../images/srvnp1.PNG)
      
      #### To create the service
      ```
      $ kubectl create -f service-definition.yaml
      ```
      
      #### To list the services
      ```
      $ kubectl get services
      ```
      
      #### To access the application from CLI instead of web browser
      ```
      $ curl http://192.168.1.2:30008
      ```

      #### A service with multiple pods is supported.
    - Will randomly route to all pods with the labels
      
      ![srvnp3](../../images/srvnp3.PNG)
      
      #### A service with pods distributed across multiple nodes is supported.
    - The NodePort will be made availiable on every `${Node_IP}:{Service_Port}`. Will reandomly route to all pods with the labels.
     
      ![srvnp4](../../images/srvnp4.PNG)
     
            
 3. ClusterIP
      #### Different from NodePort: Single IP vs. Node IP.
    - In this case the service creates a **single virtual IP** inside the cluster to enable communication between different services such as a set of frontend servers to a set of backend servers.
    ![image](https://github.com/GuanmingQiao/certified-kubernetes-administrator-course/assets/22064968/99465660-bf65-45d0-bc9d-41de69f65399)

      #### To connect the service to the pod
    - Same Yaml as NodePort Service. With `type: ClusterIP`

 5. LoadBalancer
    - Where the service provisions a **`loadbalancer`** for our application in supported cloud providers.
    
K8s Reference Docs:
- https://kubernetes.io/docs/concepts/services-networking/service/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/expose/expose-intro/


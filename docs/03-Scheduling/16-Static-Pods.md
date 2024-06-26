# Static Pods 
  - Take me to [Video Tutorial](https://kodekloud.com/topic/static-pods/)
  
In this section, we will take a look at Static Pods

#### How do you provide a pod definition file to the kubelet without a kube-apiserver?
- You can configure the kubelet to read the pod definition files from a directory on the server designated to store information about pods.

## Configure Static Pod
- The designated directory can be any directory on the host and the location of that directory is passed in to the kubelet as an option while running the service.
  - The option is named as **`--pod-manifest-path`**.
- Can find kublet configuration at `var/lib/kubelet/config.yaml`
- Kublet is deployed per-node. If you see a static pod deployed on another node, you need to `ssh ${node_ip_address}`, then modify/delete the node configuration file from there.
- **Warning: can not edit the files in manifest path**: all modifications need to be done outside of the manifest folder. Otherwise, the whole yaml file will be ignored by Kubelet. To edit an existing static pod, use `kubectl edit` command.

  
  ![sp](../../images/sp.PNG)
  
## Another way to configure static pod 
- Instead of specifying the option directly in the **`kubelet.service`** file, you could provide a path to another config file using the config option, and define the directory path as staticPodPath in the file.

  ![sp1](../../images/sp1.PNG)

## View the static pods
- To view the static pods (without `kube-apiserver` and therefore no `kubectl` command.
  ```
  $ docker ps
  ```
  ![sp2](../../images/sp2.PNG)

#### The kubelet can create both kinds of pods - the static pods and the ones from the api server at the same time.
- `kube-apiserver` is aware of static Pods created by kubelets, but is unable to edit them
- Therefore, the static Pods returned by `kubectl` command are a read-only mirror.

  ![sp3](../../images/sp3.PNG)

## Static Pods - Use Case

  ![sp4](../../images/sp4.PNG)
  
  ![sp5](../../images/sp5.PNG)
  
## Static Pods vs DaemonSets

   ![spvsds](../../images/spvsds.PNG)
  

#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/

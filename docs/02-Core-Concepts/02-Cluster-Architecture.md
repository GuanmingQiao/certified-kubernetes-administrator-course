# Cluster Architecture

  - Take me to [Video Tutorial](https://kodekloud.com/topic/cluster-architecture/)

In this section , we will take a look at the kubernetes Architecture at high level.
- 10,000 Feet Look at the Kubernetes Architecture

  ![Kubernetes Architecture](../../images/k8s-arch.PNG)
  
  ![Kubernetes Architecture 1](../../images/k8s-arch1.PNG)

  - Cargo ships: Worker Node (Host Application as Containers)
    -  Container engine: Docker / ContainerD
    -  Captain: **kublet** Service (executes instructions from kube-apiserver, and sends report back to kube-apiserver)
    -  Inter-ship communications: **kube-proxy** Pod (enable non-k8s services on two nodes to reach each other)
  - Control ships: Master Node (Manage, Plan, Schedule, Monitor Nodes)
    -  Log book maintaining information about cargo ships: **etcd Cluster** Pod
    -  Crane that loads cargo onto cargo ships: **kube-scheduler** Pod
    -  Offices for Maintainance teams: **kube-controllers** Pods
      - Cargo ship maintainance office: **node-controller** (Handle Node Lifecycle)
      - Cargo fleet maintainance office: **replication-controller** (Handle Replication Strategy)
      - CEO's office: **controller-manager**
    -  Control ship communications (internal + between captains): **kube-apiserver** Pod

K8s Reference Docs:
- https://kubernetes.io/docs/concepts/architecture/

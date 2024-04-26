# Docker vs. ContainerD

In this section we will look at the differences between Docker and ContainerD

And then Kubernetes grew in popularity as a container orchestrator and now other container runtimes like `rkt` wanted in so Kubernetes users needed it to work with container runtimes that are other than just Docker, and so Kubernetes introduced an interface called **container runtime interface or CRI**, so CRI allowed any vendor to work as a container runtime for Kubernetes as long as they adhere to the OCI standards.
So OCI stands for **Open Container Initiative** and it consists of an image spec and a runtime spec. Image spec means the specifications on how an image should be built. It defines the specifications on **how an image should be built** and **the runtime spec** defines the standards on how any container runtime should be developed so keeping these standards in mind, anyone can build a container runtime that can be used by anybody to work with Kubernetes, so that was the idea.

![](../../images/02-03-03.png)

So `rkt` and other container runtimes that adhere to the OCI standards were now supported as container runtimes for Kubernetes via the CRI, however Docker wasn’t built to support  the CRI standards, remember Docker was built way before CRI was introduced and Docker still was the dominant container tool used by most, so Kubernetes had to continue to support Docker as well and so Kubernetes introduced what is known as **`dockershim`: a hacky but temporary way to support Docker outside of the CRI**.

![](../../images/02-03-04.png)

So while most other container runtimes worked to the CRI, Docker continued to work without it. Docker consists of multiple tools that are put together, for example the Docker CLI, the Docker API, the build tools that help in building images. There was support for volumes, security, and finally the **container runtime called `runc`**, and the **daemon that managed `runc`, that was called `containerd`**. 

So **containerd is CRI compatible** and can work directly with Kubernetes as all other runtimes, so containerd can be used as a runtime on its own separate from Docker.

![](../../images/02-03-05.png)

So now you have **containerd as a separate runtime from Docker**, so Kubernetes continued to maintain support for Docker engine directly however having to maintain the dockershim was an unnecessary effort and added complications so it was decided in **v1.24 release of Kubernetes to remove dockershim completely** and so support for Docker was removed. But you see all the images that were built before Docker was removed so all the Docker images continued to work because Docker followed the image spec from the OCI standards so all the images built by Docker follow the standard so they continued to work with containerd but Docker itself was removed as a supported runtime from Kubernetes. So that’s kind of the whole story, now let’s look into containerd more specifically.

![](../../images/02-03-06.png)

So containerd although is part of Docker, is a separate project on its own now and is a member of [CNCF](https://www.cncf.io/) with the [graduated](https://www.cncf.io/projects/) status, so you can now install containerd on its own without having to install Docker itself. Now, once you install containerd it comes with a command line tool called [ctr](https://github.com/projectatomic/containerd/blob/master/docs/cli.md#client-cli), and this tool is solely made for debugging containerd and is not very user friendly.

So just to give you an idea, the `ctr` command can be used to perform basic container-related activities such as pull images, for example to pull redis image you would run

```
ctr images pull docker.io/library/redis:alpine
```

To run a container we use the `ctr` run command

```
ctr run docker.io/library/redis:alpine redis
```

But as I mentioned, this tool is solely for debugging containerd and is not very user friendly and is not to be used for managing containers on a production environment.
So a **better alternative recommended is the [nerdctl](https://github.com/containerd/nerdctl#nerdctl-docker-compatible-cli-for-container) tool**. So the `nerdctl` tool is a command line tool that’s very similar to Docker, so it’s a Docker-like CLI for containerd. It supports most of the CLI options that Docker supports and apart from that it has the added benefit that it can give us access to the newest features implemented in containerd, so for example we can work with encrypted container images or other new features that will eventually be implemented into the regular Docker command in the future. It also **supports lazy pulling of images, P2P image distribution, image signing and verifying and namespaces in Kubernetes which are not available in Docker**. So the `nerdctl` tool works very similar to Docker cli, so instead of Docker you would simply have to replace it with `nerdctl` so it can run almost all Docker commands that interact with containers like this

![](../../images/02-03-07.png)

It’s important to talk about another command like tool known as [crictl](https://github.com/kubernetes-sigs/cri-tools/blob/master/docs/crictl.md#container-runtime-interface-cri-cli). So earlier we talked about the CRI which is a single interface used to connect CRI compatible container runtimes, containerd, `rkt` and others. So the **`crictl` is a command line utility that is used to interact with the CRI compatible container runtime**, so this is kind of **interaction from the Kubernetes perspective that works across different container runtimes**. The `crictl` utility is only used for debugging purposes and getting into containers and all of that.

![](../../images/02-03-08.png)

One major difference is that the `crictl` command is also aware of pods so you can list pods by running the `crictl` pods command so this wasn’t something that Docker was aware of. So while working with Kubernetes in the past, we used Docker commands a lot to troubleshoot containers and view logs especially on the worker nodes and now you’re going to use the `crictl` command to do so. A full list of differences can be found in [this link](https://kubernetes.io/docs/reference/tools/map-crictl-dockercli/#retrieve-debugging-information).

| docker cli | crictl            | Description                                                          | Unsupported Features                |
|------------|-------------------|----------------------------------------------------------------------|-------------------------------------|
| attach     | attach            | Attach to a running container                                        | --detach-keys, --sig-proxy          |
| exec       | exec              | Run a command in a running container                                 | --privileged, --user, --detach-keys |
| images     | images            | List images                                                          |                                     |
| info       | info              | Display system-wide information                                      |                                     |
| inspect    | inspect, inspecti | Return low-level information on a container, image or task           |                                     |
| logs       | logs              | Fetch the logs of a container                                        | --details                           |
| ps         | ps                | List containers                                                      |                                     |
| stats      | stats             | Display a live stream of container(s) resource usage statistics      | Column: NET/BLOCK I/O, PIDs         |
| version    | version           | Show the runtime (Docker, ContainerD, or others) version information |                                     |

So if we look at the comparisons here, you can see that `ctr` and `crictl` are used mainly for debugging purposes, whereas the `nerdctl` is used for general purpose. The `ctr` and `nerdctl` are from the containerd community and work with containerd, whereas `crictl` is from the Kubernetes community and works across all CRI compatible runtimes.
So our labs originally had Docker installed on all the nodes so we used the Docker commands to troubleshoot, but now it’s all containerd so remember to use the `crictl` command instead to troubleshoot.

![](../../images/02-03-10.png)





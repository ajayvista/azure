
# Azure Kubernetes Service (AKS)

The Azure Kubernetes Service (AKS) is a fully managed container orchestration system that makes it easier for teams to run containers in production.

■ Azure Container Instances have no dependencies on an App Service Plan.
■ Azure provides rich support for Docker containers and images can be built and stored in the Azure Container Registry (ACR).

A Kubernetes deployment is configured as a cluster. A cluster consists of at least one master machine and one or more workers machines.  For production deployments, the preferred master configuration is a multi-master high availability deployment with three to five replicated masters. These machines can be physical hardware or VMs. Theses worker machines are called nodes or agent nodes.

Kubernetes is sometimes abbreviated to K8s. The 8 represents the eight characters between the K and the s of the word K[ubernete]s.

The Node Size of a Kubernetes cluster cannot be changed once a Kubernetes cluster has been created.

The Node Count, HTTP Application Routing and SSH Node Access can all be changed after the Kubernetes cluster has been created.


**Azure Container Registry**

Azure Container Registry is a registry hosting service provided by Azure. Each Azure Container Registry resource you create is a separate registry with a unique URL. These registries are private: they require authentication to push or pull images. Azure Container Registry runs in the cloud, and provides similar levels of scalability and availability to many other Azure services.

**Kube-apiserver** - this is the API server and is how the underlying Kubernetes APIs are exposed.  This component provides the interaction for management tools, such as kubectl or the Kubernetes dashboard.

**etcd** - allows you to maintain the state of your Kubernetes cluster and configuration, the highly available etcd is a key value store within Kubernetes.

**kube-scheduler** - when you create or scale applications, the scheduler determines what nodes can run the workload and starts them.

Azure Dev Spaces is supported only by AKS clusters in specific regions.

With Azure DevOps Projects you can:

- Automatically create Azure resources, such as an AKS cluster
- Create an Azure Application Insights resource for monitoring an AKS cluster
- Enable Azure Monitor for containers to monitor performance for the container workloads on an AKS cluster
- You can add richer DevOps capabilities by extending the default configured DevOps pipeline. For example, you can add approvals before deploying, provision additional Azure resources, run scripts or upgrade workloads.

- Identity and security management | You can configure an AKS cluster to integrate with Azure AD and reuse existing identities and group membership.
- Integrated logging and monitoring  | AKS includes Azure Monitor for containers to provide performance visibility of the cluster.

Auto Cluster node and pod scaling | Deciding when to scale up or down in large containerization environment is tricky. AKS supports two auto cluster scaling options. You can use either the horizontal pod autoscaler or the cluster autoscaler to scale the cluster. The horizontal pod autoscaler watches the resource demand of pods and will increase pods to match demand. The cluster autoscaler component watches for pods that can't be scheduled because of node constraints. It will automatically scale cluster nodes to deploy scheduled pods.

Prior to releasing the Azure Kubernetes Service (AKS), Microsoft offered the Azure Container Service (ACS) as a managed solution that provided multiple orchestration systems as a service, including Kubernetes, Docker Swarm, and DC/OS. ACS has been deprecated, and existing ACS customers will need to migrate to AKS.

```sh
kubectl apply f appl.yaml applies a configuration change to a resource from a file or stdin.

kubectl get nodes gets a list of all nodes.

az aks install-cli download and install the Kubernetes command-line tool.

az aks get-credentials gets access credentials for a managed Kubernetes cluster
```

## Docker

Docker was initially developed for Linux and has expanded to support Windows. Individual Docker images are either Windows-based or Linux-based, but can't be both at the same time. The operating system of the image determines what kind of operating system environment is used inside the container.

Linux computers with Docker installed can only run Linux containers. Windows computers with Docker installed can run both kinds of containers. Windows accomplishes this by using a virtual machine to run a Linux system and uses the virtual Linux system to run Linux containers.

﻿An Azure container registry stores and manages private Docker container images, similar to the way Docker Hub stores public Docker images. You can use the Docker command-line interface (Docker CLI) for login, push, pull, and other operations on your container registry. (ex- docker registry1.azurecr.io)

A registry is a web service to which Docker can connect to upload and download container images.  A registry is organized as a series of repositories. Each repository contains multiple Docker images that share a common name and generally the same purpose and functionality. These images normally have different versions identified with a tag. This mechanism enables you to publish and retain multiple versions of images for compatibility reasons. When you download and run an image, you must specify the registry, repository, and version tag for the image.

A repository is also the unit of privacy for an image. If you don't wish to share an image, you can make the repository private. You can grant access to other users with whom you want to share the image.

By default, Docker doesn't allow inbound network requests to reach your container. You need to tell Docker to assign a specific port number from your computer to a specific port number in the container by adding the -p option to docker run. This instruction enables network requests to the container on the specified port. Use the -d flag to instruct Docker to start the app in the background. docker ps is a shortcut for docker container ls. The names of these commands are based on the Linux utilities ps and ls, which list running processes and files, respectively. docker ps is a shortcut for docker container ls. The names of these commands are based on the Linux utilities ps and ls, which list running processes and files, respectively.

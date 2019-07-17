### What is artifactory?

  Refer to [here](https://devops.stackexchange.com/questions/1898/what-is-an-artifactory)
  
  - artifacts: the outcome of build process (stored in binary repository)
  - Artifactory: a binary repository manager product and it manages store of artifacts
  
  - Most of the times one would not use the binary repository directly but through a package manager that comes with the chosen technology. The binary repository can store individual application components that can later be assembled into a full product. It can make more efficient use of resources, reduce build times, better track binary debug databases etc.
  
  - Some of the popular package managers that use binary repository:
    - maven
    - ivy
    - gradle
    - nuget
    - npm
    - pip
  
 ### What is Kubernetes
 Refer to [here](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
 
  - Kubernetes helps you make sure containerized applications run where and when you want and helps them find the resources and tools they need to work
  - Kubernetes is a production-ready, open source platform designed with Google's accumulated experience in container orchestration, combined with best-of-breed ideas from the community.
  - there is master node: handles scheduling the pds across the Nodes in the cluster
  - Node (worker node)
    - each node has a Kubelet which is an agent for managing the node and communicate with the Kubernetes master
    - a worker node has multiple pods
    - each pod has a set of containers (such as docker container)
    - the nodes communicate with the master using the Kubernetes API
  - end user can also use the Kubernetes API directly to interact with the cluster
  - A kubernetes cluster can be deployed on either physical or virtual machines (using Minikube). Minikube is a lightweight imlementation that creates a VM on local machine that deploys a simple cluster containing only one node.
  - Some commands of minikube:
    - minikube version
    - minikube start: start the cluster. It starts a virtual machine and a kubernetes cluster is now running in that VM
  - Kubectl:
    - used to interact with Kubernetes (more detail in later section)
    - kubectl version: returns client and server version. The client version is kubectl version; the server version is Kubernetes version installed on the master
    - kubectl cluster-info: view cluster details such as where is kubernetes master and kubeDNS running
    - kubectl get nodes: shows all nodes that can be used to host our application
  - Kubernetes deployments:
    - once you have a running Kubernetes cluster, you can deploy your containerized applications on top of it
    - Kubernetes master schedules mentioned application instances onto individual Nodes in the cluster and provide a self-heading mechanism exists to address machine failure
    - when you create a deployment, you need to specify the container image for your application and the number of replicas that you want to run
    - *kubectl run kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1 --port=8080*
      The format is kubectl run <deployment name> <app image location> <optional port>
        - the command above searches for a suitable node where an instance of the application could run
        - schedule the application to run on that Node
        - configure the cluster to reschedule the instance on a new Node when needed
    - *kubectl get deployments*: list all the deployments
      - each deployment is running inside a Docker container on the node
    

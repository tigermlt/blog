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
    - ```minikube version```
    - ```minikube start```: start the cluster. It starts a virtual machine and a kubernetes cluster is now running in that VM
  - Kubectl:
    - used to interact with Kubernetes (more detail in later section)
    - ```kubectl version```: returns client and server version. The client version is kubectl version; the server version is Kubernetes version installed on the master
    - ```kubectl cluster-info```: view cluster details such as where is kubernetes master and kubeDNS running
    - ```kubectl get nodes```: shows all nodes that can be used to host our application
  - Kubernetes deployments:
    - once you have a running Kubernetes cluster, you can deploy your containerized applications on top of it
    - Kubernetes master schedules mentioned application instances onto individual Nodes in the cluster and provide a self-heading mechanism exists to address machine failure
    - when you create a deployment, you need to specify the container image for your application and the number of replicas that you want to run
    - ```kubectl run kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1 --port=8080```
      The format is kubectl run <deployment name> <app image location> <optional port>
        - the command above searches for a suitable node where an instance of the application could run
        - schedule the application to run on that Node
        - configure the cluster to reschedule the instance on a new Node when needed
    - ```kubectl get deployments```: list all the deployments
      - each deployment is running inside a Docker container on the node
      - The READY column shows the ratio of CURRENT to DESIRED replicas
        - CURRENT is the number of replicas running now
        - DESIRED is the configured number of replicas
      - The UP-TO-DATE is the number of replicas that were updated to match the desired (configured) state
      - The AVAILABLE state shows how many replicas are actually AVAILABLE to the users
    - pods running inside Kubernetes are running on a private, isolated network. They are only visible from other pods and services within the same kubernetes cluster. 
    - kubectl can create a proxy that will forward communications into a cluster-wide, private network. 
      - ```kubectl proxy```
      - can make query to the proxy: ```curl http://localhost:8001/version```
  - Viewing pods and Nodes
    Pods:
      - A pod is a kubernetes abstraction that represents a group of one or more application containers (Docker or rkt) and some shared resources for those containers for instance:
        - shared storage, as Volumes
        - networking, as a unique cluster IP address
        - information about how to run each container
      - each pod is tied to the Node where it is scheduled and remains there until termination or deletion. In case of Node failure, identical pods are scheduled on other available Nodes in the cluster.
    Nodes:
      - A Node can have many pods
      - every Node at least run:
        - Kubelet, a process responsible for communication between the Kubernetes Master and the Node. It manages the pods and the containers running on a machine
        - A container runtime (like Docker, rkt) responsible for pulling the container image from a registry, unpacking the container and running the application
      - troubleshooting with kubectl:
        - ```kubectl get``` - list resources
        - ```kubectl describe``` - show detailed information about a resource
        - ```kubectl logs``` - print the logs from a container in a pod
        - ```kubectl exec``` - execute a command on a container in a pod
        - ```kubectl get pods``` - look for existing pods
        - ```kubectl describe pods``` - view what containers are inside that Pod and what images are used to build those containers
      - execute commands on the container
        - ```kubectl exec $POD_NAME env```: list the environment variables in the specified pod
        - ```kubectl exec -ti $POD_NAME bash```: start a bash session in the pod's container
  - Kubernetes service
    - when a worker node dies, the pods running on the Node are also lost. A replicasetmight then dynamically drive the cluster back to the desiried state via creation of new pods.
    - A service is defined using YAML or JSON
    - Each pod has a unique IP address, those IPs are not exposed outside the cluster without a service. Service allow your applications to receive traffic
    - A Service routes traffic across a set of Pods. Services are the abstraction that allow pods to die and replicate in Kubernetes without impacting your application
    - Services match a set of pods using labels and selectors, a grouping primitive that allows logical operation on objects in Kubernetes. Labels are key/value pairs attached to objects.
    - ```kubectl get services```: list the current services from our cluster
    - ```kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080```: create a new service and expose to external traffic
    - ```kubectl describe services/kubernetes-bootcamp```: return information about the service like port number, IP address etc
    - ```kubectl describe deployment```: see the name of the label of a deployment
      - query list of pods using the label name: ```kubectl get pods -l run=<label_name>```
      - query list of services using the label name: ```kubectl get services -l run=<label_name>```
      - change label of a pod: ```kubectl label pod $POD_NAME app=<new_label_name>```
      - ```ubectl delete service -l run=<service_name>```: delete service
  - scalling an application:
    - scaling out a deployment will ensure new pods are created and scheduled to Nodes with available resources
    - services have an integrated load-balancer that will distribute network traffic to all pods of an exposed deployment
    - services will monitor continuously the running pods using endpoints, to ensure the traffic is sent only to available pods
    - scale the deployment to 4 replicas: ```kubectl scale deployments/kubernetes-bootcamp --replicas=4```. If you give a smaller number of replicas than current is, it can scale down the service
  - Rolling update:
    - rolling updates allow deployments' update to take place with zero downtime by incrementally updating pods instances with new ones. This is good because users expect applications to be available all the time and developers are expected to deploy new versions of them several times a day
      - it allows promote an application from one environment to another
      - it allows rooback to previous versions
      - it allows continuous integration and continuous delivery of applications with zero downtime
    - scale up and down does not affect application availability
    - if a deployment is exposed publicly, the service will load-balance the traffic only to available pods during the update
    - ```kubectl rollout undo <deployment name or tag>```: roll back to the previous working version of the deployment
### Indexing and index structures
  - refer to [here](https://www.inf.unibz.it/~artale/DB2/handout2.pdf)
  - indexing is the principal technique used to efficiently answering a given query (minimizing the number of disk accesses)
  - an index structure is usually defined on a single attribute of a relation, called the **search key**
    - it is | search key | pointer to a data-file record|
    - the search key values stored in the index are sorted and a binary search can be done on the index
  - indexes on sequential files: dense vs sparse indexes
    - index on sequential file is called **primary index**, as mentioned before index points to data file and the search key that used to build index is sorted
    - usually these indexes fit in main memory
    - primary index can be :
      - **dense**: for every record in the data file, there is a one entry in the index that points to it
        - given a search key K, the index is scanned and found (binary search can be used since search key is sorted), the corresponding data file is then read into main memory
        - it also supports range queries: the minimum value is located first and consecutive blocks are loaded in main memory until a search key greater than the maximum value is found
        - since the index is usually kept in main memory, only 1 I/O is needed during lookup
      - **sparse**: one entry in the index points to a block of data file (usually the first record on the data block)
        - given a search key, search the sparse index for the greatest key <= to K using binary search; load the entire pointed block to main memory and then use binary search to look for the specific record
        - still one disk I/O for lookup, more efficient in space at the cost of a worst computing time in main memory
                                                                    
  
  

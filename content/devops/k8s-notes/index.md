---
title: 'Kubernetes Notes'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: A free style notes of Kubernetes.
authors:
- admin
tags:
- Linux
- System
- Kubernetes
- K8S
categories:
- Linux
- System
- DevOps
- SRE
date: "2019-12-31T23:59:59Z"
lastmod: "2020-01-01T00:00:00Z"
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Placement options: 1 = Full column width, 2 = Out-set, 3 = Screen-width
# Focal point options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
image:
  placement: 2
  caption: ''
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---


## K8S NOTES

### Kubernetes Components

- Cluster  
    * A cluster is a set of machines, called nodes, that run containerized applications managed by Kubernetes.  
    * A cluster has at least one worker node and at least one master node.  
- Node  
    * Master node(s)  
    The master node(s) manages the worker nodes and the pods in the cluster. Multiple master nodes are used to provide a cluster with failover and high availability.  
    * Worker node(s)  
    The worker node(s) host the pods that are the components of the application.  

#### Master Node Components
- kube-apiserver  
- etcd  
- kube-scheduler  
- kube-controller-manager  
    * Node Controller  
    * Replication Controller  
    * Endpoints Controller  
    * Service Account & Token Controllers  
- cloud-controller-manager  
The following controllers have cloud provider dependencies:  
    * Node Controller  
    * Route Controller  
    * Service Controller  
    * Volume Controller  

#### Worker Node Components
- Kubelet  
- kube-proxy  
- Container Runtime  

#### Addons
- DNS
- Web UI (Dashboard)
- Container Resource Monitoring
- Cluster-level Logging

### K8S Abbreviation

#### CNI

Kubernetes has adopted the Container Network Interface(CNI) specification for managing network resources on a cluster.  

#### CRI
Container Runtime Interface (CRI)  
Each container runtime has it own strengths, and many users have asked for Kubernetes to support more runtimes. CRI consists of a protocol buffers and gRPC API, and libraries, with additional specifications and tools under active development.  
Kubelet communicates with the container runtime (or a CRI shim for the runtime) over Unix sockets using the gRPC framework, where kubelet acts as a client and the CRI shim as the server.  
![cri.png](./cri.png)

- Docker
- CRI-O
- Containerd
- Other CRI runtimes: frakti

#### CSI

The goal of CSI is to establish a standardized mechanism for Container Orchestration Systems (COs) to expose arbitrary storage systems to their containerized workloads. 
Assuming a CSI storage plugin is already deployed on a Kubernetes cluster, users can use CSI volumes through the familiar Kubernetes storage API objects:  
- PersistentVolumeClaims  
- PersistentVolumes  
- StorageClasses  

#### CRD

CustomResourceDefinition


### K8S Networking

#### POD Communication

- Inner POD  
Multi-containers in one POD  
Containers within a pod share an IP address and port space, and can find each other via localhost. They can also communicate with each other using standard inter-process communications.   
    * Shared volume  
    * IPC, i.e. queue  
    * Networking, localhost with different port  

- Inter PODs  


### Services

#### ClusterIP

ClusterIP accesses the services through proxy.  
ClusterIP can access services only inside the cluster.  
![clusterip](./clusterip.png)  

#### Nodeport

NodePort opens a specific port on each node of the cluster and traffic on that node is forwarded directly to the service.  
![nodeport](./nodeport.png) 

#### Loadbalancer

All the traffic on the port is forwarded to the service, there's no filtering, no routing.  
![loadbalancer](./loadbalancer.png)  


### Ingress Controller

Ingress Controller but there are third party solutions like **Traefik** and **Nginx** available. Ingress controller also provide L7 load balancing unlike cluster services.  

### Network policies

Isolation policies are configured on a per-namespace basis.  
[Kubernetes Networking](https://cloudnativelabs.github.io/post/2017-04-18-kubernetes-networking/)  


### K8S Objects
To find all the objects in some specific API version:  
- kubectl api-resources | cut -c92-  
- kubectl api-resources | cut -c92-150  
- kubectl api-resources | cut -c92-150 | wc -l  
- kubectl api-resources  

#### Pod

A thin wrapper around one or more containers  

#### DaemonSet

Implements a single instance of a pod on a worker node

#### Deployment

Details how to roll out (or roll back) across versions of your application

#### ReplicaSet

Ensures a defined number of pods are always running

#### Job

Ensures a pod properly runs to completion

#### Service

Maps a fixed IP address to a logical group of pods

#### Label

Key/Value pairs used for association and filtering  
Labels in Kubernetes are intended to be used to specify identifying attributes for objects. They are used by selector queries or with label selectors. Since they are used internally by Kubernetes, the structure of keys and values is constrained, to optimize queries.

#### Annotations
annotations are a way to attach non-identifying metadata to objects. This metadata is not used internally by Kubernetes, so they cannot be used to identify within k8s. Instead, they are used by external tools and libraries. Examples of annotations include build/release timestamps, client library information for debugging, or fields managed by a network policy like Calico in this case.  

#### Label vs. Annotation

You can use either labels or annotations to attach metadata to Kubernetes objects. Labels can be used to select objects and to find collections of objects that satisfy certain conditions. In contrast, annotations are not used to identify and select objects.  

#### Taints and Tolerations

Node affinity, described here, is a property of pods that attracts them to a set of nodes (either as a preference or a hard requirement). Taints are the opposite – they allow a node to repel a set of pods.

Taints and tolerations work together to ensure that pods are not scheduled onto inappropriate nodes. One or more taints are applied to a node; this marks that the node should not accept any pods that do not tolerate the taints. Tolerations are applied to pods, and allow (but do not require) the pods to schedule onto nodes with matching taints.

#### Node isolation/restriction

Adding labels to Node objects allows targeting pods to specific nodes or groups of nodes. This can be used to ensure specific pods only run on nodes with certain isolation, security, or regulatory properties.

#### NODESELECTOR
1. Attach a label to the node
2. Add a nodeSelector field to your pod configuration

#### Affinity and anti-affinity

    • Node affinity
    • Inter-pod affinity and anti-affinity
nodeSelector provides a very simple way to constrain pods to nodes with particular labels. The affinity/anti-affinity feature, currently in beta, greatly extends the types of constraints you can express. The key enhancements are:

    • The language is more expressive (not just “AND of exact match”)
    • You can indicate that the rule is “soft”/“preference” rather than a hard requirement, so if the scheduler can’t satisfy it, the pod will still be scheduled
    • You can constrain against labels on other pods running on the node (or other topological domain), rather than against labels on the node itself, which allows rules about which pods can and cannot be co-located

The affinity feature consists of two types of affinity, “node affinity” and “inter-pod affinity/anti-affinity”. Node affinity is like the existing nodeSelector (but with the first two benefits listed above), while inter-pod affinity/anti-affinity constrains against pod labels rather than node labels, as described in the third item listed above, in addition to having the first and second properties listed above.

Node affinity is conceptually similar to nodeSelector – it allows you to constrain which nodes your pod is eligible to be scheduled on, based on labels on the node.



### Deploy

When you wish to deploy an application in Kubernetes, you usually define three components:  
- a **Deployment** — which is a recipe for creating copies of your application called Pods  
- a **Service** — an internal load balancer that routes the traffic to Pods  
- an **Ingress** — a description of how the traffic should flow from outside the cluster to your Service  


### Ingress

#### What is an ingress?

In Kubernetes, an Ingress is an object that allows access to your Kubernetes services from outside the Kubernetes cluster. You configure access by creating a collection of rules that define which inbound connections reach which services.  
<br>
Ingress, added in Kubernetes v1.1, exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.  
<br>
Internet---[ Ingress ]--|--|--[ Services ]
<br>

An Ingress can be configured to give services externally-reachable URLs, load balance traffic, terminate SSL, and offer name based virtual hosting. An Ingress controller is responsible for fulfilling the Ingress, usually with a loadbalancer, though it may also configure your edge router or additional frontends to help handle the traffic.  

An Ingress does not expose arbitrary ports or protocols. Exposing services other than HTTP and HTTPS to the internet typically uses a service of type NodePort or LoadBalancer.  
#### What is an ingress controller?

Kubernetes supports a high level abstraction called Ingress, which allows simple host or URL based HTTP routing. An ingress is a core concept (in beta) of Kubernetes, but is always implemented by a third party proxy. These implementations are known as ingress controllers.  
<br>
In order for the Ingress resource to work, the cluster must have an ingress controller running.  
Unlike other types of controllers which run as part of the kube-controller-manager binary, Ingress controllers are not started automatically with a cluster. Let’s see some options:  
- ALB Ingress Controller  


#### Ingress vs. Ingress controller

- Ingress should be the rules for the traffic, which indicate the destination of a request will go through in the cluster.  
- Ingress Controller is the implementation for the Ingress. GCE and Nginx are both supported by k8s. They will take care of L4 or L7 proxy.   
- Just like other objects in K8s, ingress is also a type of object, which is mainly referred as set of redirection rules.  
- Where as ingress controller is like other deployment objects(could be deamon set as well) which listen and configure those ingress rules.  
- If I talk in terms of Nginx, Ingress controller is Nginx software itself where as ingress(ingress rules) are nginx configurations.  

#### The Ingress Resource

A minimal ingress resource example:  
![ingress-resource-examle](./ingress-resource-examle.png)  

As with all other Kubernetes resources, an Ingress needs apiVersion, kind, and metadata fields. Ingress frequently uses annotations to configure some options depending on the Ingress controller, an example of which is the rewrite-target annotation. Different Ingress controller support different annotations. Review the documentation for your choice of Ingress controller to learn which annotations are supported.  
The Ingress spec has all the information needed to configure a loadbalancer or proxy server. Most importantly, it contains a list of rules matched against all incoming requests. Ingress resource only supports rules for directing HTTP traffic.  

#### Ingress Rules

Each http rule contains the following information:  
- An optional host. In this example, no host is specified, so the rule applies to all inbound HTTP traffic through the IP address specified. If a host is provided (for example, foo.bar.com), the rules apply to that host.  
- A list of paths (for example, /testpath), each of which has an associated backend defined with a serviceName and servicePort. Both the host and path must match the content of an incoming request before the loadbalancer will direct traffic to the referenced service. 
- A backend is a combination of service and port names as described in the services doc. HTTP (and HTTPS) requests to the Ingress matching the host and path of the rule will be sent to the listed backend.  
- A default backend is often configured in an Ingress controller that will service any requests that do not match a path in the spec.  

#### Default Backend

An Ingress with no rules sends all traffic to a single default backend. The default backend is typically a configuration option of the Ingress controller and is not specified in your Ingress resources.  

If none of the hosts or paths match the HTTP request in the Ingress objects, the traffic is routed to your default backend.


### ConfigMap

[Ultimate Guide to ConfigMaps in Kubernetes](https://matthewpalmer.net/kubernetes-app-developer/articles/ultimate-configmap-guide-kubernetes.html)  




















### Kubernetes Operator

An Operator is an application-specific controller that extends the Kubernetes API to create, configure, and manage instances of complex stateful applications on behalf of a Kubernetes user. It builds upon the basic Kubernetes resource and controller concepts but includes domain or application-specific knowledge to automate common tasks.  
Examples are:  
- elasticsearch-operator 
- prometheus-operator
- cassandra-operator
- Flux  The GitOps Kubernetes Operator

{{% alert note %}}
Always use the array syntax when using CMD and ENTRYPOINT.
{{% /alert %}}


 

## Links

[The Difference between COPY and ADD in a Dockerfile](https://nickjanetakis.com/blog/docker-tip-2-the-difference-between-copy-and-add-in-a-dockerile)  

<br>

#### Did you find this page helpful? Consider sharing it 🙌

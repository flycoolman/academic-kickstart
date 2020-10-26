---
title: 'Kubernetes Best Practices'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: A summary of best practices of Kubernetes.
authors:
- admin
tags:
- System
- Kubernetes
- K8S
categories:
- System
- DevOps
- SRE
date: "2020-10-01T00:00:00Z"
lastmod: "2020-10-10T00:00:00Z"
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


## Kubernetes Best Practices

### Best Practices for Image Management

- Build your images you base them on only well-known and trusted image providers.  
- Some combination of the semantic version and the SHA hash of the commit where the image was built is a good practice for naming images (e.g., v1.0.1-bfeda01f).  
- It is a bad idea for production usage because latest is clearly being mutated every time a new image is built.  
  
### Deploying Services Best Practices

- Most services should be deployed as **Deployment resources**. Deployments create identical replicas for redundancy and scale.  
- Deployments can be exposed using a Service, which is effectively a load balancer. A Service can be exposed either within a cluster (the default) or externally. If you want to expose an HTTP application, you can use an Ingress controller to add things like request routing and SSL.  
- Eventually you will want to **parameterize** your application to make its configuration more reusable in different environments. Packaging tools like [Helm](https://helm.sh/) are the best choice for this kind of parameterization.  

### Setting Up a Development Environment Best Practices
- Think about developer experience in three phases: onboarding, developing, and testing. Make sure that the development environment you build supports all three of these phases.  
- When building a development cluster, you can choose between one large cluster and a cluster per developer. There are pros and cons to each, but generally a single large cluster is a better approach.  
- When you add users to a cluster, add them with their own identity and access to their own namespace. Use resource limits to restrict how much of the cluster they can use.  
- When managing namespaces, think about how you can reap old, unused resources. Developers will have bad hygiene about deleting unused things. Use automation to clean it up for them.  
- Think about cluster-level services like logs and monitoring that you can set up for all users. Sometimes, cluster-level dependencies like databases are also useful to set up on behalf of all users using templates like Helm charts.  


### Best Practices for Monitoring, Logging, and Alerting
Following are the best practices that you should adopt regarding monitoring, logging, and alerting.

#### Monitoring
- Monitor nodes and all Kubernetes components for utilization, saturation, and error rates, and monitor applications for rate, errors, and duration.  
- Use black-box monitoring to monitor for symptoms and not predictive health of a system.  
- Use white-box monitoring to inspect the system and its internals with instrumentation.  
- Implement time-series-based metrics to gain high-precision metrics that also allow you to gain insight within the behavior of your application.  
- Utilize monitoring systems like Prometheus that provide key labeling for high dimensionality; this will give a better signal to symptoms of an impacting issue.  
- Use average metrics to visualize subtotals and metrics based on factual data. Utilize sum metrics to visualize the distribution across a specific metric.  

#### Logging
- You should use logging in combination with metrics monitoring to get the full picture of how your environment is operating.  
- Be cautious of storing logs for more than 30 to 45 days and, if needed, use cheaper resources for long-term archiving.  
- Limit usage of log forwarders in a sidecar pattern, as they will utilize a lot more resources. Opt for using a DaemonSet for the log forwarder and sending logs to STDOUT.  

### Alerting
- Be cautious of alert fatigue because it can lead to bad behaviors in people and processes.  
- Always look at incrementally improving upon alerting and accept that it will not always be perfect.  
- Alert for symptoms that affect your SLO and customers and not for transient issues that don’t need immediate human attention.  

{{% alert note %}}
- It’s a good practice to monitor your cluster from a “utility cluster” to avoid a production issue also affecting your monitoring system.  
- Black-box monitoring gives you symptoms.  
- White-box monitoring gives you "Why".  
- The **USE** and **RED** methods are complementary to each other given that the USE method focuses on the infrastructure components and the RED method focuses on monitoring the end-user experience for the application.  
- **cAdvisor** and **metrics server** are used to provide detailed metrics on resource usage, **kube-state-metrics** is focused on identifying conditions on Kubernetes objects deployed to your cluster.  
{{% /alert %}}

### Best Practices for ConfigMaps and Secrets

- To support dynamic changes to your application without having to redeploy new versions of the pods, mount your ConfigMaps/Secrets as a volume and configure your application with a file watcher to detect the changed file data and reconfigure itself as needed.  
- ConfigMap/Secrets must exist in the namespace for the pods that will consume them prior to the pod being deployed. The optional flag can be used to prevent the pods from not starting if the ConfigMap/Secret is not present.  
- Use an admission controller to ensure specific configuration data or to prevent deployments that do not have specific configuration values set. An example would be if you require all production Java workloads to have certain JVM properties set in production environments. There is an alpha API called PodPresets that will allow ConfigMaps and secrets to be applied to all pods based on an annotation, without needing to write a custom admission controller.  
- If you’re using Helm to release applications into your environment, you can use a life cycle hook to ensure the ConfigMap/Secret template is deployed before the Deployment is applied.  
- Some applications require their configuration to be applied as a single file such as a JSON or YAML file. ConfigMap/Secrets allows an entire block of raw data by using the | symbol.  
- If the application uses system environment variables to determine its configuration, you can use the injection of the ConfigMap data to create an environment variable mapping into the pod. There are two main ways to do this: mounting every key/value pair in the ConfigMap as a series of environment variables into the pod using envFrom and then using configMapRef or secretRef, or assigning individual keys with their respective values using the configMapKeyRef or secretKeyRef.  
- If you’re using the configMapKeyRef or secretKeyRef method, be aware that if the actual key does not exist, this will prevent the pod from starting.  
- If you’re loading all of the key/value pairs from the ConfigMap/Secret into the pod using envFrom, any keys that are considered invalid environment values will be skipped; however, the pod will be allowed to start. The event for the pod will have an event with reason InvalidVariableNames and the appropriate message about which key was skipped. The following code is an example of a Deployment with a ConfigMap and Secret reference as an environment variable  
- If there is a need to pass command-line arguments to your containers, environment variable data can be sourced using $(ENV_KEY) interpolation syntax.  
- When consuming ConfigMap/Secret data as environment variables, it is very important to understand that updates to the data in the ConfigMap/Secret will not update in the pod and will require a pod restart either through deleting the pods and letting the ReplicaSet controller create a new pod, or triggering a Deployment update, which will follow the proper application update strategy as declared in the Deployment specification.  
- It is easier to assume that all changes to a ConfigMap/Secret require an update to the entire deployment; this ensures that even if you’re using environment variables or volumes, the code will take the new configuration data. To make this easier, you can use a CI/CD pipeline to update the name property of the ConfigMap/Secret and also update the reference in the deployment, which will then trigger an update through normal Kubernetes update strategies of your deployment. We will explore this in the following example code. If you’re using Helm to release your application code into Kubernetes, you can take advantage of an annotation in the Deployment template to check the sha256 checksum of the ConfigMap/Secret. This triggers Helm to update the Deployment using the helm upgrade command when the data within a ConfigMap/Secret is changed.  

### Best Practices Specific to Secrets
- Use 3rd party solutions to allow the use of external storage systems for secret data, such as HashiCorp Vault, Aqua Security, Twistlock, AWS Secrets Manager, Google Cloud KMS, or Azure Key Vault  
- Assign an imagePullSecrets to a serviceaccount that the pod will use to automatically mount the secret without having to declare it in the pod.spec.  
- Use CI/CD capabilities to get secrets from a secure vault or encrypted store with a Hardware Security Module (HSM) during the release pipeline.  

### RBAC Best Practices

- Applications that are developed to run in Kubernetes rarely ever need an RBAC role and role binding associated to it. Only if the application code actually interacts directly with the Kubernetes API directly does the application require RBAC configuration.  
- If the application does need to directly access the Kubernetes API to perhaps change configuration depending on endpoints being added to a service, or if it needs to list all of the pods in a specific namespace, the best practice is to create a new service account that is then specified in the pod specification. Then, create a role that has the least amount of privileges needed to accomplish its goal.  
- Use an OpenID Connect service that enables identity management and, if needed, two-factor authentication. This will allow for a higher level of identity authentication. Map user groups to roles that have the least amount of privileges needed to accomplish the job.  
- Along with the aforementioned practice, you should use Just in Time (JIT) access systems to allow site reliability engineers (SREs), operators, and those who might need to have escalated privileges for a short period of time to accomplish a very specific task. Alternatively, these users should have different identities that are more heavily audited for sign-on, and those accounts should have more elevated privileges assigned by the user account or group bound to a role.  
- Specific service accounts should be used for CI/CD tools that deploy into your Kubernetes clusters. This ensures for auditability within the cluster and an understanding of who might have deployed or deleted any objects in a cluster.  
- Limit any applications that require watch and list on the Secrets API. This basically allows the application or the person who deployed the pod to view the secrets in that namespace. If an application needs to access the Secrets API for specific secrets, limit using get on any specific secrets that the application needs to read outside of those that it is directly assigned.

{{% alert note %}}
Service accounts in Kubernetes are different than user accounts in that they are namespace bound, internally stored in Kubernetes; they are meant to represent processes, not people, and are managed by native Kubernetes controllers.
{{% /alert %}}

### Best Practices for CI/CD

- With CI, focus on automation and providing quick builds. Optimizing the build speed will provide developers quick feedback if their changes have broken the build.  
- Focus on providing reliable tests in your pipeline. This will give developers rapid feedback on issues with their code. The faster the feedback loop to developers, the more productive they’ll become in their workflow.  
- When deciding on CI/CD tools, ensure that the tools allow you to define the pipeline as code. This will allow you to version-control the pipeline with your application code.  
- Ensure that you optimize your images so that you can reduce the size of the image and also reduce the attack surface when running the image in production. Multistage Docker builds allow you to remove packages not needed for the application to run. For example, you might need Maven to build the application, but you don’t need it for the actual running image.  
- Avoid using “latest” as an image tag, and utilize a tag that can be referenced back to the buildID or Git commit.  
- If you are new to CD, utilize Kubernetes rolling upgrades to start out. They are easy to use and will get you comfortable with deployment. As you become more comfortable and confident with CD, look at utilizing blue/green and canary deployment strategies.  
- With CD, ensure that you test how client connections and database schema upgrades are handled in your application.  
- Testing in production will help you build reliability into your application, and ensure that you have good monitoring in place. With testing in production, also start at a small scale and limit the blast radius of the experiment.  

{{% alert note %}}
Including both application code and configuration code, such as a Kubernetes manifest or Helm charts, helps promote good DevOps principles of communication and collaboration. Having both application developers and operation engineers collaborate in a single repository builds confidence in a team to deliver an application to production.
{{% /alert %}}

#### Build the smallest image

Build the smallest image possible for your application:
- Multistage builds  
- Distroless base images  
- Optimized base images  

#### Container Image Tagging

- BuildID  
- Build System-BuildID  
- Git Hash  
- Githsah-buildID  

#### Deployment Strategies  

- Rolling updates  
- Blue/green deployments  
- Canary deployments  

### Worldwide Rollout Best Practices

- Distribute each image around the world. A successful rollout depends on the release bits (binaries, images, etc.) being nearby to where they will be used. This also ensures reliability of the rollout in the presence of networking slowdowns or irregularities. Geographic distribution should be a part of your automated release pipeline for guaranteed consistency.  
- Shift as much of your testing as possible to the left by having as much extensive integration and replay testing of your application as possible. You want to start a rollout only with a release that you strongly believe to be correct.  
- Begin a release in a canary region, which is a preproduction environment in which other teams or large customers can validate their use of your service before you begin a larger-scale rollout.  
- Identify different characteristics of the regions where you are rolling out. Each difference can be one that causes a failure and a full or partial outage. Try to roll out to low-risk regions first.  
- Document and practice your response to any problem or process (e.g., a rollback) that you might encounter. Trying to remember what to do in the heat of the moment is a recipe for forgetting something and making a bad problem worse.  

### Resource Management Best Practices

- Utilize pod anti-affinity to spread workloads across multiple availability zones to ensure high availability for your application.  
- If you’re using specialized hardware, such as GPU-enabled nodes, ensure that only workloads that need GPUs are scheduled to those nodes by utilizing taints.
- Use NodeCondition taints to proactively avoid failing or degraded nodes.  
- Apply nodeSelectors to your pod specifications to schedule pods to specialized hardware that you have deployed in the cluster.  
- Before going to production, experiment with different node sizes to find a good mix of cost and performance for node types.  
- If you’re deploying a mix of workloads with different performance characteristics, utilize node pools to have mixed node types in a single cluster.  
- Ensure that you set memory and CPU limits for all pods deployed to your cluster.  
- Utilize ResourceQuotas to ensure that multiple teams or applications are alotted their fair share of resources in the cluster.  
- Implement LimitRange to set default limits and requests for pod specifications that don’t set limits or requests.  
- Start with manual cluster scaling until you understand your workload profiles on Kubernetes. You can use autoscaling, but it comes with additional considerations around node spin-up time and cluster scale down.  
- Use the HPA for workloads that are variable and that have unexpected spikes in their usage.  

### Services in Kubernetes

Service Types:  

- ClusterIP  
- NodePort  
- ExternalName  
- LoadBalancer  


### Services and Ingress Controllers Best Practices

- Limit the number of services that need to be accessed from outside the cluster. Ideally, most services will be ClusterIP, and only external-facing services will be exposed externally to the cluster.  
- If the services that need to be exposed are primarily HTTP/HTTPS-based services, it is best to use an Ingress API and Ingress controller to route traffic to backing services with TLS termination. Depending on the type of Ingress controller used, features such as rate limiting, header rewrites, OAuth authentication, observability, and other services can be made available without having to build them into the applications themselves.  
- Choose an Ingress controller that has the needed functionality for secure ingress of your web-based workloads. Standardize on one and use it across the enterprise because many of the specific configuration annotations vary between implementations and prevent the deployment code from being portable across enterprise Kubernetes implementations.  
- Evaluate cloud service provider-specific Ingress controller options to move the infrastructure management and load of the ingress out of the cluster, but still allow for Kubernetes API configuration.  
- When serving mostly APIs externally, evaluate API-specific Ingress controllers, such as Kong or Ambassador, that have more fine-tuning for API-based workloads. Although NGINX, Traefik, and others might offer some API tuning, it will not be as fine-grained as specific API proxy systems.  
- When deploying Ingress controllers as pod-based workloads in Kubernetes, ensure that the deployments are designed for high availability and aggregate performance throughput. Use metrics observability to properly scale the ingress, but include enough cushion to prevent client disruptions while the workload scales.  

### Network Policy Best Practices

- Start off slow and focus on traffic ingress to pods. Complicating matters with ingress and egress rules can make network tracing a nightmare. As soon as traffic is flowing as expected, you can begin to look at egress rules to further control flow to sensitive workloads. The specification also favors ingress because it defaults many options even if nothing is entered into the ingress rules list.  
- Ensure that the network plug-in used either has some of its own interface to the NetworkPolicy API or supports other well-known plug-ins. Example plug-ins include Calico, Cilium, Kube-router, Romana, and Weave Net.  
- If the network team is used to having a “default-deny” policy in place, create a network policy such as the following for each namespace in the cluster that will contain workloads to be protected. This ensures that even if another network policy is deleted, no pods are accidentally “exposed”.  
- If there are pods that need to be accessed from the internet, use a label to explicitly apply a network policy that allows ingress. Be aware of the entire flow in case the actual IP that a packet is coming from is not the internet, but the internal IP of a load balancer, firewall, or other network device. For example, to allow traffic from all (including external) sources for pods having the allow-internet=true label, do this:
- Try to align application workloads to single namespaces for ease of creating rules because the rules themselves are namespace specific. If cross-namespace communication is needed, try to be as explicit as possible and perhaps use specific labels to identify the flow pattern:  
- Have a test bed namespace that has fewer restrictive policies, if any at all, to allow time to investigate the correct traffic patterns needed.  

### Service Mesh Best Practices

- Rate the importance of the key features service meshes offer and determine which current offerings provide the most important features with the least amount of overhead. Overhead here is both human technical debt and infrastructure resource debt. If all that is really required is mutual TLS between certain pods, would it be easier to perhaps find a CNI that offers that integrated into the plug-in?  
- Is the need for a cross-system mesh such as multicloud or hybrid scenarios a key requirement? Not all service meshes offer this capability, and if they do, it is a complicated process that often introduces fragility into the environment.  
- Many of the service mesh offerings are open source community-based projects, and if the team that will be managing the environment is new to service meshes, commercially supported offerings might be a better option. There are companies that are beginning to offer commercially supported and managed service meshes based on Istio, which can be helpful because it is almost universally agreed upon that Istio is a complicated system to manage.  

### PodSecurityPolicy Best Practices

PodSecurityPolicy is complex and can be error prone.  

{{% alert warning %}}
- Proceed with caution when enabling PodSecurityPolicy because it’s potentially workload blocking if adequate preparation isn’t done at the outset.  
- If you are enabling PodSecurityPolicy on an existing cluster with running workloads, you must create all necessary policies, service accounts, roles, and role bindings before enabling the admission controller.  
- It’s extremely important to remember that having no PodSecurityPolicies defined will result in an implicit deny. This means that without a policy match for the workload, the pod will not be created.  
- You should not enable PodSecurityPolicy on a live cluster without considering the warnings provided in the previous section. Proceed with caution.  
{{% /alert %}}

- It all comes down to RBAC. Whether you like it or not, PodSecurityPolicy is determined by RBAC. It’s this relationship that actually exposes all of the shortcomings in your current RBAC policy design. We cannot stress just how important it is to automate your RBAC and PodSecurityPolicy creation and maintenance. Specifically locking down access to service accounts is the key to using policy.  
- Understand the policy scope. Determining how your policies will be laid out on your cluster is very important. Your policies can be cluster-wide, namespaced, or workload-specific in scope. There will always be workloads on your cluster that are part of the Kubernetes cluster operations that will need more permissive security privileges, so make sure that you have appropriate RBAC in place to stop unwanted workloads using your permissive policies.  
- Do you want to enable PodSecurityPolicy on an existing cluster? Use this handy open source tool to generate policies based on your current resources. This is a great start. From there, you can hone your policies.  

### Policy and Governance for Your Cluster

Learn about **Gatekeeper**  
Gatekeeper is an open source customizable Kubernetes admission webhook for cluster policy and governance. Gatekeeper takes advantage of the OPA constraint framework to enforce custom resource definition (CRD)-based policies. Using CRDs allows for an integrated Kubernetes experience that decouples policy authoring from implementation. Policy templates are referred to as constraint templates, which can be shared and reused across clusters. Gatekeeper enables resource validation and audit functionality. One of the great things about Gatekeeper is that it’s portable, which means that you can implement it on any Kubernetes clusters, and if you are already using OPA, you might be able to port that policy over to Gatekeeper.  

### Managing Multiple Clusters Best Practices

- Limit the blast radius of your clusters to ensure cascading failures don’t have a bigger impact on your applications.  
- If you have regulatory concerns such as PCI, HIPPA, or HiTrust, think about utilizing multiclusters to ease the complexity of mixing these workloads with general workloads.  
- If hard multitenancy is a business requirement, workloads should be deployed to a dedicated cluster.  
- If multiple regions are needed for your applications, utilize a Global Load Balancer to manage traffic between clusters.  
- You can break out specialized workloads such as HPC into their own individual clusters to ensure that the specialized needs for the workloads are met.  
- If you’re deploying workloads that will be spread across multiple regional datacenters, first ensure that there is a data replication strategy for the workload. Multiple clusters across regions can be easy, but replicating data across regions can be complicated, so ensure that there is a sound strategy to handle asynchronous and synchronous workloads.  
- Utilize Kubernetes operators like the prometheus-operator or Elasticsearch operator to handle automated operational tasks.  
- When designing your multicluster strategy, also consider how you will do service discovery and networking between clusters. Service mesh tools like HashiCorp’s Consul or Istio can help with networking across clusters.  
- Be sure that your CD strategy can handle multiple rollouts between regions or multiple clusters.  
- Investigate utilizing a GitOps approach to managing multiple cluster operational components to ensure consistency between all clusters in your fleet. The GitOps approach doesn’t always work for everyone’s environment, but you should at least investigate it to ease the operational burden of multicluster environments.  

#### Why Multiple Clusters?

- Blast radius  
- Compliance  
- Security  
- Hard multitenancy  
- Regional-based workloads  
- Specialized workloads  

#### Multicluster Design Concerns

- Data replication  
- Service discovery  
- Network routing  
- Operational management  
- Continuous deployment  

#### Managing Multiple Cluster Deployments

- Operator  
- GitOps  

#### Multicluster Management Tools

- Rancher  
centrally manages multiple Kubernetes clusters in a centrally managed user interface (UI).   
- KQueen  
multitenant self-service portal for Kubernetes cluster provisioning and focuses on auditing, visibility, and security of multiple Kubernetes clusters.   
- Gardener  
provide Kubernetes as a Service to your end users  

#### Kubernetes Federation
KubeFed is not necessarily about multicluster management, but providing high availability (HA) deployments across multiple clusters. It allows you to combine multiple clusters into a single management endpoint for delivering applications on Kubernetes. For example, if you have a cluster that resides in multiple public cloud environments, you can combine these clusters into a single control plane to manage deployments to all clusters to increase the resiliency of your application.  

### Connecting Cluster and External Services Best Practices

- Establish network connectivity between the cluster and on-premises. Networking can be varied between different sites, clouds, and cluster configurations, but first ensure that pods can talk to on-premises machines and vice versa.  
- To access services outside of the cluster, you can use selector-less services and directly program in the IP address of the machine (e.g., the database) with which you want to communicate. If you don’t have fixed IP addressess, you can instead use CNAME services to redirect to a DNS name. If you have neither a DNS name nor fixed services, you might need to write a dynamic operator that periodically synchronizes the external service IP addresses with the Kubernetes Service endpoints.  
- To export services from Kubernetes, use internal load balancers or NodePort services. Internal load balancers are typically easier to use in public cloud environments where they can be bound to the Kubernetes Service itself. When such load balancers are unavailable, NodePort services can expose the service on all of the machines in the cluster.  
- You can achieve connections between Kubernetes clusters through a combination of these two approaches, exposing a service externally that is then consumed as a selector-less service in the other Kubernetes cluster.  

### Machine Leaning on Kubernetes Best Practices

- Smart scheduling and autoscaling. Given that most stages of the machine learning workflow are batch by nature, we recommend that you utilize a Cluster Autoscaler. GPU-enabled hardware is costly, and you certainly do not want to be paying for it when it’s not in use. We recommend batching jobs to run at specific times using either taints and tolerations or via a time-specific Cluster Autoscaler. That way, the cluster can scale to the needs of the machine learning workloads when needed, and not a moment sooner. Regarding taints and tolerations, upstream convention is to taint the node with the extended resource as the key. For example, a node with NVIDIA GPUs should be tainted as follows: Key: nvidia.com/gpu, Effect: NoSchedule. Using this method means that you can also utilize the ExtendedResourceToleration admission controller, which will automatically add the appropriate tolerations for such taints to pods requesting extended resources so that the users don’t need to manually add them.  
- The truth is that model training is a delicate balance. Allowing things to move faster in one area often leads to bottlenecks in others. It’s an endeavor of constant observation and tuning. As a general rule of thumb, we recommend that you try to make the GPU become the bottleneck because it is the most costly resource. Keep your GPUs saturated. Be prepared to always be on the lookout for bottlenecks, and set up your monitoring to track the GPU, CPU, network, and storage utilization.  
- Mixed workload clusters. Clusters that are used to run the day-to-day business services might also be used for the purposes of machine learning. Given the high performance requirements of machine learning workloads, we recommend using a separate node pool that’s tainted to accept only machine learning workloads. This will help protect the rest of the cluster from any impact from the machine learning workloads running on the machine learning node pool. Furthermore, you should consider multiple GPU-enabled node pools, each with different performance characteristics to suit the workload types. We also recommend enabling node autoscaling on the machine learning node pool(s). Use mixed mode clusters only after you have a solid understanding of the performance impact that your machine learning workloads have on your cluster.  
- Achieving linear scaling with distributed training. This is the holy grail of distributed model training. Most libraries unfortunately don’t scale in a linear fashion when distributed. There is lots of work being done to make scaling better, but it’s important to understand the costs because this isn’t as simple as throwing more hardware at the problem. In our experience, it’s almost always the model itself and not the infrastructure supporting it that is the source of the bottleneck. It is, however, important to review the utilization of the GPU, CPU, network, and storage before pointing fingers at the model itself. Open source tools such as Horovod seek to improve distributed training frameworks and provide better model scaling.  

### Building Application Platforms Best Practices

- Use admission controllers to limit and modify API calls to the cluster. An admission controller can validate (and reject invalid) Kubernetes resources. A mutating admission controller can automatically modify API resources to add new sidecars or other changes that users might not even need to know about.  
- Use kubectl plug-ins to extend the Kubernetes user experience by adding new tools to the familiar existing command-line tool. In rare occasions, a purpose-built tool might be more appropriate.  
- When building platforms on top of Kubernetes, think carefully about the users of the platform and how their needs will evolve. Making things simple and easy to use is clearly a good goal, but if this also leads to users that are trapped and unable to be successful without rewriting everything outside of your platform, it will ultimately be a frustrating (and unsuccessful) experience.  

### Managing State and Stateful Applications

#### Volume Best Practices

- Try to limit the use of volumes to pods requiring multiple containers that need to share data, for example adapter or ambassador type patterns. Use the emptyDir for those types of sharing patterns.  
- Use hostDir when access to the data is required by node-based agents or services.  
- Try to identify any services that write their critical application logs and events to local disk, and if possible change those to stdout or stderr and let a true Kubernetes-aware log aggregation system stream the logs instead of leveraging the volume map.  

#### Kubernetes Storage Best Practices
Cloud native application design principles try to enforce stateless application design as much as possible; however, the growing footprint of container-based services has created the need for data storage persistence. These best practices around storage in Kubernetes in general will help to design an effective approach to providing the required storage implementations to the application design:  

- If possible, enable the DefaultStorageClass admission plug-in and define a default storage class. Many times, Helm charts for applications that require PersistentVolumes default to a default storage class for the chart, which allows the application to be installed without too much modification.  
- When designing the architecture of the cluster, either on-premises or in a cloud provider, take into consideration zone and connectivity between the compute and data layers using the proper labels for both nodes and PersistentVolumes, and using affinity to keep the data and workload as close as possible. The last thing you want is a pod on a node in zone A trying to mount a volume that is attached to a node in zone B.  
- Consider very carefully which workloads require state to be maintained on disk. Can that be handled by an outside service like a database system or, if running in a cloud provider, by a hosted service that is API consistent with currently used APIs, say a mongoDB or mySQL as a service?  
- Determine how much effort would be involved in modifying the application code to be more stateless.  
- While Kubernetes will track and mount the volumes as workloads are scheduled, it does not yet handle redundancy and backup of the data that is stored in those volumes. The CSI specification has added an API for vendors to plug in native snapshot technologies if the storage backend can support it.  
- Verify the proper life cycle of the data that volumes will hold. By default the reclaim policy is set to for dynamically provisioned persistentVolumes which will delete the volume from the backing storage provider when the pod is deleted. Sensitive data or data that can be used for forensic analysis should be set to reclaim.  

#### StatefulSet and Operator Best Practices

- The decision to use Statefulsets should be taken judiciously because usually stateful applications require much deeper management that the orchestrator cannot really manage well yet (read the “Operators” section for the possible future answer to this deficiency in Kubernetes).  
- The headless Service for the StatefulSet is not automatically created and must be created at deployment time to properly address the pods as individual nodes.  
- When an application requires ordinal naming and dependable scaling, it does not always mean it requires the assignment of PersistentVolumes.  
- If a node in the cluster becomes unresponsive, any pods that are part of a StatefulSet are not not automatically deleted; they instead will enter a Terminating or Unkown state after a grace period. The only way to clear this pod is to remove the node object from the cluster, the kubelet beginning to work again and deleting the pod directly, or an Operator force deleting the pod. The force delete should be the last option and great care should be taken that the node that had the deleted pod does not come back online, because there will now be two pods with the same name in the cluster. You can use kubectl delete pod nginx-0 --grace-period=0 --force to force delete the pod.  
- Even after force deleting a pod, it might stay in an Unknown state, so a patch to the API server will delete the entry and cause the StatefulSet controller to create a new instance of the deleted pod: kubectl patch pod nginx-0 -p '{'metadata':{'finalizers':null}}'.  
- If you’re running a complex data system with some type of leader election or data replication confirmation processes, use preStop hook to properly close any connections, force leader election, or verify data synchronization before the pod is deleted using a graceful shutdown process.  
- When the application that requires stateful data is a complex data management system, it might be worth a look to determine whether an Operator exists to help manage the more complicated life cycle components of the application. If the application is built in-house, it might be worth investigating whether it would be useful to package the application as an Operator to add additional manageability to the application. Look at the CoreOS Operator SDK for an example.  

#### ReplicaSet vs. StatefulSets

ReplicaSet:  
- Pods in a ReplicaSet are scaled out and assigned random names when scheduled.  
- Pods in a ReplicaSet are scaled down in an arbitrary manner.  
- Pods in a ReplicaSet are never called directly through their name or IP address but through their association with a Service.  
- Pods in a ReplicaSet can be restarted and moved to another node at any time.  
- Pods in a ReplicaSet that have a PersistentVolume mapped are linked only by the claim, but any new pod with a new name can take over the claim if needed when - rescheduled.  

StatefulSets:  
- Pods in a StatefulSet are scaled out and assigned sequential names. As the set scales up, the pods get ordinal names, and by default a new pod must be fully - online (pass its liveness and/or readiness probes) before the next pod is added.  - 
- Pods in a StatefulSet are scaled down in reverse sequence.  - 
- Pods in a StatefulSet can be addressed individually by name behind a headless Service.  - 
- Pods in a StatefulSet that require a volume mount must use a defined PersistentVolume template. Volumes claimed by pods in a StatefulSet are not deleted when the StatefulSet is deleted.  

### Admission Control Best Practices

- Admission plug-in ordering doesn’t matter.   
- Don’t mutate the same fields. Configuring multiple mutating admission webhooks also presents challenges.   
- Fail open/fail closed. You might recall seeing the failurePolicy field as part of both the mutating and validating webhook configuration resources. This field defines how the API server should proceed in the case where the admission webhooks have access issues or encounter unrecognized errors. You can set this field to either Ignore or Fail. Ignore essentially fails to open, meaning that processing of the request will continue, whereas Fail denies the entire request. This might seem obvious, but the implications in both cases require consideration. Ignoring a critical admission webhook could result in policy that the business relies on not being applied to a resource without the user knowing.  
One potential solution to protect against this would be to raise an alert when the API server logs that it cannot reach a given admission webhook. Fail can be even more devastating by denying all requests if the admission webhook is experiencing issues. To protect against this you can scope the rules to ensure that only specific resource requests are set to the admission webhook. As a tenet, you should never have any rules that apply to all resources in the cluster.  
- If you have written your own admission webhook, it’s important to remember that user/system requests can be directly affected by the time it takes for your admission webhook to make a decision and respond. All admission webhook calls are configured with a 30-second timeout, after which time the failurePolicy takes effect. Even if it takes several seconds for your admission webhook to make an admit/deny decision, it can severely affect user experience when working with the cluster. Avoid having complex logic or relying on external systems such as databases in order to process the admit/deny logic.  
- Scoping admission webhooks. There is an optional field that allows you to scope the namespaces in which the admission webhooks operate on via the NamespaceSelector field. This field defaults to empty, which matches everything, but can be used to match namespace labels via the use of the matchLabels field. We recommend that you always use this field because it allows for an explicit opt-in per namespace.  
- The kube-system namespace is a reserved namespace that’s common across all Kubernetes clusters. It’s where all system-level services operate. We recommend never running admission webhooks against the resources in this namespace specifically, and you can achieve this by using the NamespaceSelector field and simply not matching the kube-system namespace. You should also consider it on any system-level namespaces that are required for cluster operation.  
- Lock down admission webhook configurations with RBAC. Now that you know about all the fields in the admission webhook configuration, you have probably thought of a really simple way to break access to a cluster. It goes without saying that the creation of both a MutatingWebhookConfiguration and ValidatingWebhookConfiguration is a root-level operation on the cluster and must be locked down appropriately using RBAC. Failure to do so can result in a broken cluster or, even worse, an injection attack on your application workloads.  
- Don’t send sensitive data. Admission webhooks are essentially black boxes that accept AdmissionRequests and output AdmissionResponses. How they store and manipulate the request is opaque to the user. It’s important to think about what request payloads you are sending to the admission webhook. In the case of Kubernetes secrets or ConfigMaps, they might contain sensitive information and require strong guarantees about how that information is stored and shared. Sharing these resources with an admission webhook can leak sensitive information, which is why you should scope your resource rules to the minimum resource needed to validate and/or mutate.  


### Authorization
In Kubernetes, the authorization of each request is performed after authentication but before admission.  
Unlike admission controllers, if a single authorization module admits the request, the request can proceed. Only for the case in which all modules deny the request will an error be returned to the user.  

#### Authorization Modules  
- Attribute-Based Access Control (ABAC)  
Allows authorization policy to be configured via local files  
- RBAC  
Allows authorization policy to be configured via the Kubernetes API  
- Webhook  
Allows the authorization of a request to be handled via a remote REST endpoint  
- Node  
Specialized authorization module that authorizes requests from kubelets  

#### Authorization Best Practices

- Given that the ABAC policies need to be placed on the filesystem of each master node and kept synchronized, we generally recommend against using ABAC in multimaster clusters. The same can be said for the webhook module because the configuration is based on a file and a corresponding flag being present. Furthermore, changes to these policies in the files require a restart of the API server to take effect, which is effectively a control-plane outage in a single master cluster or inconsistent configuration in a multimaster cluster. Given these details, we recommend using only the RBAC module for user authorization because the rules are configured and stored in Kubernetes itself.  
- Webhook modules, although powerful, are potentially very dangerous. Given that every request is subject to the authorization process, a failure of a webhook service would be devastating for a cluster. Therefore, we generally recommend not using external authorization modules unless you completely vet and are comfortable with your cluster failure modes if the webhook service becomes unreachable or unavailable.  

{{% alert note %}}
Please refer to [the book](https://www.amazon.com/Kubernetes-Best-Practices-Blueprints-Applications/dp/1492056472/ref=sr_1_3?crid=22K492WLGAPTP&dchild=1&keywords=kubernetes+best+practices&qid=1602558033&sprefix=Kubernetes+Best+Practices%2Caps%2C176&sr=8-3) for details.
{{% /alert %}}

## Links

[Kubernetes Best Practices: Blueprints for Building Successful Applications on Kubernetes](https://www.amazon.com/Kubernetes-Best-Practices-Blueprints-Applications/dp/1492056472/ref=sr_1_3?crid=22K492WLGAPTP&dchild=1&keywords=kubernetes+best+practices&qid=1602558033&sprefix=Kubernetes+Best+Practices%2Caps%2C176&sr=8-3)  

<br>

#### Did you find this page helpful? Consider sharing it 🙌

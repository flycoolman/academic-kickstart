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





{{% alert note %}}
- including both application code and configuration code, such as a Kubernetes manifest or Helm charts, helps promote good DevOps principles of communication and collaboration. Having both application developers and operation engineers collaborate in a single repository builds confidence in a team to deliver an application to production.  
{{% /alert %}}


![cri.png](./cri.png)


{{% alert note %}}
TBD
{{% /alert %}}



## Links

[Kubernetes Best Practices: Blueprints for Building Successful Applications on Kubernetes](https://www.amazon.com/Kubernetes-Best-Practices-Blueprints-Applications/dp/1492056472/ref=sr_1_3?crid=22K492WLGAPTP&dchild=1&keywords=kubernetes+best+practices&qid=1602558033&sprefix=Kubernetes+Best+Practices%2Caps%2C176&sr=8-3)  


<br>

#### Did you find this page helpful? Consider sharing it 🙌

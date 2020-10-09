---
title: 'Minikube'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary:  A simple guide introduces how to use Minikube.
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
date: "2020-07-01T00:00:00Z"
lastmod: "2020-07-01T00:00:00Z"
featured: false
draft: trues

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


## Minikube

**Minikube** is a tool that makes it easy to run Kubernetes locally. Minikube runs a single-node Kubernetes cluster inside a Virtual Machine (VM) on your laptop for users looking to try out Kubernetes or develop with it day-to-day.

### Set up Minikube

[minikube start](https://minikube.sigs.k8s.io/docs/start/)  

### Minikube VM Login
- On VM console:  
username: root
no password

- from host terminal:  
    minikube ssh  


username+IP on host:  
username: docker  
password: tcuser  
i.e.  

    ssh docker@192.168.99.103


Exit the login:
exit

### Setting a VM driver by default

minikube config set vm-driver virtualbox

### Use local images by re-using the Docker daemon
[How To Install and Use Docker on Ubuntu 18.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04)  
[Use local images by re-using the Docker daemon](https://kubernetes.io/docs/setup/learning-environment/minikube/#use-local-images-by-re-using-the-docker-daemon)  

eval $(minikube docker-env)  

### Commands for using minikube

minikube start  
minikube delete  
minikube status  

minikube start -p test  
minikube delete -p test  
<br>
minikube ssh  
minikube ssh -p test  
minikube ip  
minikube dashboard  
minikube addons list  
<br>
kubectl get pods -A  

[Minikube Cheat Sheet: most helpful commands and features I wish I knew from the start](https://blog.pilosus.org/posts/2019/05/18/minikube-cheat-sheet/)  


### Minikube command auto completion

sudo apt install bash-completion  
[How to add bash auto completion in Ubuntu Linux](https://www.cyberciti.biz/faq/add-bash-auto-completion-in-ubuntu-linux/)  
[How can I enable Tab completion to minikube?](https://stackoverflow.com/questions/57891054/how-can-i-enable-tab-completion-to-minikube  )  
 


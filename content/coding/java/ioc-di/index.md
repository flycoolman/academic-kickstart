---
title: 'Inversion of Control and Dependency Injection'
# subtitle: 'Create a beautifully simple website in under 10 minutes :rocket:'
summary: A summary of Spring Inversion of Control and Dependency Injection.
authors:
- admin
tags:
- Java
- Spring
- IoC
- DI
categories:
- Java
date: "2020-07-30T00:00:00Z"
lastmod: "2020-08-29T00:00:00Z"
featured: false
draft: true

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

### Definition

#### Inversion of Control (IoC)


Central to the Spring Framework is its inversion of control (IoC) container, which provides a consistent means of configuring and managing Java objects using reflection. The container is responsible for managing object lifecycles of specific objects: creating these objects, calling their initialization methods, and configuring these objects by wiring them together.

{{% alert note %}}
A process in which an object defines its dependencies without creating them.
{{% /alert %}}


The Spring IoC Container is the leading dependency injection framework. 

This chapter covers the Spring Framework implementation of the Inversion of Control (IoC) principle. IoC is also known as dependency injection (DI). It is a process whereby objects define their dependencies (that is, the other objects they work with) only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned from a factory method. The container then injects those dependencies when it creates the bean. This process is fundamentally the inverse (hence the name, Inversion of Control) of the bean itself controlling the instantiation or location of its dependencies by using direct construction of classes or a mechanism such as the Service Locator pattern.




<br>

{{% alert note %}}
- Representation - org.springframework.context.ApplicationContext
- Responsibilities - instantiating, configuring, and assembling Beans
- Tool: configuration metadata

{{% /alert %}}

<br>

The interface org.springframework.context.ApplicationContext represents the Spring IoC container and is responsible for instantiating, configuring, and assembling the aforementioned beans. The container gets its instructions on what objects to instantiate, configure, and assemble by reading configuration metadata. 


The main control of the program was inverted, moved away from you to the framework.


As a result I think we need a more specific name for this pattern. Inversion of Control is too generic a term, and thus people find it confusing. As a result with a lot of discussion with various IoC advocates we settled on the name Dependency Injection.





#### Dependency Injection (DI)

Objects can be obtained by means of either dependency lookup or dependency injection. Dependency lookup is a pattern where a caller asks the container object for an object with a specific name or of a specific type. Dependency injection is a pattern where the container passes objects by name to other objects, via either constructors, properties, or factory methods.

IOC basically facilitates having different components designed and coded separately and later used together by defining their relation with DI.

But in Spring, since it is a framework or Container who does that job of injecting dependancies (DI) and not us, The flow of control is reversed, (Framework to Application) it is DI with IOC.

Objects can be obtained by means of either dependency lookup or dependency injection. Dependency lookup is a pattern where a caller asks the container object for an object with a specific name or of a specific type. Dependency injection is a pattern where the container passes objects by name to other objects, via either constructors, properties, or factory methods.




I'm going to start by talking about the various forms of dependency injection, but I'll point out now that that's not the only way of removing the dependency from the application class to the plugin implementation. The other pattern you can use to do this is Service Locator, and I'll discuss that after I'm done with explaining Dependency Injection.


The Styles of DI

If you use Dependency Injection there are a number of styles to choose between. I would suggest you follow constructor injection unless you run into one of the specific problems with that approach, in which case switch to setter injection.


### Step 2: Install GUI on the VM

Ssh to the VM,  

    vagrant ssh

then use the below commands to intall GUI on the VM.

    sudo yum -y groupinstall "GNOME Desktop"
    echo "exec gnome-session" >> ~/.xinitrc
    systemctl set-default graphical.target

Use command

    startx

in VirtualBox console to start the GUI.

### Step 3: Install Docker on the VM

Install docker and start the service.

    sudo yum -y install docker
    sudo systemctl status docker
    sudo systemctl start docker
    sudo systemctl enable docker

Create user group 'docker' and add you into the group.

    sudo groupadd docker
    sudo usermod -aG docker $(whoami)
    sudo usermod -aG docker vagrant

Reevaluate the group and restart the docker service.

    logout
    sudo systemctl restart docker

Check if you can run docker commands without sudo.

    docker info

### Step 4: Install SVN

Add the repository and install SVN.

    sudo vim /etc/yum.repos.d/wandisco-svn.repo

>[WandiscoSVN]
>name=Wandisco SVN Repo
>baseurl=http://opensource.wandisco.com/centos/$releasever/svn-1.8/RPMS/$basearch/
>enabled=1
>gpgcheck=0

    sudo yum remove subversion*
    sudo yum clean all
    sudo yum install subversion

    svn --version

### Step 5: Check out the branch

UTF-8 settings:

    export LC_ALL=C
    sudo vi .bashrc

Add below to the file \~/.bashrc

>LANG=en_US.UTF-8
>export LANG

Change SELinux settings, so that docker image can access your home directory.

    chcon -Rt svirt_sandbox_file_t /home/vagrant/

Checkout the branch

    mkdir -p ietf
    cd ietf
    svn co https://svn.tools.ietf.org/svn/tools/ietfdb/personal/flycoolman/7.10.1.dev0

### Step 6: Set up database

    cd 7.10.1.dev0/
    ./docker/setupdb

### Step 7: Set up virtual environment

    ./docker/run

{{% alert note %}}
rsyslog error can be ignored!  
[FAIL] rsyslogd is not running ... failed!
{{% /alert %}}



**In virtual environment of the container**

    pip install --upgrade -r requirements.txt

    ./ietf/manage.py migrate

{{% alert note %}}
The below operation might be needed if the migration fails.  

    sudo cp docker/settings_local.py ietf/  

Then run the migrate command again.
{{% /alert %}}

### Step 8: Run the tests
In the virtual environment to run the tests:

./ietf/manage.py test --settings=settings_sqlitetest

{{% alert note %}}
Make sure that one of the following commands runs to completion without errors.
{{% /alert %}}

### Step 9: Start the Development Server

    ./ietf/manage.py runserver 0.0.0.0:8000 &

Test the access to datatracker.

![ietf-datatracker](./featured.png)


### Step 10: Mailserver and Rsync Data

Go to [the original page](https://trac.tools.ietf.org/tools/ietfdb/wiki/SprintCoderSetup) for details about:  
- [(Optional) Run the mailserver](https://trac.tools.ietf.org/tools/ietfdb/wiki/SprintCoderSetup)  
- [Manually Edit or rsync Datatracker Data Directories](https://trac.tools.ietf.org/tools/ietfdb/wiki/SprintCoderSetup)

### Setup Complete

For other workflow things, please refer to [the original setup guide.](https://trac.tools.ietf.org/tools/ietfdb/wiki/SprintCoderSetup)

### Links
[The IoC Container](https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-introduction)  
[Inversion of Control Containers and the Dependency Injection pattern](https://martinfowler.com/articles/injection.html)  
[Spring Framework](https://en.wikipedia.org/wiki/Spring_Framework#Inversion_of_control_container_.28dependency_injection.29)  
[What is Dependency Injection and Inversion of Control in Spring Framework?](https://stackoverflow.com/questions/9403155/what-is-dependency-injection-and-inversion-of-control-in-spring-framework)  
[Spring – Inversion of Control vs Dependency Injection](https://howtodoinjava.com/spring-core/spring-ioc-vs-di/)  

<br>

#### Did you find this page helpful? Consider sharing it 🙌

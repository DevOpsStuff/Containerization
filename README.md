# Docker Container Stuff

### What is Container?
   Containers offer a logical packaging mechanism in which applications can be abstracted from the environment in which they actually run. This decoupling allows container-based applications to be deployed easily and consistently, regardless of whether the target environment is a private data center, the public cloud, or even a developer’s personal laptop. Containerization provides a clean separation of concerns, as developers focus on their application logic and dependencies, while IT operations teams can focus on deployment and management without bothering with application details such as specific software versions and configurations specific to the app.

For those coming from virtualized environments, containers are often compared with virtual machines (VMs). You might already be familiar with VMs: a guest operating system such as Linux or Windows runs on top of a host operating system with virtualized access to the underlying hardware. Like virtual machines, containers allow you to package your application together with libraries and other dependencies, providing isolated environments for running your software services. As you’ll see below however, the similarities end here as containers offer a far more lightweight unit for developers and IT Ops teams to work with, carrying a myriad of benefits.

### Comparing Containers and Virtual Machines

![ContainerVsVirtualMachine](ContainerVsVM.png)

### Why Container?

   Instead of virtualizing the hardware stack as with the virtual machines approach, containers virtualize at the operating system level, with multiple containers running atop the OS kernel directly. This means that containers are far more lightweight: they share the OS kernel, start much faster, and use a fraction of the memory compared to booting an entire OS.
   
### What is Docker?

   Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

### The Docker platform

   Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security allow you to run many containers simultaneously on a given host. Containers are lightweight because they don’t need the extra load of a hypervisor, but run directly within the host machine’s kernel. This means you can run more containers on a given hardware combination than if you were using virtual machines. You can even run Docker containers within host machines that are actually virtual machines!
   
   Docker provides tooling and a platform to manage the lifecycle of your containers:

   * Develop your application and its supporting components using containers.
   * The container becomes the unit for distributing and testing your application.
   * When you’re ready, deploy your application into your production environment, as a container or an orchestrated service. This works the same whether your production environment is a local data center, a cloud provider, or a hybrid of the two.
   
# Docker Components:
 
 **Docker Engine**
    
  Docker Engine is a client-server application with these major components:
  
   * A server which is a type of long-running program called a daemon process (the dockerd command).
   * A REST API which specifies interfaces that programs can use to talk to the daemon and instruct it what to do.
   * A command line interface (CLI) client (the docker command).
   
   ![DockerEngine](https://docs.docker.com/engine/images/engine-components-flow.png)
   
  The CLI uses the Docker REST API to control or interact with the Docker daemon through scripting or direct CLI commands. Many other Docker applications use the underlying API and CLI.

  The daemon creates and manages Docker objects, such as images, containers, networks, and volumes.
  
  **What can I use Docker for?**
  
  * Fast, consistent delivery of your applications
  * Responsive deployment and scaling
  * Running more workloads on the same hardware
  
  **Docker architecture**
  
  Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers. The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon. The Docker client and daemon communicate using a REST API, over UNIX sockets or a network interface.
  
  ![DockerArchitecture](https://github.com/DevOpsStuff/Containerization/blob/master/Docker%20Architecture.png)
  
  the below diagram shows how docker works,
  
  ![DockerArch](https://docs.docker.com/engine/images/architecture.svg)
  
  **The Docker daemon**
  
   The Docker daemon (dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. A daemon can also communicate with other daemons to manage Docker services.
   
  **The Docker client**
  
  The Docker client (docker) is the primary way that many Docker users interact with Docker. When you use commands such as docker run, the client sends these commands to dockerd, which carries them out. The docker command uses the Docker API. The Docker client can communicate with more than one daemon.
  
  **Docker registries**
  
  A Docker registry stores Docker images. Docker Hub and Docker Cloud are public registries that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry. If you use Docker Datacenter (DDC), it includes Docker Trusted Registry (DTR).

  When you use the docker pull or docker run commands, the required images are pulled from your configured registry. When you use the docker push command, your image is pushed to your configured registry.
  
# Docker Objects
 
  When you use Docker, you are creating and using images, containers, networks, volumes, plugins, and other objects. This section is a brief overview of some of those objects.
  
 **IMAGES**
 
   An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization. For example, you may build an image which is based on the ubuntu image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

  You might create your own images or you might only use those created by others and published in a registry. To build your own image, you create a Dockerfile with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image. When you change the Dockerfile and rebuild the image, only those layers which have changed are rebuilt. This is part of what makes images so lightweight, small, and fast, when compared to other virtualization technologies.

 **CONTAINERS**
 
  A container is a runnable instance of an image. You can create, start, stop, move, or delete a container using the Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

  By default, a container is relatively well isolated from other containers and its host machine. You can control how isolated a container’s network, storage, or other underlying subsystems are from other containers or from the host machine.

  A container is defined by its image as well as any configuration options you provide to it when you create or start it. When a container is removed, any changes to its state that are not stored in persistent storage disappear.
  
  * Example `docker run` command
  
   The following command runs an ubuntu container, attaches interactively to your local command-line session, and runs /bin/bash.
   
   ```
   $ docker run -i -t ubuntu /bin/bash
   ```
   
   When you run this command, the following happens (assuming you are using the default registry configuration):

   * If you do not have the `ubuntu` image locally, Docker pulls it from your configured registry, as though you had run `docker pull ubuntu` manually.
   * Docker creates a new container, as though you had run a `docker container create` command manually.
   * Docker allocates a read-write filesystem to the container, as its final layer. This allows a running container to create or modify files and directories in its local filesystem.
   * Docker creates a network interface to connect the container to the default network, since you did not specify any networking options. This includes assigning an IP address to the container. By default, containers can connect to external networks using the host machine’s network connection.
   * Docker starts the container and executes /bin/bash. Because the container is run interactively and attached to your terminal (due to the -i and -t flags), you can provide input using your keyboard and output is logged to your terminal.
   * When you type exit to terminate the /bin/bash command, the container stops but is not removed. You can start it again or remove it.
   
# The underlying technology
  
   Docker is written in Go and takes advantage of several features of the Linux kernel to deliver its functionality.
   
   **Namespaces**
   
   Docker uses a technology called namespaces to provide the isolated workspace called the container. When you run a container, Docker creates a set of namespaces for that container.

   These namespaces provide a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.
   
   Docker Engine uses namespaces such as the following on Linux:

   * The pid namespace: Process isolation (PID: Process ID).
   * The net namespace: Managing network interfaces (NET: Networking).
   * The ipc namespace: Managing access to IPC resources (IPC: InterProcess Communication).
   * The mnt namespace: Managing filesystem mount points (MNT: Mount).
   * The uts namespace: Isolating kernel and version identifiers. (UTS: Unix Timesharing System).
   
   **Control groups**
   
   Docker Engine on Linux also relies on another technology called control groups (cgroups). A cgroup limits an application to a specific set of resources. Control groups allow Docker Engine to share available hardware resources to containers and optionally enforce limits and constraints. For example, you can limit the memory available to a specific container.
   
   **Union file systems**
   
   Union file systems, or UnionFS, are file systems that operate by creating layers, making them very lightweight and fast. Docker Engine uses UnionFS to provide the building blocks for containers. Docker Engine can use multiple UnionFS variants, including AUFS, btrfs, vfs, and DeviceMapper.
   
   **Container format**
   
   Docker Engine combines the namespaces, control groups, and UnionFS into a wrapper called a container format. The default container format is libcontainer. In the future, Docker may support other container formats by integrating with technologies such as BSD Jails or Solaris Zones.
   

# Docker installation

Follow this Document [DockerInstallation](setup.md)

**Docker Commands**

```
->Http API from client to server

Docker needs root access
/var/run/
ls -l /run

from another user
docker run -it ubuntu /bin/bash
cat /etc/group
sudo gpasswd -a prasanna docker
run -> docker run -it ubuntu /bin/bash

#docker info
#docker version

#docker run it centos /bin/bash
uname -a #sharing same kernel
create afile in the container
exit
docker ps
docker ps -a
cd /var/lib/docker/aufs/diff
docker start <containerid>
docker attach <container id>


##Docker components
->Docker engine
-> images
-> containers
-> registries and repositories

Dev -> test -> prod

#Docker Engine
A Docker Program 

#Docker Images
Docker run -it fedora /bin/bash
images are composed of multiple layers
:latest

-> to fetch all fedora images
docker pull -a fedora
docker images fedora
docker pull coreos/etcd

images are localy stored under /var/lib/docker/aufs/

#Docker containers
-> containers are launced from images
docker run -> to launch the container
docker run -it ubuntu /bin/bash

ctrl + p + q

#repo and registry



#Layered images
(layer0 -> layer1 -> layer2) together single images
(baseimage-> nginx -> updates) 
each layer have unique id

union mounts allows multiple layers -> read only -> only the top layer is writeable
bootfs layer -> sometimes
rootfs -> layer1 -> layer2

#cli demo
docker images
docker pull coreos/apache
docker images --tree

ls -l /var/lib/docker/aufs/layers/
cat the file

ls -l /var/lib/docker/aufs/diff/<id>

docker images --tree

#docker copying
docker images
 
docker ps -a
docker commit  <id> apple
docker images
docker images --tree
docker history apple

docker save -o /tmp/apple.tar apple
ls -lh /tmp/apple.tar

tar -tf /tmp/apple.tar

docker load -i /tmp/apple.tar
docker run -it apple /bin/bash
cat /tmp/cool-file

#images are build time contructs and containers are runtime contructs
top layer -> thin writeable layer -> consumes space

#one process per containers
docker run -d ubuntu /bin/bash -c "ping 8.8.8.8 -c 30"
docker ps
docker top
docker ps
docker ps -a
docker run ubuntu:14.0

#docker run
docker run --cpu-shares=256
docker run memory=1g

docker run -d ubuntu:14.01 /bin/bash -c "ping 8.8.8.8"
docker ps
docker inspect <id>
docker attach <name>
exit
docker ps

#Container Management
-> container management
-> container config
-> look inside running containers
-> differentways to get shell access

seperate netns,pidns,mnt
start -> stop -> restart
docker run -it ubuntu:14.04 /bin/bash
ctrl + p + q
docker ps
docker stop <id> or <name>
docker ps
docker stop -> sigterm to container process PID1
docker kill -s <sigkill> -> sigkill to container 
docker ps -l -> last container
docker start <id>
docekr attach <id>
ctrl +p + q
docker ps
docker restart <id>
docekr ps

one process / container

docker ps 
docker info
ls -l /var/lib/docker/containers | wc -l
docker rm <id>
docker stop <id>
docker rm <id>
docker info

docker rm -f 

#inside the container
docker top
docker ps 
docker top <id>
#docker run phusion/basheimage
docker attach <id>
ps -ef
docker logs
docker inspect

docker ps
docker inspect <id>

ls -l /var/lib/docker/containers/<id>/

#Getting inside the shell
docker attach -> attachs to pid1
ssh ->
nsenter-> allows us to enter namespaces
-> requires the containerid(docker inspect)

docker inspect <id> | grep pid
nsenter -m -u -n -p -i -t <pid> /bin/bash
exit
docker ps

docker-enter <id>

docker exec -it <id> /bin/bash

docker run -v /usr/local/bin:/target jpetazzo/nsenter


#DockerFile

'''
FROM ubuntu:15.04
MAINTAINER prassanna.mit@gmail.com
RUN apt-get update
RUN apt-get install -y nginx
RUN apt-get install -y golang

CMD [ "echo 'hello world'"]
'''

ls -l
docker build -t helloworld:0.1 .

docker images --tree
docker history <id>
docker run helloworld:0.1 

#Docker registry
docker images
docker tag <id> prasanna/helloworld:1.0
docker images
docker build -t 
docker push prasanna/helloworld:1.0
docker rmi <id>

#Deeper in to DockerFile
-> the Build cache
-> DockerFile

#Build Cache

docker images
cd DockerBuildFiles
docker build -t="v0.1" .
docker build -t="v0.2" .

#images layers
docker images
docker info

```
FROM ubuntu:15.04
MAINTAINER prassanna.mit@gmail.com
RUN apt-get update
RUN apt-get install -y apache2
RUN apt-get install -y vim
RUN apt-get install -y golang
CMD ["echo", "Hello World"]
```

mkdir web
cd web

```
FROM ubuntu:15.04
RUN apt-get update
RUN apt-get install -y apache2
RUN apt-get install -y apache2-utils
RUN apt-get install -y vim
RUN apt-get clean
EXPOSE 80
CMD ["apache2ctl","-D","FOREGROUND"]
```

docker build -t="webserver"
docker run -d -p 80:80 webserver
docker stop <id>

#Avoid creating unnessary layers

#CMD vs RUN
CMD
-> runtime
-> run comands in containers at launch time
equvalent of docker run <args> /bin/bash
-> one CMD per Dockerfile
-> shellform and execform
CMD echo "hello world"
CMD echo $var1

-> Execform
["command","args1"]


RUN
-> buildtime
-> add layers to images
-> used to install apps


#ENTRYPOINT
ENTRYPOINT ["echo"]
docker build -t="asdad" .
docker run asdad hellp asdasd
docker run -it asdad /bin/bash

ENTRYPOINT ["apache2ctl"]
build it and run

docker run -d -p 80:80 web2 -D FOREGROUND

#ENV
ENV Key=value
ENV var1=prasanna var2=ranganathan

```
FROM ubuntu:15.04
RUN apt-get update && apt-get install -y iputils-ping apache2
ENV var1=ping var2=8.8.8.8
CMD $var1 $var2
```

#Volumes
docker run -it -v /test_vol --name=voltainer ubuntu:15.-4 /bin/bash
create afile under /test_vol in the container
docker inspect


docker run -it -p 5984:5984 -v $(pwd)/data:/usr/local/var/lib/couchdb --name couchdb klaemo/couchdb
curl -X PUT http://192.168.99.100:5984/db
insert a document using curl
curl -X PUT -H "Content-Type: application/json" -d '{"name": "Avengers"}' http://172.28.128.3:5984/db/avengers

docker create -v /usr/local/var/lib/couchdb --name db-data debian:jessie /bin/true
docker ps
docker ps -a
-> first container ->
docker run -d -p 5984:5984 -v /usr/local/var/lib/clouchdb --name db1 --volumes-from db-data klaemo/couchdb
->second container
docker run -d -p 5985:5984 -v /usr/local/var/lib/clouchdb --name db2  --volumes-from db-data klaemo/couchdb



docker run -it --volumes-from=voltainer ubuntu:15.04 /bin/bash

#host mount
not very much scalable
docker run -it -v /data:/data ubuntu:15.04 /bin/bash


Dockerfile
VOLUME /data

docker rm -v <container>
docker stop voltainer
docekr rm -v voltainer


-> unshare tool
unshare -m bash
unshare -n

#memory error
docker run -it --rm -m 100m debian
free -m
cat /sys/fs/cgroup/memory/memory.limit_in_bytes
echo "$((num / 1024 /1024))


#Docker Networking

Libnetwork
before 1.11 - network won't scale
Three pillars of Docker Networking
-> CNM(container network Model)
  -> DNA of Docker networking
-> CNM -> LibNetwork -> Drivers

CNM Vs CNI

CNM -> (sandbox, endpoint, network)
sandbox -> namespace
endpoint->eth0
network -> connected endpoints

Libnetwork
->control plane and management plane

Drivers
-> Data plane

->Overlay
->MacVlan
->Bridge
-> "local"

Native Drivers Vs Remote Drivers

docker network ls
default is bridge
1) docker0 is bridge network
docker network inspect <id>
2) none network
docker run -d -P --net none --name no-network-app ubuntu
docker exec -it no-network-app bin/bash
3) host network
docker run -d -P --net host --name host-network ubuntu
docker exec -it ubuntu /bin/bash
host network is sometimes disadvanges on the same machine

4) User Defined network
is similar to bridge network
docker network create --driver bridge my-network
docker network ls
docker run -d -P --net my-network --name hello ubuntu
docker inspect hello

#Single Host networking
on bridge driver
-> bridge network are single host(Vswitch)
-> 
docker network create -d bridge --subnet 10.0.0.1/24 ps-bridge
docker network inspect ps-bridge

on linux
apt-get install bridge-utils
brctl show
docker network ls
ip link show
docker run -dt --name c1 --network ps-bridge alpine sleed 1d
docker run -dt --name c2 --network ps-bridge alpine sleed 1d
docker network inspect ps-bridge
brctl show -> creates two interfaces

docker exec -it c1 sh
ping c2 
every docker hass DNS server when we create a name for the docker
docker run -d --name web1 --network ps-bridge -p 5000:8080 nginx

#Multi-host Overlay Network
-> Swarm mode

#MACVlan
-> every container gets it own Ip and own mac address on linux but on windows mac address will br shared

#IPVlan
-> mostly cloud friendly

#Network Services
service Discovery
Route mesh


##Docker Compose

```
version: "2"
services: 
   kv-store-1:
      image: redis
   kv-store-2:
       image: redis
```

file2:

```
version: '2'
services:
   elasticsearch:
       image: elasticsearch:2.2.1
   kibana:
       image: kibana:4.4.2
       ports:
         - "5601:5601"
       environment:
         - ELASTICSEARCH_URL=http://elasticsearch:9200
       depends_on:
         - elasticsearch
   logstash:
       image: logstash:2.2.2
       command: -e 'input { tcp { port => 5555 } } output { elasticsearch { hosts => ["elasticsearch:9200"] } }'
       ports:
         - "5555:5555"
       depends_on:
         - elasticsearch
```
```

# References:

 [Introduction to Docker](https://medium.freecodecamp.org/a-beginner-friendly-introduction-to-containers-vms-and-docker-79a9e3e119b) [A must Read]

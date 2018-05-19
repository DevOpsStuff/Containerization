# Containerization
DockerStuff
```
Docker Stuff:

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

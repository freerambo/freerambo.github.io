---
layout:     post
title:      "Microservices training01"
subtitle:   ""
date:       2019-03-10 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    - meetup
    
---

## Microservices training

by john@xx.com

john.cloudlab.jnnapti.io

u/n: john
pwd: john^^%


`Microservices ->  Springboot -> Docker -> kubbernates -> Openshift`


Virtualization

Virtual Machine(VM)


Guest OS
Hypervisor
O/S
H/W(RAM, Processor, Disk, Network)



Cloud : the difference is the way you use it. Managing the virtual machines and services. 

- Openstack to build a private cloud 
- AWS, Azure for public cloud

run on low power mode, only able to receive wake up call. 

- IaaS
- PaaS
- SaaS

Container can run and terminate fast. a jailed process with segregated hardware resources no separate VM. 

Container technology is part of kernal 

Docker as a manager of containers.


RedHat yum install 

Debian apt-get install


`echo $?`

if return 0, means previous command run successfully. 

`history` gives all hostory commands

docker ps list all containers. 

Rule of silence: if there is nothing to say, say nothing.

`rm -rf /tmp/foo/f1` return nothing since rule of silence

`docker pull images`

`docker run` will create a jailed process - container

sudo docker attach dreamy_euclid 

two monitor will be mirror


short switch -i
long switch --name


docker run --help 


ifconfig -- interface config 
on windows, ipconfig



inet : 178.128.81.101 


*CTL + R * for reverse search


```

[yuzhu@lab225 ~]$ docker ps
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
[yuzhu@lab225 ~]$ echo $?
1
[yuzhu@lab225 ~]$ 

systemctl status docker

 cat /usr/lib/systemd/system/docker.service 
 
 [yuzhu@lab225 ~]$ systemctl enable docker  
 
 [yuzhu@lab225 ~]$ sudo docker ps
 CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
 
 
[yuzhu@lab225 ~]$ sudo docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE


[yuzhu@lab225 ~]$ sudo docker run ubuntu:18.04 date
Unable to find image 'ubuntu:18.04' locally
Trying to pull repository docker.io/library/ubuntu ... 
18.04: Pulling from docker.io/library/ubuntu
6cf436f81810: Pull complete 
987088a85b96: Pull complete 
b4624b3efe06: Pull complete 
d42beb8ded59: Pull complete 
Digest: sha256:7a47ccc3bbe8a451b500d2b53104868b46d60ee8f5b35a24b41a86077c650210
Status: Downloaded newer image for docker.io/ubuntu:18.04
Mon Mar 11 04:27:46 UTC 2019
[yuzhu@lab225 ~]$ echo $?
0


[yuzhu@lab225 ~]$ sudo docker run ubuntu:18.04 date
Mon Mar 11 04:28:57 UTC 2019


[yuzhu@lab225 ~]$ sudo docker stop determined_perlman 
determined_perlman

[yuzhu@lab225 ~]$ sudo docker logs determined_perlman 


 docker ps -a | grep -v CONTAINER | awk '{print $NF}'
 
 sudo docker attach dreamy_euclid
 
 
 WM-C02TX3AGHTD8:~ yuzhu$ docker exec -it webserver /bin/bash 

root@197985581a87:/usr/share/nginx/html# echo "<h1>hello from Yuanbo</h1>" > index.html 


exit



sudo docker cp webserver:/usr/share/nginx/html webserver 

[yuzhu@lab225 webserver]$ vi index.html 
[yuzhu@lab225 webserver]$ sudo vi index.html 
[yuzhu@lab225 webserver]$ sudo docker cp index.html webserver:/usr/share/nginx/html

docker run -d --name webserver -p 80:80 nginx 


WM-C02TX3AGHTD8:docker yuzhu$ echo "<h1>Hello from yuabo XXX.</h1>" > index.html
WM-C02TX3AGHTD8:docker yuzhu$ docker cp index.html webserver2:/usr/share/nginx/html
WM-C02TX3AGHTD8:docker yuzhu$ docker commit webserver2 myapp:0.0.1
sha256:57fc60091301bd7778732b5553899885f72cc60c804b8f209a539f2278e1970f

[root@lab225 myapp]# cat Dockerfile 
FROM nginx

RUN echo "<h1>Hello from Dockerfile</h1>" > /usr/share/nginx/html/index.html

[root@lab225 myapp]# docker build -t myapp:0.0.1 .
Sending build context to Docker daemon 2.048 kB
Step 1/2 : FROM nginx
 ---> 881bd08c0b08
Step 2/2 : RUN echo "<h1>Hello from Dockerfile</h1>" > /usr/share/nginx/html/index.html
 ---> Running in 56aee876dbeb

 ---> c4b6f864b819
Removing intermediate container 56aee876dbeb
Successfully built c4b6f864b819

WM-C02TX3AGHTD8:myapp yuzhu$ docker run -d --name myapp -p 9090:80 myapp:0.0.1

docker container ls

```



sudo su  - switch to super user


[root@lab225 yuzhu]# docker rmi myapp:0.0.1
Untagged: myapp:0.0.1
Deleted: sha256:c4b6f864b819474b3906748df364509957eccdd17448b29fecc896f10ee92376
Deleted: sha256:ccb3cc4d2b66b53a69f29141708520b8d95b7b89ce255d533fd0ee6b1343185f



```yaml

docker pull nginx
docker images
docker run --name webserver nginx
docker rm webserver
docker run -d --name webserver nginx
docker ps
docker run --help
docker ps
docker inspect webserver
docker inspect webserver | grep IP
docker rm -f webserver
docker run -d --name webserver -p 80:80 nginx
docker ps
docker exec -it webserver /bin/bash
docker diff webserver
docker images
docker cp 
docker cp webserver:/usr/share/nginx/html webserver
docker cp index.html webserver:/usr/share/nginx/html
docker run -d --name webserver2 -p 6080:80 nginx
docker cp index.html webserver2:/usr/share/nginx/html
docker diff webserver2
cd /var/lib/docker/
docker ps
cd /var/lib/docker/
docker ps
docker stop webserver2
docker diff webserver2
docker commit
docker commit webserver2 myapp:0.0.1
docker images
docker rm -f webserver
docker rm -f webserver2
docker run -d --name webserver -p 80:80 myapp:0.0.1

----------------------------------------------------------
mkdir -p /data/mynginx
cd /data/mynginx
cat > Dockerfile <<DELIM
FROM ubuntu:18.04

RUN apt-get update && apt-get -y install nginx

CMD ["nginx", "-g", "daemon off;"]
DELIM
docker build -t mynginx:0.0.1 .
mkdir -p /data/myapp
cd /data/myapp
cat > Dockerfile <<DELIM
FROM mynginx:0.0.1

RUN echo "<h1>Hello from Dockerfile</h1>" > /var/www/html/index.html
RUN echo "<h1>About Dockerfile</h1>" > /var/www/html/about.html
DELIM
docker build -t myapp:0.0.1 .
docker run -d --name webserver -p 80:80 myapp:0.0.1


FROM ubuntu:18.04

RUN apt-get update && apt-get -y install openjdk-8-jdk 

```


share location between contanier and host server 
```yaml

docker run -it --name java-dev -v /data/src:/src myjdk:0.0.1 /bin/bash

```

---

### END


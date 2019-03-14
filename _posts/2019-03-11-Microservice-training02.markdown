---
layout:     post
title:      "Docker training02"
subtitle:   ""
date:       2019-03-11 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    - training
    
---

## Docker training02

Communication cross containers, cross VMs. 

iputils-ping

net-tools


```yaml

cat > Dockerfile << DELIM
FROM ubuntu:18.04

RUN apt-get update && apt-get -y install iputil-ping net-tools
DELIM


docker exec -it c2 ifconfig 

   49  cat Dockerfile 
   50  vi Dockerfile 
   51  docker build -t ping:0.0.1 .
   52  docker ps
   53  docker images
   54  docker run -itd --name c1 ping:0.0.1 /bin/bash
   55  docker run -itd --name c2 ping:0.0.1 /bin/bash
   56  docker ps
   59  docker inspect c1
   60  docker exec -it c1 /bin/bash
   61  docker exec -it c2 ifconfig 

 
```

Ping dynamicly


```yaml

[root@lab225 ping]# docker run -itd --name c2 ping:0.0.1 /bin/bash
5b7411298f359554b0796953fba4352db7300abc5c7131353c967999951b252c
[root@lab225 ping]# docker run -itd --link c2:c2 --name c1 ping:0.0.1 /bin/bash
4f85a2d31824d6236f040f9d12b561285cbce6d31b81741ab714834aa147906b
[root@lab225 ping]# 

[root@lab225 ping]# docker exec -it c1 /bin/bash
root@4f85a2d31824:/# cat /etc/hosts
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
172.17.0.4      c2 5b7411298f35
172.17.0.5      4f85a2d31824



docker network ls


docker network inspect bridge

[root@lab225 ping]# docker network create --driver bridge net1 --subnet 172.18.0.0/16
db18dbb173f61f02d0c096849bfd24a9ea2d094f44d86dd00e8f3ed7783e1555
[root@lab225 ping]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
cfa4000f7f3a        bridge              bridge              local
f61b0c83c8d6        host                host                local
db18dbb173f6        net1                bridge              local
b23cf81aab3e        none                null                local



[root@lab225 ping]# docker run -itd --name c3 --net net1 ping:0.0.1 /bin/bash
94856b349ed3e1196f2a199236fa2f3c05a8a6b6f486af267b7878e8582c7160

Service discovery. 


[root@lab225 ping]# docker exec -it c3 ping -c2 c2
ping: c2: Name or service not known
[root@lab225 ping]# docker exec -it c3 ping -c1 c4
PING c4 (172.19.0.3) 56(84) bytes of data.
64 bytes from c4.net2 (172.19.0.3): icmp_seq=1 ttl=64 time=0.054 ms

--- c4 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.054/0.054/0.054/0.000 ms

[root@lab225 ping]# docker network connect net2 c2 
[root@lab225 ping]# docker exec -it c3 ping -c1 c2
PING c2 (172.19.0.4) 56(84) bytes of data.
64 bytes from c2.net2 (172.19.0.4): icmp_seq=1 ttl=64 time=0.069 ms





```

### IP Rounte

```yaml

[root@lab225 ping]# ip route
default via 178.128.80.1 dev eth0 
10.15.0.0/16 dev eth0 proto kernel scope link src 10.15.0.30 
172.17.0.0/16 dev docker0 proto kernel scope link src 172.17.0.1 
172.18.0.0/16 dev br-db18dbb173f6 proto kernel scope link src 172.18.0.1 
172.19.0.0/16 dev br-24d6299743ec proto kernel scope link src 172.19.0.1 
178.128.80.0/20 dev eth0 proto kernel scope link src 178.128.81.101 

```

Create Overlay network for containers cross hosts 

proxy server: 

Load banlancer : health check. bridge, forward request  

discovery server: return discovered services


Scalability 
- Geograpgic DNS. 
- Load balance

Failure: 


State: 

App server is **stateless**. 

DB server is **stateful**.  `replication for failure. sharding for scalability`

failure in cloud, it will terminate and start a new one. 

Data has to be persisted and replica. 


## DB Server
```yaml

docker run --name dbserver -e MYSQL_ROOT_PASSWORD=mysecretpassword -d mysql:latest

-e is set enviroment variable to container

 docker exec -it dbserver mysql -uroot -p
 
 docker run --rm  
 
 remove the container once container job done. 
 
 [root@lab225 data]# docker run --rm -v /data/spring-jpa:/src -v /data/.m2:/root/.m2 -it myjdk:0.0.1 /bin/bash
 
 ./mvnw clean install  
 
  docker run \
  --rm \
  -v /data/spring-jpa:/src \
  -w /src \
  --link dbserver:dbserver \
  -e DB_HOST=dbserver \
 -p 80:8080 \
  myjdk:8 bash -c "java -jar target/*.jar"
 
```
Docker Run a springboot with mysql

```yaml
docker run --name dbserver -e MYSQL_ROOT_PASSWORD=mysecretpassword -d mysql

docker exec -it dbserver mysql -uroot -p
cd /data/
git clone https://github.com/buzypi/spring-jpa
docker run --rm -v /data/spring-jpa:/src -v /data/.m2:/root/.m2 -w /src myjdk:8 bash -c "./mvnw clean install"
docker run --rm -v /data/spring-jpa:/src -w /src --link dbserver:dbserver -e DB_HOST=dbserver -p 80:8080 myjdk:8 bash -c "java -jar target/*.jar"



[root@lab225 data]# docker cp dbserver:/var/lib/mysql /data/

docker run --name dbserver -v /data/mysql:/var/lib/mysql --net net1 -d mysql


[root@lab225 data]# docker run --rm --name dbserver -v /data/mysql:/var/lib/mysql --net net1 -d mysql
0033609a65629137a55d5805c629cc60d3a2530090048377c2c2cf738a702dd0


 docker exec -it dbserver mysql -uroot -p

[root@lab225 data]# docker run --name dbserver -v /data/mysql:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=mysecretpassword --net net1 -d mysql 
5d961deeb5028b045807b7fc28bf60249547bd007caeec73c81188bf7a275361
[root@lab225 data]# echo $?
0



docker run --rm --name dbserver -v /data/mysql:/var/lib/mysql --net net1 -d mysql

docker exec -it dbserver mysql -uroot -p

# Inside mysql:
show databases;
use peopledb
show tables;
select * from contact;

docker run --rm -v /data/spring-jpa:/src -v /data/.m2:/root/.m2 -w /src myjdk:0.0.1 bash -c "./mvnw clean install"
docker run --rm -v /data/spring-jpa:/src -w /src --net net1 -e DB_HOST=dbserver -p 80:8080 myjdk:0.0.1 bash -c "java -jar target/*.jar"

docker run --rm -d --net net1 -e DB_HOST=dbserver -p 80:8080 myspringapp:0.0.1

# JDK8
mkdir -p /data/myjdk
cd /data/myjdk
cat > Dockerfile <<DELIM
FROM ubuntu:18.04

RUN apt-get update && apt-get -y install openjdk-8-jdk
DELIM
docker build -t myjdk:8 .
# Network creation
docker network create --driver bridge net1


docker run --rm --name dbserver -v /data/mysql:/var/lib/mysql --net net1 -d mysql

docker exec -it dbserver mysql -uroot -p

# Inside mysql:
show databases;
use peopledb
show tables;
select * from contact;

docker run --rm -v /data/spring-jpa:/src -v /data/.m2:/root/.m2 -w /src myjdk:8 bash -c "./mvnw clean install"
docker run --rm -v /data/spring-jpa:/src -w /src --net net1 -e DB_HOST=dbserver -p 80:8080 myjdk:8 bash -c "java -jar target/*.jar"

mkdir -p /data/myspringapp
cd /data/myspringapp
cat > Dockerfile <<DELIM
FROM myjdk:8

RUN apt-get update \
 && apt-get -y install git \
 && cd /tmp \
 && git clone https://github.com/buzypi/spring-jpa \
 && cd spring-jpa \
 && ./mvnw clean install \
 && mkdir -p /srv \
 && mv target/*.jar /srv \
 && rm -rf /tmp/spring-jpa /root/.m2

WORKDIR /srv

CMD ["java", "-jar", "demo-0.0.1-SNAPSHOT.jar"]
DELIM
docker build -t myspringapp:0.0.1 .

docker run --rm -d --name appserver --net net1 -e DB_HOST=dbserver -p 80:8080 myspringapp:0.0.1


# JDK8
mkdir -p /data/myjdk
cd /data/myjdk
cat > Dockerfile <<DELIM
FROM ubuntu:18.04

RUN apt-get update && apt-get -y install openjdk-8-jdk
DELIM
docker build -t myjdk:8 .
# Network creation
docker network create --driver bridge net1

FROM myjdk:0.0.1

RUN apt-get update \
&& apt-get -y install git \
&& cd /tmp \
&& git clone https://github.com/buzypi/spring-jpa \
&& cd spring-jpa \
&& .mvnw clean install \
&& mkdir -p /srv \
&& mv target/*.jar /srv \
&& rm -rf /tmp/spring-jpa /root/.m2 




```



Orchestration

1. Docker compose, swarm, machine
2. Kubernates
3. Openshift


```yaml

docker-compose.yaml
version: '3'
services:
  appserver:
    image: "myspringapp:0.0.1" 
    environment:
      DB_HOST: dbserver
    networks:
    - app_net
    ports:
     - "80:8080"
  dbserver:
    networks:
    - app_net
    volumes:
    - "/data/mysql:/var/lib/mysql"
    image: "mysql:latest"
networks:
  app_net:
    driver: bridge
    
    
    
    
    
```


https://docs.docker.com/compose/gettingstarted/

docker tag ping:old ping:0.0.1


version: '3'
services:
  ping:
    image: "ping:0.0.1" 
    deploy:
      replicas: 3
    networks:
    - ping_net
    command:["ping", "8.8.8.8"]
networks:
  ping_net:
    driver: overlay


```yaml


version: '3'
services:
  ping:
    image: "ping:0.0.1"
    deploy:
      replicas: 3
    networks:
    - ping-net
    command: ["ping", "8.8.8.8"]
networks:
  ping-net:
    driver: overlay

docker stack deploy --compose-file docker-compose.yml ping
docker service ls
docker service ps ping_ping

[root@lab225 ping]# docker network inspect ping_ping-net 
[
    {
        "Name": "ping_ping-net",
        "Id": "r3c1xp9i82abtptet1xxzbt12",
        "Created": "2019-03-12T09:01:11.92971119Z",
        "Scope": "swarm",
        "Driver": "overlay",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "10.0.0.0/24",
                    "Gateway": "10.0.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Containers": {
            "cb76fa94b4aa671b980fc677012b6431037e8cfeb744d07f1a93b218cc66497e": {
                "Name": "ping_ping.1.0dhjyajr5yj4g6iiqzlas1sza",
                "EndpointID": "f241639ea798ddaf682f29dd5d567c94a2dc3d934fd610771699f518f8b86907",
                "MacAddress": "02:42:0a:00:00:03",
                "IPv4Address": "10.0.0.3/24",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.driver.overlay.vxlanid_list": "4097"
        },
        "Labels": {
            "com.docker.stack.namespace": "ping"
        },
        "Peers": [
            {
                "Name": "lab225-d34a66b6275c",
                "IP": "178.128.81.101"
            },
            {
                "Name": "lab210-a3f5823bf799",
                "IP": "178.128.85.179"
            }
        ]
    }
]


systemctl stop docker 


```
---

### END


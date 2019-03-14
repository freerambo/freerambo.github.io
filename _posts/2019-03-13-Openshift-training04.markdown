---
layout:     post
title:      "Openshift training04"
subtitle:   ""
date:       2019-03-14 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    - training
    
---

## Opeshift training04


### NOTES

```dockerfile
yum install wget docker vim git
cd /data
wget "https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz"
tar -xzvf openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz 
cd openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit
export PATH=$PATH:/data/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit/

cat > /etc/docker/daemon.json <<DELIM
{
   "insecure-registries": [
     "172.30.0.0/16"
   ]
}
DELIM
service docker restart

oc cluster up --base-dir="/data/clusterup" --public-hostname=178.128.81.101

# Uninstall - if there are problems
oc cluster down
for i in $(mount | grep openshift | awk '{ print $3}'); do sudo umount "$i"; done
sudo rm -rf /data/clusterup
sudo rm -rf /etc/origin /etc/kubernetes
mv ~/.kube ~/.kube-old

# VNC
docker run -p 6080:80 -d --name vnc dorowu/ubuntu-desktop-lxde-vnc

```
 
 
 
 
 ```
 You are logged in as:
     User:     developer
     Password: <any value>
 
 To login as administrator:
     oc login -u system:admin
     
 ```
 
 mount storage 
 external IP should be preconfigured
 
 1. persistent volumes 
 2. persistent volumes claims
 
 
 
 list all devices
 
 
 mount /dev/xvda1
 umount /dev/xvda1
 
  
 ```dockerfile
ls /dev

```
 
 
 ### Openshift run kubernets file:
 
 ```
 
 apiVersion: v1
 kind: PersistentVolume
 metadata:
   name: db-volume
   labels:
     type: local
 spec:
   storageClassName: manual
   capacity:
     storage: 5Gi
   accessModes:
     - ReadWriteOnce
   hostPath:
     path: '/data/mysql'
 ---
 apiVersion: v1
 kind: PersistentVolumeClaim
 metadata:
   name: db-volume-claim
 spec:
   storageClassName: manual
   accessModes:
     - ReadWriteOnce
   resources:
     requests:
       storage: 4Gi
 ---
 apiVersion: apps/v1
 kind: Deployment
 metadata:
   name: dbserver
 spec:
   replicas: 1
   selector:
     matchLabels:
       app: dbserver
   template:
     metadata:
       labels:
         app: dbserver
     spec:
       volumes:
       - name: db-storage
         persistentVolumeClaim:
           claimName: db-volume-claim
       containers:
       - name: mysql
         image: mysql
         volumeMounts:
         - mountPath: "/var/lib/mysql"
           name: db-storage
 ---
 apiVersion: v1
 kind: Service
 metadata:
   name: dbserver
 spec:
   selector:
     app: dbserver
   ports:
   - protocol: TCP
     port: 3306
     targetPort: 3306
 ---
 apiVersion: apps/v1
 kind: Deployment
 metadata:
   name: appserver
 spec:
   replicas: 1
   selector:
     matchLabels:
       app: appserver
   template:
     metadata:
       labels:
         app: appserver
     spec:
       containers:
       - name: springapp
         image: myspringapp:0.0.1
         imagePullPolicy: Never
         env:
         - name: DB_HOST
           value: dbserver
 ---
 apiVersion: v1
 kind: Service
 metadata:
   name: appserver
 spec:
   selector:
     app: appserver
   ports:
   - protocol: TCP
     port: 80
     targetPort: 8080

 ```
 
 
 
 
 ```
 cd /data/
 git clone https://github.com/buzypi/express-hello
 cd express-hello/
 export PATH=$PATH:/opt/node/bin/
 npm install
 node app.js 
 # In another browser
 curl localhost:3000
 
 ```
 
 -P 80
 -p 8080:80
 
 
 
 
 ```dockerfile
 
 apiVersion: build.openshift.io/v1
 kind: BuildConfig
 metadata:
   name: express-bc
 spec:
   output:
     to:
       kind: ImageStreamTag
       name: express-hello:latest
   source:
     git:
       uri: https://github.com/buzypi/express-hello
     type: Git
   strategy:
     dockerStrategy:
       from:
         kind: ImageStreamTag
         name: ubuntu:18.04
     type: Docker


 1138  oc start-build bc/express-bc
 1139  oc get builds
 1140  oc start-build bc/express-bc
 1141  oc get builds
 1142  oc logs express-bc-2
 1143  oc get pods
 1144  oc logs express-bc-2-build
 
 
 oc get dc
 
 oc get builds
 
 oc rollout latest dc/express-dc
 
 oc rollout undo dc/express-dc
 
 
  oc describe pod express-dc-5-vjr96 | grep IP
  
  image streams
  
  oc get is
  
  oc describe is/express-hello
  
  
```

Github Webhook


```yaml

apiVersion: apps.openshift.io/v1
kind: DeploymentConfig
metadata:
  name: express-dc
spec:
  replicas: 1
  selector:
    app: express-app
  template:
    metadata:
      labels:
        app: express-app
    spec:
      containers:
      - name: express
        image: express-hello:latest
  triggers:
  - type: ConfigChange
  - imageChangeParams:
      automatic: true
      containerNames:
      - express
      from:
        kind: ImageStreamTag
        name: express-hello:latest
    type: ImageChange
---
apiVersion: build.openshift.io/v1
kind: BuildConfig
metadata:
  name: express-bc
spec:
  output:
    to:
      kind: ImageStreamTag
      name: express-hello:latest
  source:
    git:
      uri: https://github.com/buzypi/express-hello
    type: Git
  strategy:
    dockerStrategy:
      from:
        kind: ImageStreamTag
        name: ubuntu:18.04
    type: Docker
  triggers:
  - type: GitHub
    github:
      secret: mysecretkey
---
apiVersion: image.openshift.io/v1
kind: ImageStream
metadata:
  name: ubuntu
spec:
  lookupPolicy:
    local: false
  tags:
  - annotations:
      openshift.io/imported-from: ubuntu:18.04
    from:
      kind: DockerImage
      name: ubuntu:18.04
    importPolicy: {}
    name: "18.04"
    referencePolicy:
      type: Source

```


S2I images, sourcecode to image 

```shell

[root@lab225 clusterup]# find . -name master-config.yaml
./kube-apiserver/master-config.yaml
./openshift-apiserver/master-config.yaml
./openshift-controller-manager/master-config.yaml
[root@lab225 clusterup]# 


[root@lab225 clusterup]# oc get is
NAME            DOCKER REPO                               TAGS      UPDATED
express-hello   172.30.1.1:5000/myproject/express-hello   latest    22 minutes ago
myapp           172.30.1.1:5000/myproject/myapp           latest    7 hours ago
ubuntu          172.30.1.1:5000/myproject/ubuntu          18.04     4 hours ago


```
 
 #### Delete all deployment,dc,bc,svc,is,pods
```shell
 [root@lab225 clusterup]# oc delete deployment,dc,bc,svc,is,pods --all
``` 
 
---

### END


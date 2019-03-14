---
layout:     post
title:      "Kubernetes training03"
subtitle:   ""
date:       2019-03-13 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    - training
    
---

## Kubernetes training03


No minikube.

sudo su -- switch to root user. 

```dockerfile

cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kube*
EOF

# Set SELinux in permissive mode (effectively disabling it)
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

systemctl enable kubelet && systemctl start kubelet
systemctl enable docker && systemctl start docker

kubeadm init --pod-network-cidr=10.244.0.0/16

kubeadm join --token 67ad79.fc0cff959fe632b6 128.199.138.184:6443 \
--discovery-token-ca-cert-hash sha256:0126afa821b8a76040e57362cfb901f7d92fb6d23858a91aee01b5a86c5bc6ee\
--ignore-preflight-errors=all

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f \
https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml

## apply will do update if exists


# Untaint master
kubectl taint nodes $(hostname) node-role.kubernetes.io/master:NoSchedule-

# Ensure that the node is ready before you run anything else
kubectl get nodes

kubectl run hello --image ubuntu:18.04 -- bash -c "while true;do echo Hello;sleep 1;done"

[root@lab225 yuzhu]# kubectl get deployment
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
hello   1/1     1            1           8m28s

  811  kubectl exec -it hello-5d957b94f4-4jt5d /bin/bash
  813  kubectl attach hello-5d957b94f4-4jt5d
  814  kubectl logs -f hello-5d957b94f4-4jt5d 
  
  
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: nginx-deployment
    labels:
      app: nginx-app
  spec:
    replicas: 5
    selector:
      matchLabels:
        app: nginx-app
    template:
      metadata:
        labels:
          app: nginx-app
      spec:
        containers:
        - name: nginx
          image: nginx
  ---
  apiVersion: v1
  kind: Service
  metadata:
    name: nginx-service
    labels:
      app: nginx-app
  spec:
    selector:
      app: nginx-app
    ports:
    - protocol: TCP
      port: 80
      targetPort: 80
    externalIPs:
    - 206.189.32.86 
  
  kubectl apply -f myfirstdeployment.yaml
  kubectl get all -l app=nginx-app


```



Self notes

status check 

memory & disk checks 

```dockerfile

[root@lab225 yuzhu]# systemctl status docker 
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Tue 2019-03-12 09:23:43 UTC; 16h ago
     Docs: http://docs.docker.com
 Main PID: 9965 (dockerd-current)
   CGroup: /system.slice/docker.service
           ├─9965 /usr/bin/dockerd-current --add-runtime docker-runc=/usr/libexec/docker/docker-runc-current --default-runtime=docker-runc --exec...
           └─9971 /usr/bin/docker-containerd-current -l unix:///var/run/docker/libcontainerd/docker-containerd.sock --metrics-interval=0 --start-...

Mar 12 09:23:43 lab225 dockerd-current[9965]: time="2019-03-12T09:23:43.241289998Z" level=info msg="Default bridge (docker0) is assigned ...address"
Mar 12 09:23:43 lab225 dockerd-current[9965]: time="2019-03-12T09:23:43.287154937Z" level=info msg="Loading containers: done."
Mar 12 09:23:43 lab225 dockerd-current[9965]: time="2019-03-12T09:23:43.310598750Z" level=info msg="Daemon has completed initialization"
Mar 12 09:23:43 lab225 dockerd-current[9965]: time="2019-03-12T09:23:43.310635328Z" level=info msg="Docker daemon" commit="07f3374/1.13.1...n=1.13.1
Mar 12 09:23:43 lab225 dockerd-current[9965]: time="2019-03-12T09:23:43.315094266Z" level=info msg="API listen on /var/run/docker.sock"
Mar 12 09:23:43 lab225 systemd[1]: Started Docker Application Container Engine.
Mar 12 09:24:22 lab225 dockerd-current[9965]: time="2019-03-12T09:24:22.069124493Z" level=error msg="Handler for DELETE /v1.26/images/4c9...:latest"
Mar 12 09:24:22 lab225 dockerd-current[9965]: time="2019-03-12T09:24:22.069864774Z" level=error msg="Handler for DELETE /v1.26/images/4c9...:latest"
Mar 12 09:24:28 lab225 dockerd-current[9965]: time="2019-03-12T09:24:28.356021588Z" level=error msg="Handler for DELETE /v1.26/images/4c9...:latest"
Mar 12 09:24:38 lab225 dockerd-current[9965]: time="2019-03-12T09:24:38.670505108Z" level=error msg="Handler for DELETE /v1.26/images/4c9...:latest"
Hint: Some lines were ellipsized, use -l to show in full.
[root@lab225 yuzhu]# free -m
              total        used        free      shared  buff/cache   available
Mem:           3790         200         894          40        2694        3183
Swap:             0           0           0
[root@lab225 yuzhu]# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/vda1        80G  4.2G   76G   6% /
devtmpfs        1.9G     0  1.9G   0% /dev
tmpfs           1.9G     0  1.9G   0% /dev/shm
tmpfs           1.9G   41M  1.9G   3% /run
tmpfs           1.9G     0  1.9G   0% /sys/fs/cgroup
tmpfs           380M     0  380M   0% /run/user/1001



kubectl describe nodes
kubectl get nodes


kubectl taint nodes $(hostname) node-role.kubernetes.io/master:NoSchedule-   (- for remove the master:NoSchedule)
 
[root@lab225 yuzhu]# kubectl get pods
NAME                     READY   STATUS              RESTARTS   AGE
hello-5d957b94f4-4jt5d   0/1     ContainerCreating   0          8s


kubectl get deployments

kubectl delete deployment hello 




```


YAML 


A sinngle pod can has many containers. a collection of containers. tightly coupled containers. 

TAG may be the same. Digests of image can be differ. 

```dockerfile



docker images --digests | grep nginx



[root@lab225 data]# kubectl describe deployments

NewReplicaSet:   nginx-deployment-68684b6b67 (2/2 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  7m50s  deployment-controller  Scaled up replica set nginx-deployment-68684b6b67 to 1
  Normal  ScalingReplicaSet  6m31s  deployment-controller  Scaled up replica set nginx-deployment-68684b6b67 to 3
  Normal  ScalingReplicaSet  5m3s   deployment-controller  Scaled down replica set nginx-deployment-68684b6b67 to 2
  
  
  [root@lab225 data]# kubectl get rs
  NAME                          DESIRED   CURRENT   READY   AGE
  nginx-deployment-68684b6b67   2         2         2       8m36s
  
  
  kubectl expose deployment nginx-deployment --port 80 --target-port 80
  kubectl get services
  
  [root@lab225 data]# kubectl get endpoints
  NAME               ENDPOINTS                     AGE
  kubernetes         178.128.81.101:6443           179m
  nginx-deployment   10.244.0.6:80,10.244.0.7:80   44s
  
  [root@lab225 data]# kubectl get services
  NAME               TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
  kubernetes         ClusterIP   10.96.0.1      <none>        443/TCP   3h
  nginx-deployment   ClusterIP   10.109.83.82   <none>        80/TCP    60s
  
  [root@lab225 data]# kubectl get all -l app=nginx-app
  NAME                                    READY   STATUS    RESTARTS   AGE
  pod/nginx-deployment-68684b6b67-2rw78   1/1     Running   0          66m
  pod/nginx-deployment-68684b6b67-98pwn   1/1     Running   0          66m
  pod/nginx-deployment-68684b6b67-fnjcl   1/1     Running   0          110m
  pod/nginx-deployment-68684b6b67-l8p8v   1/1     Running   0          66m
  pod/nginx-deployment-68684b6b67-z2dd9   1/1     Running   0          108m
  
  NAME                                          DESIRED   CURRENT   READY   AGE
  replicaset.apps/nginx-deployment-68684b6b67   5         5         5       110m
  
  
  
  [root@lab225 data]# kubectl get svc
  NAME               TYPE        CLUSTER-IP      EXTERNAL-IP      PORT(S)   AGE
  appserver          ClusterIP   10.108.214.88   <none>           80/TCP    116s
  kubernetes         ClusterIP   10.96.0.1       <none>           443/TCP   4h32m
  nginx-deployment   ClusterIP   10.109.83.82    <none>           80/TCP    93m
  nginx-service      ClusterIP   10.107.89.60    178.128.81.101   80/TCP    21m
  [root@lab225 data]# kubectl get svc --all-namespaces
  NAMESPACE     NAME               TYPE        CLUSTER-IP      EXTERNAL-IP      PORT(S)         AGE
  default       appserver          ClusterIP   10.108.214.88   <none>           80/TCP          2m45s
  default       kubernetes         ClusterIP   10.96.0.1       <none>           443/TCP         4h33m
  default       nginx-deployment   ClusterIP   10.109.83.82    <none>           80/TCP          94m
  default       nginx-service      ClusterIP   10.107.89.60    178.128.81.101   80/TCP          22m
  kube-system   kube-dns           ClusterIP   10.96.0.10      <none>           53/UDP,53/TCP   4h33m
  
```
  
  
### lecture note

```

cat > appclientserver.yaml <<DELIM
apiVersion: apps/v1
kind: Deployment
metadata:
  name: appserver
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-app
  template:
    metadata:
      labels:
        app: nginx-app
    spec:
      containers:
      - name: nginx
        image: nginx
---
apiVersion: v1
kind: Service
metadata:
  name: appserver
spec:
  selector:
    app: nginx-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: appclient
spec:
  replicas: 1
  selector:
    matchLabels:
      app: appclient
  template:
    metadata:
      labels:
        app: appclient
    spec:
      containers:
      - name: curl
        image: radial/busyboxplus:curl
        stdin: true
        tty: true
DELIM

kubectl apply -f appclientserver.yaml 
kubectl get svc
kubectl get pods
# After they are both running:
kubectl exec -it appclient-<podid> /bin/sh
# Inside the container
nslookup appserver
curl appserver

```


### kubernetes UI

```shell

kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
kubectl get svc --all-namespaces
kubectl edit svc kubernetes-dashboard -n kube-system
# Add
# externalIPs:
# - ...
# to the spec
# Verify
kubectl get svc --all-namespaces
# Go to: https://<user>-443.cloudlab.jnaapti.io/

# Create a user
kubectl create serviceaccount dashboard -n default
kubectl create clusterrolebinding dashboard-admin -n default --clusterrole=cluster-admin --serviceaccount=default:dashboard
kubectl get secret $(kubectl get serviceaccount dashboard -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode

```




service is the interface to client. maping the pods.

Deployment manage the coupling and replicas of pods. 

PODs is the collection of containers. 



```

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
      - hostPath:
          path: "/data/mysql"
        name: db-storage
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
  externalIPs:
  - 206.189.32.86


```

kubectl create namespace myns

kubectl config get-contexts

cluster - user - namespace


```dockerfile
cd /usr/local/bin
wget "https://raw.githubusercontent.com/ahmetb/kubectx/master/kubens"
chmod +x kubens
cd
export PATH=$PATH:/usr/local/bin
kubens
kubens myns
kubens
kubens default

kubectl create serviceaccount dashboard -n default
kubectl create clusterrolebinding dashboard-admin -n default --clusterrole=cluster-admin --serviceaccount=default:dashboard
x=`kubectl get serviceaccount dashboard -o jsonpath="{.secrets[0].name}"`
kubectl get secret $x -o jsonpath="{.data.token}" | base64 --decode

kind: Role
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: pod-reader
rules:
- apiGroups: ["", "extensions", "apps"] # "" indicates the core API group
  resources: ["pods", "deployments"]
  verbs: ["get", "watch", "list"]
---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: read-pods
subjects:
- kind: User
  name: system:serviceaccount:default:dashboard # Name is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role #this must be Role or ClusterRole
  name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to
  apiGroup: rbac.authorization.k8s.io


  467  cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kube*
EOF

  468  # Set SELinux in permissive mode (effectively disabling it)
  469  setenforce 0
  470  sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
  471  yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
  472  echo $?
  473  systemctl enable kubelet && systemctl start kubelet
  474  systemctl enable docker && systemctl start docker
  475  docker ps
  476  systemctl status docker
  477  free -m
  478  df -h
  479  kubeadm init --pod-network-cidr=10.244.0.0/16
  480  kubeadm join 206.189.32.86:6443 --token xohqnh.afgvnaw5irhg009p --discovery-token-ca-cert-hash sha256:034806372a0cb3f24cdcb3f9949c86178d1a5634232abde4bb377718b5af2c59
  481  kubeadm join 206.189.32.86:6443 --token xohqnh.afgvnaw5irhg009p --discovery-token-ca-cert-hash sha256:034806372a0cb3f24cdcb3f9949c86178d1a5634232abde4bb377718b5af2c59 --ignore-preflight-errors=all
  482  mkdir -p $HOME/.kube
  483  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  484  sudo chown $(id -u):$(id -g) $HOME/.kube/config
  485  cat $HOME/.kube/config 
  486  vim $HOME/.kube/config 
  487  kubectl get nodes
  488  kubectl describe nodes
  489  kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
  490  kubectl get nodes
  491  ifconfig
  492  kubectl describe nodes
  493  kubectl taint nodes $(hostname) node-role.kubernetes.io/master:NoSchedule-
  494  kubectl describe nodes
  495  kubectl get pods
  496  docker ps
  497  kubectl get pods
  498  kubectl run hello --image ubuntu:18.04 -- bash -c "while true;do echo Hello;sleep 1;done"
  499  kubectl get pods
  500  kubectl describe pods
  501  docker ps
  502  docker logs -f k8s_hello_hello-5d957b94f4-5kx55_default_67b8fe41-453b-11e9-ae2d-b213f329f280_0
  503  docker rm -f k8s_hello_hello-5d957b94f4-5kx55_default_67b8fe41-453b-11e9-ae2d-b213f329f280_0
  504  kubectl get pods
  505  docker ps
  506  docker images
  507  kubectl get pods
  508  kubectl get deployment
  509  kubectl describe deployment
  510  kubectl get pods
  511  kubectl exec -it hello-5d957b94f4-5kx55 /bin/bash
  512  kubectl atach hello-5d957b94f4-5kx55 
  513  kubectl attach hello-5d957b94f4-5kx55 
  514  kubectl logs -f hello-5d957b94f4-5kx55 
  515  history | tail -n5
  516  df -h
  517  kubectl get deployments
  518  kubectl delete deployment hello
  519  kubectl get pods
  520  cd /data/
  521  vim myfirstpod.yaml
  522  kubectl apply -f myfirstpod.yaml 
  523  kubectl get pods
  524  kubectl describe pod nginx-pod
  525  ping 10.244.0.6
  526  curl 10.244.0.6
  527  kubectl describe pod nginx-pod | grep IP
  528  ping 10.244.0.6
  529  kubectl describe pod nginx-pod
  530  docker images --digests
  531  docker images --digests | grep nginx
  532  kubectl describe pod nginx-pod | grep Image
  533  vim myfirstpod.yaml 
  534* 
  535  kubectl get pods
  536  kubectl describe pod nginx-pod | grep Image
  537  docker images --digests | grep nginx
  538  vim myfirstpod.yaml 
  539  kubectl describe pod nginx-pod | grep Image
  540  kubectl describe pod nginx-pod | grep IP
  541  curl 10.244.0.6
  542  kubectl get pods
  543  kubectl exec -it nginx-pod /bin/bash
  544  kubectl exec -it nginx-pod /bin/sh
  545  curl 10.244.0.6
  546  cp myfirstpod.yaml myfirstdeployment.yaml
  547  vim myfirstdeployment.yaml 
  548  kubectl apply -f myfirstdeployment.yaml 
  549  vim myfirstdeployment.yaml 
  550  kubectl apply -f myfirstdeployment.yaml 
  551  kubectl get deployments
  552  kubectl get pods
  553  kubectl delete -f myfirstpod.yaml 
  554  kubectl get pods
  555  kubectl describe pods | grep IP
  556  curl 10.244.0.7
  557  vim myfirstdeployment.yaml 
  558  kubectl apply -f myfirstdeployment.yaml 
  559  kubectl get deployments
  560  kubectl get pods
  561  kubectl describe deployments
  562  kubectl get rs
  563  kubectl get replicasets
  564  kubectl describe rs nginx-deployment-68684b6b67
  565  vim myfirstdeployment.yaml 
  566  kubectl get pods
  567  kubectl get deployments
  568  kubectl expose deployment nginx-deployment --port 80 --target-port 80 
  569  kubectl get services
  570  curl 10.107.24.12
  571  kubectl get endpoints
  572  history | tail -n10
  573  kubectl get services
  574  vim myfirstdeployment.yaml 
  575  kubectl apply -f myfirstdeployment.yaml 
  576  kubectl get endpoints
  577  kubectl get pods
  578  kubectl get endpoints
  579  history | tail -n10
  580  history | tail -n20
  581  cat /proc/sys/net/bridge/bridge-nf-call-iptables 
  582  vim myfirstdeployment.yaml 
  583  kubectl apply -f myfirstdeployment.yaml 
  584  kubectl get svc
  585  kubectl delete svc nginx-deployment
  586  cat myfirstdeployment.yaml 
  587  vim myfirstdeployment.yaml 
  588  ifconfig eth0
  589  fg
  590  kubectl apply -f myfirstdeployment.yaml 
  591  kubectl get svc
  592  cat myfirstdeployment.yaml 
  593  kubectl get all
  594  kubectl get all -l app=nginx-app
  595  vim myfirstdeployment.yaml 
  596  kubectl get all -l app=nginx-app
  597  kubectl apply -f myfirstdeployment.yaml 
  598  kubectl get all -l app=nginx-app
  599  cat myfirstdeployment.yaml 
  600  kubectl get all -l app=nginx-app
  601  netstat -nltp
  602  docker ps
  603  ifconfig eth0
  604  vim myfirstdeployment.yaml 
  605  kubectl get all -l app=nginx-app
  606  kubectl apply -f myfirstdeployment.yaml 
  607  kubectl get svc
  608  kubectl get ep
  609  vim myfirstdeployment.yaml 
  610  kubectl apply -f myfirstdeployment.yaml 
  611  kubectl delete -f myfirstdeployment.yaml 
  612  kubectl get all -l app=nginx-app
  613  vim appclientserver.yaml
  614  kubectl apply -f appclientserver.yaml 
  615  kubectl get pods
  616  kubectl exec -it appclient-bcb4b7b5-vlqt7 /bin/sh
  617  kubectl get svc
  618  history | tail -n10
  619  cat appclientserver.yaml 
  620  kubectl exec -it appclient-bcb4b7b5-vlqt7 /bin/sh
  621  kubectl get svc
  622  kubectl get svc --all-namespaces
  623  kubectl exec -it appclient-bcb4b7b5-vlqt7 /bin/sh
  624  kubectl get pods
  625  kubectl delete -f appclientserver.yaml 
  626  vim appclientserver.yaml
  627  kubectl apply -f appclientserver.yaml 
  628  kubectl get pods
  629  kubectl exec -it appserver-65cd9549b8-ddfrp /bin/sh
  630  kubectl exec -it -c curl appserver-65cd9549b8-ddfrp /bin/sh
  631  kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
  632  kubectl proxy
  633  kubectl get svc
  634  kubectl get svc --all-namespaces
  635  kubectl edit svc kubernetes-dashboard
  636  kubectl edit svc kubernetes-dashboard -n kube-system
  637  ifconfig eth0
  638  fg
  639  kubectl get svc --all-namespaces
  640  kubectl create serviceaccount dashboard -n default
  641  kubectl create clusterrolebinding dashboard-admin -n default --clusterrole=cluster-admin --serviceaccount=default:dashboard
  642  kubectl get secret $(kubectl get serviceaccount dashboard -o jsonpath="{.secrets[0].name}") -o jsonpath="{.data.token}" | base64 --decode
  643* 
  644  vim myfirstdeployment.yaml 
  645  kubectl get svc 
  646  kubectl get svc --all-namespaces
  647  docker images
  648  cd mysql/
  649  pwd
  650  ls
  651  cd ../myspringapp/
  652  vim docker-compose.yml 
  653  vim kube-app.yaml
  654  kubectl apply -f kube-app.yaml 
  655  vim kube-app.yaml
  656  kubectl apply -f kube-app.yaml 
  657  vim kube-app.yaml
  658  kubectl apply -f kube-app.yaml 
  659  kubectl get deployments
  660  kubectl delete deployment/appserver
  661  kubectl get all 
  662  vim kube-app.yaml
  663  kubectl apply -f kube-app.yaml 
  664  kubectl get all
  665  vim kube-app.yaml
  666  kubectl apply -f kube-app.yaml 
  667  kubectl get all
  668  kubectl logs -f pod/appserver-6cdd46b579-79clh
  669  kubectl describe pod/appserver-6cdd46b579-79clh | grep IP
  670  curl 10.244.0.18/contacts
  671  curl 10.244.0.18:8080/contacts
  672  vim kube-app.yaml
  673  ifconfig eth0
  674  fg
  675  kubectl apply -f kube-app.yaml 
  676  kubectl get all
  677  vim kube-app.yaml 
  678  cat kube-app.yaml 
  679  kubectl get pods
  680  kubectl get namespaces
  681  docker ps
  682  kubectl get pods
  683  kubectl get pods --all-namespaces
  684  kubectl create namespace myns
  685  kubectl get namespaces
  686  kubectl delete -f kube-app.yaml 
  687  vim kube-app.yaml 
  688  kubectl apply -f kube-app.yaml 
  689  kubectl get deployments
  690* kubectl get svc -
  691  kubectl get deployments --all-namespaces
  692  kubectl get pods
  693  kubectl get pods --all-namespaces
  694  kubectl get pods -n myns
  695  kubectl get all -n myns
  696  kubectl config get-contexts
  697  vim ~/.kube/config 
  698  kubectl config set-context kubernetes-admin@kubernetes --namespace myns
  699  kubectl config get-contexts
  700  kubectl get pods
  701  kubectl config get-contexts
  702  kubectl get roles
  703  kubectl get roles --all-namespaces
  704  kubectl get rolebindigs --all-namespaces
  705  kubectl get rolebindings --all-namespaces
  706  kubectl get clusterroles --all-namespaces
  707  kubectl get clusterrolebindings --all-namespaces
  708  kubectl describe clusterrolebinding/cluster-admin
  709  kubectl describe clusterrolebinding/dashboard-admin
  710  kubectl get pods
  711  kubectl config get-contexts
  712  kubectl run hello --image ubuntu:18.04 -- bash -c "while true;do echo Hello;sleep 1;done"
  713  kubectl get pods
  714  kubectl get pods --all-namespaces
  715  cd /usr/local/bin/
  716  wget "https://raw.githubusercontent.com/ahmetb/kubectx/master/kubens"
  717  chmod +x kubens 
  718  cd
  719  kubens
  720  kubens default
  721  kubens
  722  kubectl get pods
  723  kubens myns
  724  kubectl get pods
  725  history | tail -n10
  726  kubectl get serviceaccounts
  727  kubectl get serviceaccounts -n default
  728  kubens
  729  kubens default
  730  kubectl get serviceaccount dashboard
  731  kubectl get pods
  732  kubectl get all
  733  kubens myns
  734  kubectl get pods
  735  kubectl get pod hello-5d957b94f4-kt6hh
  736  kubectl describe pod hello-5d957b94f4-kt6hh
  737  kubectl get pod hello-5d957b94f4-kt6hh
  738  kubectl get pod hello-5d957b94f4-kt6hh -o yaml
  739  kubectl get pod hello-5d957b94f4-kt6hh -o json
  740  kubectl get pod hello-5d957b94f4-kt6hh -o json | less
  741  kubectl get pod hello-5d957b94f4-kt6hh -o jsonpath "{.spec}"
  742  kubectl get pod hello-5d957b94f4-kt6hh -o jsonpath="{.spec}"
  743  kubectl get pod hello-5d957b94f4-kt6hh -o json | less
  744  kubectl get pod hello-5d957b94f4-kt6hh -o jsonpath="{.spec.status}"
  745  kubectl get pod hello-5d957b94f4-kt6hh -o jsonpath="{.spec.podIP}"
  746  kubectl get pod hello-5d957b94f4-kt6hh -o jsonpath="{.spec.status}"
  747  kubectl get pod hello-5d957b94f4-kt6hh -o json | less
  748  kubectl get pod hello-5d957b94f4-kt6hh -o jsonpath="{.spec.containers}"
  749  kubectl get pod hello-5d957b94f4-kt6hh -o jsonpath="{.spec.containers[0].podIP}"
  750  kubectl get pod hello-5d957b94f4-kt6hh -o jsonpath="{.spec.containers}"
  751  kubectl get pod hello-5d957b94f4-kt6hh -o json | less
  752  kubectl get pod hello-5d957b94f4-kt6hh -o jsonpath="{.status}"
  753  kubectl get pod hello-5d957b94f4-kt6hh -o jsonpath="{.status.podIP}"
  754  kubectl get serviceaccount dashboard
  755  kubens
  756  kubens default
  757  kubectl get serviceaccount dashboard
  758  kubectl get serviceaccount dashboard -o json
  759  kubectl get secrets
  760  kubectl get serviceaccount dashboard -o json
  761  kubectl get serviceaccount dashboard -o jsonpath="{.secrets[0].name}"
  762  kubectl get secret dashboard-token-xsxf7
  763  kubectl get secret dashboard-token-xsxf7 -o json
  764  echo "ZXlKaGJHY2lPaUpTVXpJMU5pSXNJbXRwWkNJNklpSjkuZXlKcGMzTWlPaUpyZFdKbGNtNWxkR1Z6TDNObGNuWnBZMlZoWTJOdmRXNTBJaXdpYTNWaVpYSnVaWFJsY3k1cGJ5OXpaWEoyYVdObFlXTmpiM1Z1ZEM5dVlXMWxjM0JoWTJVaU9pSmtaV1poZFd4MElpd2lhM1ZpWlhKdVpYUmxjeTVwYnk5elpYSjJhV05sWVdOamIzVnVkQzl6WldOeVpYUXVibUZ0WlNJNkltUmhjMmhpYjJGeVpDMTBiMnRsYmkxNGMzaG1OeUlzSW10MVltVnlibVYwWlhNdWFXOHZjMlZ5ZG1salpXRmpZMjkxYm5RdmMyVnlkbWxqWlMxaFkyTnZkVzUwTG01aGJXVWlPaUprWVhOb1ltOWhjbVFpTENKcmRXSmxjbTVsZEdWekxtbHZMM05sY25acFkyVmhZMk52ZFc1MEwzTmxjblpwWTJVdFlXTmpiM1Z1ZEM1MWFXUWlPaUptTWpVNE9UTm1PQzAwTlRZd0xURXhaVGt0WVdVeVpDMWlNakV6WmpNeU9XWXlPREFpTENKemRXSWlPaUp6ZVhOMFpXMDZjMlZ5ZG1salpXRmpZMjkxYm5RNlpHVm1ZWFZzZERwa1lYTm9ZbTloY21RaWZRLm01WkdWc0RDYlFLckQzN1hLbFdfaVlLRlZhdXhQZjY3WHhnbEVHZmJBemFGdFBwR01qVWNjdUUzalRtM2Y5b054eFNxYjREbU9GVkNER1pPajVsUjMyTG9WMmRybXA1Z1ZiRFl0Z2VubFpyZnZ1bDhVVDVxMnluMU5lT2NNU3BXUkVxUnZjUFFmb0V1MnppQVNfTWlIU01tR000WEdxNXhGeU5YdTE0Rk9xa0F3M1dCMkJMTkl0YzZYYWZOZm44N05JTlU4TXRqUGxBcVgwYW9WWUluSWk2dnRITHUwRzM0cjRPZnp2RmgydDZvSzQ5UHIxOU5WazY2X3o1ZE83YTQ2TzNzbHJ4Z0xhT3duazhyeWo3WXY4T2ZHWHcxd1VveXljSFRTOE9sOGxTYXVaaFZnLXBULV96RjZsRUxXUlp4dXNZS2M2QXFsWVA1NHBqTERrQWFIQQ==" | base64 --decode
  765  kubectl delete clusterrolebinding dashboard-admin
  766  kubectl get secret dashboard-token-xsxf7 -o json
  767  x=`kubectl get serviceaccount dashboard -o jsonpath="{.secrets[0].name}"`
  768  kubectl get secret $x -o jsonpath="{.data.token}" | base64 --decode
  769  vim pod-reader.yaml
  770  kubectl apply -f pod-reader.yaml 
  771  kubectl run hello --image ubuntu:18.04 -- bash -c "while true;do echo Hello;sleep 1;done"
  772  vim pod-reader.yaml
  773  kubectl apply -f pod-reader.yaml 
  774  vim pod-reader.yaml
  775  kubectl apply -f pod-reader.yaml 

```


 kubectl delete clusterrolebindings dashboard-admin
 
 
 
---

### END


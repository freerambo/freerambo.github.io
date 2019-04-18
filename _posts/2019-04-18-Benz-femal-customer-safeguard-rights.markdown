---
layout:     post
title:      "奔驰女客户维权"
subtitle:   ""
date:       2019-04-18 16:21:00
author:     "Yuanbo"
header-img: "img/home-bg-o.jpg"
catalog:    true

tags:
    - sharing
    
---

## Microservices training05

Everything is a file on linux.

/dev/KVM mount to container. Then container can access devices. 

Eureka -- DS 
feign --> proxy
hystrix --> circuit break 
zuul --> API gateway  
 
 
 
 ```shell
 
 Openshift Uninstallation
 oc cluster down
 docker ps -a
 for i in $(mount | grep openshift | awk '{ print $3}'); do sudo umount "$i"; done
 rm -rf ~/.kube /etc/kubernetes /var/lib/etcd
 
 # Kubernetes Installation
 systemctl enable kubelet && systemctl start kubelet
 systemctl enable docker && systemctl start docker
 
 kubeadm init --pod-network-cidr=10.244.0.0/16
 
 rm -rf $HOME/.kube
 mkdir -p $HOME/.kube
 sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
 sudo chown $(id -u):$(id -g) $HOME/.kube/config
 
 kubectl apply -f \
 https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
 
 # Untaint master
 kubectl taint nodes $(hostname) node-role.kubernetes.io/master:NoSchedule-
 
 # Ensure that the node is ready before you run anything else
 kubectl get nodes
 
 # Istio Installation
 mkdir -p /data/
 cd /data
 curl -L https://git.io/getLatestIstio | sh -
 cd /data/istio-1.0.6/
 kubectl apply -f install/kubernetes/istio-demo.yaml
 export PATH=$PATH:/data/istio-1.0.6/bin
 kubectl get svc -n istio-system
 
 kubectl label namespace default istio-injection=enabled


kubectl port-forward --address 0.0.0.0 service/jaeger-query 6080:16686 -n istio-system

kubectl get svc -n istio-system

## check folder size 
du -hs .

 ```
 
 
#### Reference: 
 
 https://udacity.com/course/scalable-microservices-with-kubernetes--ud615?autoenroll=true
 https://www.oreilly.com/topics/operations
 https://learning.oreilly.com/library/view/cloud-native-devops/9781492040750/
 https://www.oreilly.com/library/view/beyond-the-twelve-factor/9781492042631/

operatorhub.io 

CNCF
KubeCon
DockerCon
Openshift

---

### END


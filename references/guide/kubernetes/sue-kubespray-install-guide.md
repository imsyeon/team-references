# Kubespray 쿠버네티스 설치 가이드

- Ansible의 playbook과 inventory 설정으로 Kubernetes 클러스터 설치
- Kubernetes는 최소 1개의 Mater Node와 1개의 Worker Node 1개를 요구함
- 본 가이드에서는 1 Master, 1 Worker Node를 기준으로 진행함

## 1. ssh key 생성 및 복사

- SSH key 기반의 인증을 사용할 수 있도록 master node를 포함한 모든 node에 key 복사 필요

- Key는 Master Node 생성

```
# Master에서 진행
$ ssh-keygen -t rsa
```

- 생성한 SSH key를 Master와 Worker에 복사

```
# Master에서 실행
$ cat ~/.ssh/id_rsa.pub

# Master, Worker 모두 적용
$ vi ~/.ssh/authorized_keys
```

## 2. kubespray 파일 git clone

> 이후 모든 과정은 Master에서 진행

```
# Master에서 실행
$ git clone https://github.com/kubernetes-sigs/kubespray.git -b v2.17.1
```

## 3. python install

```
# 패키지 업데이트
$ sudo apt update

# python 설치
$ sudo apt install -y python3-pip
```

## 4. kubespray python package install

```
$ cd kubespray/
$ sudo pip3 install -r requirements.txt
```

## 5. inventory 파일 설정

#### 5.1. sample 디렉터리를 복사하여 inventory 디렉터리 생성

```
$ cp -r inventory/sample inventory/mycluster
```

#### 5.2. inventory 파일 수정

-  inventory/mycluster/inventory.ini 파일 수정

```
$ vi inventory/mycluster/inventory.ini 
---

# ## Configure 'ip' variable to bind kubernetes services on a
# ## different ip than the default iface
# ## We should set etcd_member_name for etcd cluster. The node that is not a etcd member do not need to set the value, or can set the empty string value.
[all]
master ansible_host=10.0.0.244  ip=10.0.0.244 etcd_member_name=etcd1
node1 ansible_host=10.0.0.22    ip=10.0.0.22
# node3 ansible_host=95.54.0.14  # ip=10.3.0.3 etcd_member_name=etcd3
# node4 ansible_host=95.54.0.15  # ip=10.3.0.4 etcd_member_name=etcd4
# node5 ansible_host=95.54.0.16  # ip=10.3.0.5 etcd_member_name=etcd5
# node6 ansible_host=95.54.0.17  # ip=10.3.0.6 etcd_member_name=etcd6

# ## configure a bastion host if your nodes are not directly reachable
# [bastion]
# bastion ansible_host=x.x.x.x ansible_user=some_user

[kube_control_plane]
master
# node1
# node3

[etcd]
master
# node1
# node3

[kube_node]
node1
# node3
# node4
# node5
# node6

[calico_rr]

[k8s_cluster:children]
kube_control_plane
kube_node
calico_rr
```

## 6. kubespray 설치

```
$ ansible-playbook -i inventory/mycluster/inventory.ini -become --become-user=root cluster.yml 
```

## 7. kubernetes cluster 설치 확인

```
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```
```
$ kubectl get nodes
NAME     STATUS   ROLES                  AGE     VERSION
master   Ready    control-plane,master   2m37s   v1.21.6
node1    Ready    <none>                 107s    v1.21.6

$ kubectl get pods -n kube-system
NAME                                       READY   STATUS    RESTARTS   AGE
calico-kube-controllers-8575b76f66-c46hn   1/1     Running   0          67s
calico-node-frvjn                          1/1     Running   0          88s
calico-node-lhq2p                          1/1     Running   0          88s
coredns-8474476ff8-h5xz7                   1/1     Running   0          54s
coredns-8474476ff8-rm4fz                   1/1     Running   0          48s
dns-autoscaler-7df78bfcfb-mjjrc            1/1     Running   0          50s
kube-apiserver-master                      1/1     Running   0          2m37s
kube-controller-manager-master             1/1     Running   1          2m37s
kube-proxy-ft72d                           1/1     Running   0          104s
kube-proxy-x52r4                           1/1     Running   0          104s
kube-scheduler-master                      1/1     Running   1          2m39s
nginx-proxy-node1                          1/1     Running   0          110s
nodelocaldns-dnc9n                         1/1     Running   0          49s
nodelocaldns-v55th                         1/1     Running   0          49s
```

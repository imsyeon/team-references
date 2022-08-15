# Kubeadm 쿠버네티스 설치 가이드

- Kubernetes는 최소 1개의 Mater Node와 1개의 Worker Node 1개를 요구함
- 본 가이드에서는 containerd를 CRI 런타임으로 사용
- Master, Worker node로 사용할 컨테이너 모두 진행 필요
- 운영급으로는 사용 불가능

## 1. containerd 설치 전 사전 작업

- containerd를 설치하기 위해 사전 작업 필요
- 아래 명령어를 사용하여 시스템에 Containerd를 설치

```
$ cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
overlay
br_netfilter
EOF

$ sudo modprobe overlay
$ sudo modprobe br_netfilter

# 필요한 sysctl 파라미터를 정의
# 이 파라미터는 재시작 시에도 그대로 유지
$ cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
net.bridge.bridge-nf-call-iptables  = 1
net.ipv4.ip_forward                 = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

# 시스템을 재시작하지 않고 sysctl 파라미터를 반영하기 위한 작업
$ sudo sysctl --system
```

## 2. containerd 설치

- containerd는 Docker를 설치 시 함께 설치
- containerd 설치하는 과정은 Docker를 설치하는 과정에 포함됨

```
# HTTPS를 활용해 저장소에 접근하기 위해 패키지 설치 진행
$ sudo apt-get update
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

# Docker의 공식 GPG 키를 시스템에 추가합니다.
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Docker stable 버전으로 설치
$ echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 새로운 저장소 추가로 업데이트 진행
$ sudo apt-get update

# containerd.io 설치
$ sudo apt-get install containerd.io
```

## 3. containerd 설정 작업

- containerd 실행하기 전 containerd 기본 설정 정의
- containerd config default 수행 시 기본적으로 제공하는 설정 값 설정 가능
- tee 명령어를 통해 화면으로도 출력 및 config.toml 파일로도 저장 가능

```
$ sudo mkdir -p /etc/containerd
$ containerd config default | sudo tee /etc/containerd/config.toml
```

## 4. systemd를 cgroup driver로 사용하기

- systemd를 cgroup driver로 사용하기 위해 /etc/containerd/config.toml에 'SystemdCgroup = true' 설정 추가 필요
- 설정 오류 시 containerd 실행 오류 발생
```
$ sudo vi /etc/containerd/config.toml
---

[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  ...
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true
```

## 5. containerd 재시작

```
$ sudo systemctl restart containerd
```

---

# kubeadm로 Kubernetes 클러스터 구축

## 1. Kubernetes 패키지 설치

- 모든 node에 패키지 설치 진행
- kubectl 설치는 **Worker node**에서 제외

```
# Kubernetes apt 저장소를 위한 패키지 설치
$ sudo apt-get update
$ sudo apt-get install -y apt-transport-https ca-certificates curl

# Google Cloud public signing key 다운
$ sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

# Kubernetes apt 저장소 등록
$ echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

# kubelet, kubeadm, kubectl 설치 (Work node에서는 kubectl 제외)
$ sudo apt-get update
$ sudo apt-get install -y kubelet kubeadm kubectl

# kubelet, kubeadm, kubectl이 자동으로 업그레이드 방지를 위한 버전을 고정
$ sudo apt-mark hold kubelet kubeadm kubectl
```

## 2. kubeadm을 이용한 클러스터 구축

### 2.1. Master node 초기화

- Master node로 사용할 컨테이너에서 진행
- Master node의 private ip 입력

```
$ ifconfig
$ sudo kubeadm init \
> --apiserver-cert-extra-sans=10.0.0.210 \
> --control-plane-endpoint=10.0.0.210
```

- 정상 종료 시 아래와 같은 결과 확인
- 가장 하단의 kubeadm join으로 시작하는 명령어는 추후 Worker node를 추가하기 위해 사용하므로 따로 기록 필요

```

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of control-plane nodes by copying certificate authorities
and service account keys on each node and then running the following as root:

  kubeadm join 10.0.0.210:6443 --token 4zteih.7z5ks1uw1n1mh8qy \
	--discovery-token-ca-cert-hash sha256:06a014fbaa594d57c3d63b5352513fe1cfaf3dfd74ab543daaf60451fad5867d \
	--control-plane 

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.0.0.210:6443 --token 4zteih.7z5ks1uw1n1mh8qy \
	--discovery-token-ca-cert-hash sha256:06a014fbaa594d57c3d63b5352513fe1cfaf3dfd74ab543daaf60451fad5867d 
```
## 3. kubectl 설정하기

- kubectl 명령어를 루트 사용자가 아닌 사용자가 사용하기 위한 작업

```
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## 4. Pod 네트워크 애드온 설치

- calico 사용

```
$ curl https://docs.projectcalico.org/manifests/calico.yaml -O
$ kubectl apply -f calico.yaml
```

## 5. Worker node 추가

- Worker node로 사용할 컨테이너에서 실행
- Master node에서 2.1. 과정 실행 시 나온 kubeadm join 이하 명령어를 실행

```
$ sudo kubeadm join 10.0.0.210:6443 --token 8qag87.2qdv36jfa1dx9ea3 --discovery-token-ca-cert-hash sha256:06a014fbaa59
94d57c3d63b5352513fe1cfaf3dfd74ab543daaf60451fad5867d
```

> 5번 과정 실행 시 To see the stack trace of this error execute with --v=5 or higher 와 같은 오류 발생 시 실행 시
> 
> Master node에서 실행   
> $ kubeadm token create --print-join-command
>
> 참고 블로그   
> https://gurumee92.tistory.com/262?category=957852   
> https://fusiondeveloper.tistory.com/65


## 6. 설치 확인

```
$ kubectl get nodes

NAME            STATUS     ROLES                  AGE     VERSION
ip-10-0-0-210   Ready      control-plane,master   9m28s   v1.23.1
ip-10-0-0-59    Ready   <none>                 5s      v1.23.1
```

## 7. 테스트 deployment

### nginx deployment 생성

#### 1. YAML 파일을 기반으로 deployment 생성

```
$ kubectl apply -f https://k8s.io/examples/application/deployment.yaml
```
- application/deployment.yaml 파일 내부 정보

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80

```
### 2. deployment 정보

```
$ kubectl describe deployment nginx-deployment
```

### 3. deployment에 의해 생성된 Pod 확인

```
$ kubectl get pods -l app=nginx

NAME                               READY   STATUS              RESTARTS   AGE
nginx-deployment-9456bbbf9-4qqgz   1/1     Running             0          10m
nginx-deployment-9456bbbf9-ktzjq   1/1     Running             0          10m
nginx-deployment-ff6655784-rmc8m   0/1     ContainerCreating   0          8s
```
### 4. deployment 업데이트하기

- application/deployment-update.yaml 
  + 이 update.yml 파일은 nginx 1.16.1을 사용하도록 deployment 업데이트 진행

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.16.1 # Update the version of nginx from 1.14.2 to 1.16.1
        ports:
        - containerPort: 80
```

- 새 YAML 파일을 적용

```
$  kubectl apply -f https://k8s.io/examples/application/deployment-update.yaml
```

- deployment가 새 이름으로 파드 생성 후 이전 파드 삭제 확인

```
$ kubectl get pods -l app=nginx

NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-ff6655784-jc4lb   1/1     Running   0          8m57s
nginx-deployment-ff6655784-rmc8m   1/1     Running   0          9m7s
```
> 참고   
> https://kubernetes.io/ko/docs/tasks/run-application/run-stateless-application-deployment/

## 8. Kubernetes에 애플리케이션 배포

- demo 이미지 다운로드
```
$ kubectl create deployment demo --image=springguides/demo --dry-run -o=yaml > deployment.yaml
$ echo --- >> deployment.yaml
$ kubectl create service clusterip demo --tcp=8080:8080 --dry-run -o=yaml >> deployment.yaml
```

- 애플리케이션 배포

```
$ kubectl apply -f deployment.yaml
deployment.apps/demo created
service/demo created
```

- 배포 확인

```
$ kubectl get all
NAME                                   READY   STATUS    RESTARTS   AGE
pod/demo-86795d784d-mtd58              1/1     Running   0          13m

NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
service/demo         ClusterIP   10.111.141.142   <none>        8080/TCP   13m
service/kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP    66m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/demo               1/1     1            1           13m

NAME                                         DESIRED   CURRENT   READY   AGE
replicaset.apps/demo-86795d784d              1         1         1       13m
```
- Kubernetes에서 서비스로 노출한 애플리케이션에 연결하기
- SSH 터미널을 만들어서 확인

```
$ kubectl port-forward svc/demo 8080:8080
```
- 다른 창에서 실행 확인

```
$ curl localhost:8080/actuator/health
{"status":"UP"}
```

> 참고   
> https://spring.io/guides/gs/spring-boot-kubernetes/
---

# kubernetes CLI

## kubeadm 삭제

```
$ sudo kubeadm reset

[reset] WARNING: Changes made to this host by 'kubeadm init' or 'kubeadm join' will be reverted.
[reset] Are you sure you want to proceed? [y/N]: y
[preflight] Running pre-flight checks
W0120 09:06:43.440083    8477 removeetcdmember.go:80] [reset] No kubeadm config, using etcd pod spec to get data directory
[reset] No etcd config found. Assuming external etcd
[reset] Please, manually reset etcd to prevent further issues
[reset] Stopping the kubelet service
[reset] Unmounting mounted directories in "/var/lib/kubelet"
...
```

## 전체 Pod 상태 확인

```
$ kubectl get all -A

NAMESPACE     NAME                                           READY   STATUS            RESTARTS   AGE
kube-system   pod/calico-kube-controllers-85b5b5888d-8vfrk   1/1     Running           0          70s
kube-system   pod/calico-node-6sfng                          1/1     Running           0          70s
kube-system   pod/calico-node-fn5wc                          0/1     PodInitializing   0          70s
kube-system   pod/coredns-64897985d-plt9f                    1/1     Running           0          46m
kube-system   pod/coredns-64897985d-r4vbc                    1/1     Running           0          46m
kube-system   pod/etcd-ip-10-0-0-147                         1/1     Running           4          46m
kube-system   pod/kube-apiserver-ip-10-0-0-147               1/1     Running           4          46m
kube-system   pod/kube-controller-manager-ip-10-0-0-147      1/1     Running           4          46m
kube-system   pod/kube-proxy-rs6qz                           1/1     Running           0          46m
kube-system   pod/kube-proxy-vjkx9                           1/1     Running           1          45m
kube-system   pod/kube-scheduler-ip-10-0-0-147               1/1     Running           4          46m

NAMESPACE     NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                  AGE
default       service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP                  47m
kube-system   service/kube-dns     ClusterIP   10.96.0.10   <none>        53/UDP,53/TCP,9153/TCP   47m

NAMESPACE     NAME                         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
kube-system   daemonset.apps/calico-node   2         2         1       2            1           kubernetes.io/os=linux   70s
kube-system   daemonset.apps/kube-proxy    2         2         2       2            2           kubernetes.io/os=linux   47m

NAMESPACE     NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
kube-system   deployment.apps/calico-kube-controllers   1/1     1            1           70s
kube-system   deployment.apps/coredns                   2/2     2            2           47m

NAMESPACE     NAME                                                 DESIRED   CURRENT   READY   AGE
kube-system   replicaset.apps/calico-kube-controllers-85b5b5888d   1         1         1       70s
kube-system   replicaset.apps/coredns-64897985d                    2         2         2       46m
```
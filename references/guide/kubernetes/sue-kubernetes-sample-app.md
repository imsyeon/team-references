# spring music sample app 배포 가이드

## 1. deployment 생성

- spring-music-sample.yaml 파일 생성

```
$ vi spring-music-sample.yaml

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: spring-music
  namespace: default
spec:
  selector:
    matchLabels:
      app: spring-music
  replicas: 1
  template:
    metadata:
      labels:
        app: spring-music
    spec:
      containers:
      - name: spring-music
        image: paastaccc/spring-music-sample:0.1
        imagePullPolicy: Always
        resources:
          requests:
            cpu: 500m
            memory: 200Mi
        ports:
        - containerPort: 8080
```

- spring music pod 배포

```
$ kubectl apply -f spring-music-sample.yaml
deployment.apps/spring-music created
```

- 배포 확인

```
$ kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
spring-music-677c446bfc-8kfpc       1/1     Running   0          3h32m
```

## 2. Service 생성

- 쿠버네티스는 기본적으로 내부에서 실행한 컨테이너는 외부 접속이 불가능
- 애플리케이션 간 통신이 필요한 경우 Service로 오픈하여 외부에 노출하거나 다른 애플리케이션 간 통신에 활용
- spring-music-svc.yaml 파일 생성

```
$ vi spring-music-svc.yml

---
apiVersion: v1
kind: Service
metadata:
  name: spring-music-service
spec:
  ports:
  ports:
    - port: 8083
      protocol: TCP
      targetPort: 8080
  selector:
    app: spring-music
  type: NodePort
```

## 3. Service 배포

```
$ kubectl apply -f spring-music-svc.yml
service/spring-music-service created
```

- 배포 확인

```
$ kubectl get service
NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
spring-music-service   NodePort    10.233.13.223   <none>        8083:31441/TCP   11s
```

## 4. 접속 확인

- kubectl get service 명령어 실행 시 spring-music-service port 확인

```
$ kubectl get service
NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
spring-music-service   NodePort    10.233.13.223   <none>        8083:31441/TCP   11s
```

- {Node IP}:{port}로 접속
    + {Node IP} : Master나 Worker node의 public IP
    + {port} : 배포한 service의 port

- http://3.35.21.7:31441/ 로 접속 후 정상 작동 확인

## 5. 서비스 삭제

```
$ kubectl delete services spring-music-service
```

---

> ### (참고) Service yaml을 생성하지 않고 명령어를 통한 외부 노출
> - NodePort를 통한 외부 통신
>
>   ```
>   $ kubectl expose deployment/spring-music --type="NodePort" --port 8081 --target-port=8080 --protocol="TCP"
>   
>   service/spring-music exposed
>   ````
>   ```
>   # service 생성 확인
>   $ kubectl get services
>   # service 상세 정보 확인
>   $ kubectl describe deployment/spring-music
>   ````
> - 4번 과정과 동일하게 접속 확인

---

# nginx sample app 배포 가이드

## 1. deployment 생성

- nginx-deployment.yaml 생성

```
$ vi nginx-deployment.yaml

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
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
## 2. pod 배포

```
$ kubectl apply -f nginx-deployment.yaml 
deployment.apps/nginx-deployment created
```

## 3. 외부 노출

```
$ kubectl expose deployment/nginx-deployment --type="NodePort" --port 8080 --target-port=80 --protocol="TCP"
```

## 4. service 확인 

```
$ kubectl get service
NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
nginx-deployment       NodePort    10.233.12.91    <none>        8080:30231/TCP   4h18m

$ kubectl describe services/nginx-deployment
Name:                     nginx-deployment
Namespace:                default
Labels:                   app=nginx
Annotations:              <none>
Selector:                 app=nginx
Type:                     NodePort
IP Family Policy:         SingleStack
IP Families:              IPv4
IP:                       10.233.12.91
IPs:                      10.233.12.91
Port:                     <unset>  8080/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  30231/TCP
Endpoints:                10.233.90.3:80,10.233.90.4:80,10.233.90.5:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
```

## 접속 확인

- {Node IP}:{port}로 접속
    + {Node IP} : Master나 Worker node의 public IP
    + {port} : 배포한 service의 port

- http://3.35.21.7:30231/ 로 접속 후 nginx ui 확인

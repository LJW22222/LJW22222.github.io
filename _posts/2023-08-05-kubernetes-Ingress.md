---
title: Kuberntes[Nginx]
date: 2023-08-05 19:46:00 +0800
categories: [CT, Kubernetes]
tags: [CT, Kubernetes, Nginx]
---
# Nginx Ingress Controller
Nginx Ingress Controller란 Nginx를 사용하여 Ingress기능을 이용하는데 필요한 Ingress Controller를 생성하는 방법입니다.  

여기서 Ingress, Ingress Controller가 등장하는데 이를 좀 더 알아보겠습니다.

## Ingress
예를 들어서 클라이언트가 서비스 1,2,3가 각각 80, 81, 82 포트 번호를 가지고 있다고 가정을 해보겠습니다.  

## Ingress 적용 전
 ![Kuberntes-Ingress-No](/assets/img/kubernetes/kuberntes-ingress-no.png){: width="700" height="400" }  
 Ingress적용 전 사진을 보면 서비스의 포트 번호들이 전부 그대로 외부로 노출되어 있는 모습을 볼 수 있습니다.  

 또한 서비스의 포트 번호가 각각 다르다는 불편함도 존재합니다.  

## Ingress 적용 후  
 ![Kuberntes-Ingress-Yes](/assets/img/kubernetes/kuberntes-ingress-yes.png){: width="700" height="400" }  
 하지만 Ingress를 적용하면, 클라이언트와 클러스터 사이에 프록시 서버가 생기게 됩니다.  

 프록시 서버는 클라이언트와 서비스 중간에 두는형태 입니다.  
 
 그러면 클라이언트는 프록시 서버에 요청을 보내고, 프록시 서버는 경로를 기준으로 서비스를 구분하여 클라이언트는 포트 구분 없이 서비스에 접근이 가능해 집니다.  

 즉, 외부에서 들어온 HTTP, HTTPS를 Cluster 내부의 Service로 라우트 시켜주는 것과 동일합니다.

 이런 프록시 서버를 쿠버네티스 클러스터 내부에 존재하는데, 이를 Ingress라고 부릅니다. 
## Ingress Controller
Ingress를 동작시킬 구현체가 바로 Ingress Controller입니다.  

즉, Ingress를 관리하고 규칙을 정해주는 것이라고 생각하시면 됩니다.  

## Nginx Ingress Controller 적용법
먼저 helm을 설치해줍니다.

helm은 kubernetes의 패키지 관리를 도와주는 도구라고 생각하면 됩니다.

Node.js에는 npm, Python에는 pip가 있듯이 helm도 이와 같은 개념입니다.  
### helm 설치
```
sudo snap install helm --classic
```
helm설치가 끝났으면, 이를 사용하여 Nginx ingress controller를 설치합니다.

### Nginx Ingress Controller
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm install nginx-ingress ingress-nginx/ingress-nginx
```

### SSL
Ingress리소스를 작성하기 전에 SSL을 사용하고 있다면, SSL인증서를 셋팅하는 방법에 대해 알아보겠습니다.

먼저 SSL인증서를 받아야 하는데, 먼저 도메인을 구입한 다음(가비아를 사용했습니다.), Cloudflare에서 구매했던 도메인을 등록합니다.  

여기서 도메인을 등록할때, 가비아에서 설정을 따로 해줘야 합니다.  
설정 방법은 Cloudflare에 자세히 나와 있습니다. 

Cloudflare에 등록을 성공했다면  
- SSL/TLS -> 원본서버   

탭에 들어가서 인증서를 생성합니다. (.pem방식)
여기서 주의할 점은 certificate, private전부 안전하게 보관해야합니다.

그런다음 이 두개의 키들을 라즈베리파이4 서버안에 옮겨 놓습니다.  

그럼다음 아래의 명령어를 실행해주면 SSL secret key가 생성이 됩니다.
```
kubectl create secret tls my-tls-secret --key /path/to/private.key --cert /path/to/certificate.crt
```

### Ingress 리소스 작성 [ yaml 파일 ]
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ailymit-store-ingress
  annotations:
   # kubernetes.io/ingress.class: nginx 
   # nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName : nginx
  tls:
    - hosts:
      - [ domain 이름 ]
      secretName: [ ssl 인증서 저장한 이름 ]
  rules:
    - host:  [ domain 이름 ]
      http:
        paths:
        - path: /test1
          pathType: Prefix
          backend:
            service:
              name: [ service 이름 ]
              port:
                number: [ 실행중인 포트 번호 ]
        - path: /test2
          pathType: Prefix
          backend:
            service:
              name: [ service 이름 ]
              port:
                number: [ 실행중인 포트 번호 ]
```
위의 yaml파일을 제대로 작성하고나서 
```
kubelctl apply -f [ yaml 파일 이름 ]
```
yaml파일을 적용시켜주면 Nginx Ingress Controller가 생성이 되는걸 확인할 수 있습니다.

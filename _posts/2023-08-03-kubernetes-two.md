---
title: Kuberntes[Setting]
date: 2023-08-03 22:21:00 +0800
categories: [CT, Kubernetes]
tags: [CT, Kubernetes]
---
# Kubernetes
이전에 라즈베리파이와 쿠버네티스에 대해 알아보았습니다.  

이번 포스터에서는 클러스터링한 라즈베리파이4에 쿠버네티스를 적용시키는 방법에 대해 알아보겠습니다.

참고로 클러스터링한 라즈베리파이4에 설치해야 하기 때문에 Kubernetes는 일반 버전인 K8s가 아닌 경량화버전인 k3s를 기준으로 설명 드리겠습니다.

## 라즈베리파이4 Kubernets 적용 방법
먼저 라즈베리파이4에 OS를 설치해줍니다.
설치하는 법을 모르신다면 
[라즈베리파이 셋팅방법](/_posts/2023-07-20-raspberry-pi-one.md)을 참고하시면 됩니다.

### 라즈베리파이 업데이트( 최신 버전 유지 )
 - 마스터 노드, 워커 노드 공통 사항 입니다.
 먼저, 라즈베리파이4를 최신버전으로 업데이트를 해줍니다.
 ```linux
 //라즈베리파이4 최신버전으로 업데이트 밎 업그레이드
 sudo apt update
 sudp apt uprade -y
 sudo reboot
 ```
 업데이트를 해주고나서 재부팅을 해줍니다. 
 참고로 워커 노드, 마스터 노드 둘다 공통사항입니다.

### 툴 설치
필요한 툴들을 설치해 줍니다.
```
//필요 툴 설치
sudo apt install -y curl | wget
```

### 비활성화 [ 선택 사항 ]
충돌 가능성이 존재하므로 비활성화를 해줍니다.
```
//선택사항
//SELinux 비활성화 ( 충돌할 가능성이 존재 )
sudo setenforce 0
//AppArmor 비활성화 ( 충돌 가능성 존재 )
sudo aa-complain /usr/sbin/k3s
```
하지만 보안에 관련해서는 취약점이 생길 가능성이 존재하므로 주의해야 합니다.

### 설정 사항
```
//마스터 노드와 워커노드 전부 설정, 하지 않으면 오류 발생
sudo apt install linux-modules-extra-raspi
```
라즈베리파이4에 쿠버네티스를 적용할 때, 위의 설정을 해주지 않으면
```
The connection to the server 127.0.0.1:6443 was refused - did you specify the right host or port?
```
위의 오류가 발생합니다. 

## 마스터 노드 셋팅
### Kubernetes 설치 ( k3s )
```
//1
curl -sfL [https://get.k3s.io](https://get.k3s.io/) | sh –
```
```
//2
curl -sfL https://get.k3s.io | sh -s - --docker --disable=traefik
```
하지만 저는 트래픽 관리와 로드벨런서를 위해 Nginx ingress controller를 사용해야 하고, 보다 쉬운 배포를 위해 Docker를 사용하기로 하여서 2번 명령어를 실행했습니다.

### 상태 확인
```java
sudo systemctl status k3s
```
### 노드 상태 확인

```java
sudo kubectl get nodes -o wide
```
### kubectl 명령줄 도구 구성 
K3s kubernetes클러스와 상호작용하기위한 K3s API 서버와 통신하도록 설정
```java
mkdir ~/.kube
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config && sudo chown $USER ~/.kube/config
sudo chmod 600 ~/.kube/config && export KUBECONFIG=~/.kube/config
```
### 상태 확인 
```java
kubectl get nodes
kubectl cluster-info
kubectl cluster-info
```

## 워커노드 설정
### 마스터 노트에서 토큰 확인
```java
sudo cat /var/lib/rancher/k3s/server/node-token
```
### 워커노드 설치
```java
curl -sfL [http://get.k3s.io](http://get.k3s.io/) | K3S_URL=https://[MASTERNODE IP] K3S_TOKEN=[MASTER TOKEN] sh -
```

위의 명령어들이 문제없이 실행되고, k3s가 잘 설치되었으면 기본적인 Kubernetes설정은 끝이난 겁니다.

다음 포스터에서는 Nginx Ingress Controller 설치 및 셋팅에 대해 알아보겠습니다.
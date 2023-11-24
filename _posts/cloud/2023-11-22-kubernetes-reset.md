---
title: "[Kubernetes] 쿠버네티스 초기화 및 삭제"
category: Cloud
tags: cloud kubernetes
---

2023-11-22 클라우드 프로그래밍 12주차 강의

-----

## 쿠버네티스 클러스터 초기화 및 삭제

```shell
# master worker0 worker1 클러스터 노드에서 다 실행
# 반드시 관리자 권한
sudo kubeadm reset 
sudo apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*
sudo apt-get autoremove
sudo rm -rf ~/.kube
```
        apt-get purge kubeadm kubectl kubelet kubernetes-cni kube*: 쿠버네티스 관련 삭제
        sudo apt-get autoremove: 의존관계에 있던 것들도 삭제
        ~/.kube : 해당 파일도 삭제

        클러스터가 안맺어지면 모두 삭제를 해야 한다. 그래야 클러스터를 맺을 수 있다.

## 쿠버네티스 재설치(root 로그인 후)
```shell
sudo su
#반드시 root 계정 접속후 실행
sudo apt install kubeadm kubelet kubectl kubernetes-cni
```

<b class="text-red">해당 과정을 master, worker0, worker1 에서 모두 수행한다.</b>

## 클러스터 구성

```shell
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```
        네트워크내용은 꼭 쓰지는 않아도됨, 플라넬을 사용하는데에 기본 네트워크임

```shell
# 아래 3가지 내용을 실행하라고 하는데 이걸 반드시 실행해야한다.
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/comfig
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# 그리고 토큰이 나오는데 토큰을 꼭 백업 해 놓아야함 + 밑줄까지 반드시 복사
```
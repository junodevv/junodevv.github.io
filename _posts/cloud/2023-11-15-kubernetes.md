---
title: "[Kubernetes] 쿠버네티스란"
category: Cloud
tags: cloud kubernetes
---

2023-11-15 클라우드 프로그래밍 11주차 강의

-----

목차
- 컨테이너 오케스트레이션
- 쿠버네티스 개요
- 쿠버네티스 아키텍처
- 쿠버네티스 통신

-----

## 컨테이너 오케스트레이션

컨테이너 관리 문제점
- 컨테이너가 적을 때는 수동으로 관리가 가능하지만, 컨테이너가 많아지면 관리 복잡성, 배포 어려움, 리소스 비효율, 장애 대응 지연, 부하 분산 복잡성, 서비스 탐색 문제, 보안 관리 부담 등이 증가하여 오케스트레이션의 필요성이 커짐

컨테이너 오케스트레이션이란
- 컨테이너로 패키징(pod)된 애플리케이션의 배포, 관리, 확장을 자동화하는 기술

특징
- 중앙제어
        마스터(중앙) 컴퓨터에서 명령어나 UI를 통해 모든 컨테이너를 제어함

- 상태관리
        image:"app1"
        replicas: 2 
        => 같은 "app1"패키지(pod)를 2개 생성해서 실행시킨다.

- 스케줄링
        여유있는 서버에 pod를 실행하도록 지정할 수 있다.

- 배포 버전관리
        새버전을 배포하고 불안정하면 이전 버전으로 다시 되돌릴 수 있다.

## 쿠버네티스 개요

쿠버네티스란
- 컨테이너를 쉽고 빠르게 배포/확장하고 관리를 자동화해주는 오픈소스 플랫폼
- 하나의 어플리케이션을 생성하기 위해 하나의 파드(pod)가 필요
        어플리케이션은 1개 이상의 컨테이너로 구성된다.(1개 일 수도 있다.)

쿠버네티스 관리 대상(object)
- 파드(pod)
    - 가장 작은 배포 단위이면서 단일 혹은 다수의 컨테이너를 포함
    - 컨테이너를 개별적으로 하나씩 배포하는 것이 아닌 유사한 역할을 하는 컨테이너를 파드라는 단위로 묶어서 배포
- 서비스
    - 배포한 파드를 외부에서 접근할 수 있게 함
- 네임스페이스
    - 쿠버네티스 클러스터의 논리적인 분리 단위
- 볼륨
    - 컨테이너의 파일을 저장하고 컨테이너 간 파일을 공유할 수 있는 저장소

쿠버네티스 기본 구성
- 기본 구성은 마스터노드 - 워커노드
        이떄, 워커노드는 계속 확장할 수 있다.

쿠버네티스를 사용하는 이유
- 오픈소스
    - 무료로 사용이 가능하고 사용자가 필요에 맞게 커스텀 마이징하고 확장할 수 있는 유연성을 제공
- 강력하고 다양한 기능
    - 다양한 기능을 제공하여 컨테이너화된 애플리케이션의 배포, 확장, 관리, 감시 등을 효율적으로 수행
- 큰 커뮤니티와 생태계
    - 다양한 라이브러리를 이용할 수 있어 개발 및 운영을 더 쉽게 할 수 있음
- Google의 백업
    - 쿠버네티스는 Google이 초기에 개발하고 지원했음. Google의 이러한 백업은 프로젝트의 기술적인 신뢰성을 높여줌

쿠버네티스특징
- 무중단 서비스
    - 서비스 중단 없이 애플리케이션을 업그레이드할 수 있어서 안정적으로 서비스를 제공할 수 잇음
            노드들을 확장할 수 있기 때문
- 클라우드 벤더 종속성 해결
    - 특정 클라우드 벤더에 종속되어 있지 않기 때문에 락인 형산이 없음
    - 다른 오픈 소스 제품 혹은 상용 제품과의 호환성도 뛰어남
- 효율적인 자원 사용
    - 파드가 사용할 수있는 자원(CPU, 메모리 등)을 사전에 지정할 수 있어서 시스템(노드)의 전체 자원을 관리할 수 있음
- 유연한 확장성
    - 파드의 자원 사용률에 따라 파드의 개수를 늘리거나 줄일 수 있음
- 애플리케이션 배포의 가속화
    - 개발자는 인프라 구성에 대한 정보 없이도 컨테이너화된 애플리케이션을 손쉽게 배포할 수 있음
            명령어하나로 쉽게 배포가능


## 쿠버네티스 아키텍처

쿠버네티스 구조

마스터 노드의 구조
- API 서버 : 명령어들을 가지고 있는 라이브러리같은거
- 스케줄러: 스케줄링을 위한 프로그램
- 컨트롤러 매니저
- etcd: DB인데

- 쿠버네티스 클러스터
    - 쿠버네티스의 여러 리소르를 관리하기 위한 집합체
    - 마스터 노드와 워커 노드를 이용해 하나의 쿠버네티스 클러스터를 구성할 수 있음
- 마스터노드
    - 쿠버네티스 클러스터 전체를 관리하는 시스템으로, 컨트롤 플레인(control plane) 이라고도 함
- 워커 노드
    - 마스터 노드에 의해 명령을 받아 파드를 생성하고 서비스 한다고 해서 컴퓨팅 머신(xomputing machine) 이라고도 함
- 컨테이너 런타임
    - 파드를 실행하는 엔진으로, 대표적으로 도커가 있으며, 그 밖에도 컨테이너디, 크라이오 등이 런타임 엔진으로 많이 사용되고 있음
- 영구 스토리지
    - 파드는 기본적으로 휘발성
    - 데이터베이스 같은 중요한 데이터는 파드 외부에 있는 영구 스토리지(워커 노드의 D 드라이브나 데이터 저장 용도의 전용 스토리지 등)에 저장해야 함
    - CSI(Container Storage Interface)로 외부 스토리지를 파드에 연결할 수 있음

쿠버네티스 컴포넌트

- API 서버(kube-apiserver)
    - 쿠버네티스 클러스터의 API를 사용할 수 있게 해주는 프로세스
    - 클러스터로 요청이 들어왔을 때 그 요청이 유효한지 검증
    - API 서버는 파드를 만든다는 사실을 etcd에 알리고 사용자에게 파드가 생성되었음을 알림
            마스터 노드에서 파드생성 명령을 내리면 kubelet을 통해 도커에 접근해 파드를 생성한다.

- etcd
    - 클러스터에 필요한 정보, 파드와 같은 리소스들의 상태 정보가 담겨 있는 곳
    - 키-값(key-value) 형태로 저장
    - 사용자에게 파드가 생성되었음을 알렸지만, 내부적으로는 파드가 생성되기 전

- 스케줄러
    - 워커 노드의 리소스 사용량(CPU나 메모리 등의 사용량)을 참조해 파드를 어떤 노드에 할당해야 할지 결정
    - 파드를 위치시킬 적당한 워커 노드를 확인하고 API서버에게 이 사실을 알림

- kubelet
    - 클러스터의 각 노드에서 실행되는 에이전트로, 파드에서 컨테이너의 동작(생성 및 운영)을 관리
    - API 서버는 파드가 생성될 워커 노드에 있는 kubelet에 파드의 생성 정보를 전달하고 해당 정보를 이용해 파드 생성
    - 파드 생성시 kubelet은 다시 API 서버에 생성된 파드의 정보를 전달하고 API서버는 다시 etcd를 업데이트

- 컨트롤러 매니저
    - kube-controller-manager와 cloud-controller-manager 두 가지 유형
    - kube-controller-manager는 다양한 컴포넌트의 상태를 지속적으로 모니터링하는 동시에 실행 상태를 유지하는 역할
    - cloud-controller-manager는 EKS, AKS 같은 퍼블릭 클라우드에서 제공하는 쿠버네티스와 연동되는 서비스들을 관리
            상태를 모니터링하는것, 파드가 죽었는지 죽었다면 몇개가 죽었는지 모니터링

- 프록시
    - 클러스터의 모든 노드에서 실행되는 네트워크의 프록시
    - 프록시는 노드에 대한 네트워크의 규칙을 관리하기 때문에 클러스터 내부와 외부에 대한 통신을 담당
            파드들이 워크노드0에 있을지 워크노드1에 있을지 모르니까 내부적으로 연결되는 경로가 바뀌는데 그것을 파악해서 연결해줌
            프록시와 CNI를 통해 외부에서 개별 pod로 접속할 수 있도록해줌
            내부적인 구조를 알기 어렵지만 역할을 알면된다.

- 컨테이너 런타임
    - 컨테이너 실행을 담당
    - 쿠버네티스는 다양한 종류의 런타임을 지원하는데, 많이 사용하는 도커부터 컨테이너디, 크라이오 등이 있음

## 쿠버네티스 컨트롤러
        서비스를 하는 주체 = pod

쿠버네티스 컨트롤러 유형

<img width="550" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/9235322b-7cb9-43ae-8e2a-c8aa6dbb815b">
        주로 쓰는 건 디플로이먼트

- 디플로이먼트
    - 쿠버네티스에서 상태가 없는(stateless) 애플리케이션을 배포할 때 사용하는 가장 기본적인 컨트롤러
    - 레플리카셋의 상위 개념이면서 파드를 배포할 때 사용
            stateless를 권장

- 레플리카셋
    - 몇 개의 파드를 유지할지 결정하는 컨트롤러
    - 예를 들어 5개의 파들를 유지하도록 설정했다면 1개의 파드가 삭제되었다 해도 5개의 파드를 유지하기 위해 또 다른 파드 1개를 생성

- 잡
    - 하나 이상의 파드를 지정하고 지정된 수의 파드가 성공적으로 실행되도록 함
    - 예를 들어 노드의 하드웨어 장애나 재부팅 등으로 파드가 비정상적으로 작동하면(혹은 실행되지 않으면) 다른 노드에서 파드를 시작해 서비스가 지속되도록 함

- 크론잡
    - 잡의 일종으로 특정 시간에 특정 파드를 실행시키는 것과 같이 지정한 일정에 따라서 잡을 실행시킬 때 사용
    - 주로 애플리케이션 프로그램, 데이터베이스와 같이 중요한 데이터를 백업하는데 사용
            잡과 크론잡을 직접 사용할 일은 거의 없음 정의를 알아두기

- 데몬셋
    - 특정 노드 또는 모든 노드에 파드를 배포하고 관리함
    - 노드마다 배치되어야하는 성능 수집 및 로그 수집 같은 작업에 사용

## 쿠버네티스 서비스

서비스(Service)
- 동적으로 변하는 파드에 고정된 방법으로 접근하기 위해서 사용하는 것
- 서비스를 사용하면 파드가 클러스터 내의 어디에 떠 있든지 고정된 주소를 이용해 접근할 수 있음
        외부에서 접속하게 해주는 애

서비스 유형
        보통 노드포트나 로드밸런서를 사용

- 클러스터IP(ClusterIP)
    - 클러스터 내부에서 파드들이 통신할 방법을 제공
    - 클러스터 내의 모든 파드가 해당 클러스터 IP주소로 접근할 수 있음
            내부에서만 접속하는 ip를 만들기 때문에 외부에서는 접속할 수 없음

- 노드포트(NodePort)
    - 서비스를 외부로 노출할 때 사용
    - 노드포트로 서비스를 노출하기 위해 워커 노드의 IP와 포트를 이용
    - 예 워커 노드의 IP 192.168.2.3이고 30010이라는 특정포트를 사용한다면 외부에서 192.168.2.3:30010으로 접근할 수 있음
            외부에서 접근하도록 해줌

- 로드밸런서(LoadBalancer)
    - 주로 퍼블릭 클라우드에 존재하는 로드밸런서에 연결하고자 할때 사용
    - 사용자는 로드밸런서의 외부 IP(external IP)를 통해 접근
        외부접근, 여유있는 서비스로 연결시켜줌
        근데 이걸 되게 아마 하려면 구글드라이브, AWS 등의 클라우드 서비스 회사에서 돌려야 됨, 그 회사들이 로드밸런싱을 가능케하는 장비가 있어서

## 쿠버네티스 통신

통신 특징
- 파드가 사용하는 네트워크(가상의 네트워크)와 노드가 사용하는 네트워크(마스터 및 워커 노드가 사용하는  네트워크)는 다름
- 같은 노드에 떠 있는 파드끼리만 통신 가능
- 다른 노드의 파드와 통신하려면 CNI(컨테이너 간의 통신을 위한 네트워크 인터페이스) 플러그인이 필요
        CNI: 

같은 파드에 포함된 컨테이너 간 통신
- 기본적으로 같은 파드 내에 있는 컨테이너 간의 통신은 직접 통신(로컬 호스트 통신)이 가능
- 하나의 파드에 하나의 가상 네트워크가 생성, 그 파드내에 존재하는 컨테이너들은 같은 가상 네트워크(동일한 IP)를 사용
- 같은 파드 내에 존재하는 컨테이너들은 포트번호로 구별

단일 노드에서 파드간 통신
- 단일 노드에 떠 있는 파드들은 같은 대역을 사용하므로 docker0이라는 브리지를 통해 서로 통신

다수의 노드에서 파드간 통신
- 워커 노드1과 워커 노드2에 떠 있는 파드의 IP가 같은 경우(워커 노드에서 파드마다 가상의 네트워크가 생성되기 때문)
- 노드에서 사용하는 물리적인 네트워크 위에 가상의 네트워크를 구성하는 오버레이 네트워크로 문제 해결
- 오버레이 네트워크를 사용하면 클러스터로 묶인 모든 노드에 떠 있는 파드 간의 통신이 가능
- 오버레이 네트워크를 구성하기 위해 클러스터를 구성할 때 CNI 규약을 따르는 플러그인을 함께 설치해야 함

CNI 플러그인 통신
- 대표적으로 플라넬(flannel)이라는 CNI 플러그인을 많이 사용
- 플라넬 같은 오버레이 네트워크에서는 가상 네트워크 인터페이스 및 라우터의 라우팅 규칙 같은 기능을 제공

파드와 서비스 간의 통신
- 서비스도 파드처럼 IP를 가짐
- 파드와 서비스에서 사용하는 IP 대역은 다름
- 서비스에서 사용하는 가상 네트워크는 ifconfig나 라우팅 테이블에서 확인할 수 없음

​​​​외부와 서비스 간 통신

외부와 통신할 수 있는 서비스 유형
- 노드포트(NodePort)
    - 노드 IP(eth)에 포트를 붙여서 외부에 노출시키는 것을 말함
    - 예를 들어 노드의 IP가 192.168.10.2라면  포트를 풑여서 192.168.10.2:30001 같은 형태를 만들어서 외부에 노출
- 로드밸런서(LoadBalancer)
    - AWS, 애저, GCP과 같은 퍼블릭 클라우드에서 지원하는 쿠버네티스 로드밸런서를 사용
    - 로드밸런서와 파드를 연결한 후 해당 로드밸런서의 IP를 이용해 클러스터 외부에서 파드에 접근할 수 있도록 해줌
- 인그레스(Ingress)
    - 클러스터 외부에서 내부로 접근하는 요청들을 어떻게 처리할지에 관한 규칙 모음
    - 인그레스 자체는 클러스터 외부에서 URL로 접근할 수 있도록 로드밸런싱, SSL 인증서 처리 등의 규칙을 정의한 것에 불과
    - 실제로 동작시키는 것은 인그레스 컨트롤러(Ingress-Controller)

*디테일 하게 알 수는 없고 네트워크가 ip가 어떻게 되더라 정도 알면된다.

## reference

[교수님 강의 및 블로그, hull.kr](https://hull.kr/cloud/20)

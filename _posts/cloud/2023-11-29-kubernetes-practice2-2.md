---
title: "[Kubernetes] 쿠버네티스 실습 2 - 기본 사용법(1) II"
category: Cloud
tags: cloud kubernetes
---

2023-11-29 클라우드 프로그래밍 13주차 강의

-----
# 디플로이먼트

## 디플로이먼트와 서비스 사용

디플로이먼트
- 쿠버네티스에서 상태가 없는(statless) 애플리케이션을 배포할 때 사용
- 디플로이먼트는 레플리카셋의 상위 개념으로서 파드의 개수를 유지할 뿐만 아니라 배포 작업을 좀 더 세분화해 관리할 수 있게 해줌

## 디플로이먼트 배포 전략

- 이전 버전의 애플리케이션에서 업데이트가 필요한 경우에 주로 사용되며, 배포 방법에는 롤링, 재생성, 블루/그린, 카나리가 있음
 
- 롤링 업데이트(이게 기본 전략)
    - 새 버전의 애플리케이션(파드)을 배포할 때 새 버전의 애플리케이션은 하나씩 늘려가고 기존 버전의 애플리케이션은 하나씩 줄여나가는 방식으로 쿠버네티스에서 사용하는 표준 배포 방식
    - 상당히 안정적인 배포 방식이지만 업데이트가 느리게 진행된다는 단점이 있음
            
            원래가지고 있던 파드들을 하나씩 v2로 교체해 나간다.
            연결성있는 서비스를 제공하기 위해
            은행같은 경우는 이러한 서비스를 하지 않음

- 재생성 업데이트
    - 모든 이전 버전(V1)의 파드를 모두 한 번에 종료하고 새 버전(V2)의 파드로 일괄적으로 교체하는 방식
    - 빠르게 교체할 수 있지만 새로운 버전의 파드에 문제가 발생하면 대처가 늦어질 수 있다는 단점이 있음

- 블루/그린 업데이트
    - 애플리케이션의 이전 버전(블루, V1 파드)과 새 버전(그린, V2 파드)이 동시에 운영됨
    - 서비스 목적으로 접속할 수 있는 것은 새 버전의 파드만 가능하며 이전 버전의 파드는 테스트 목적으로만 접속 가능
    - 새로운 버전의 파드에 문제가 발생했을 때 빠르게 대응할 수 있으며 안정적으로 배포할 수 있음
    - 많은 파드가 필요하므로 그만큼 많은 자원(CPU, 메모리)이 필요하다는 단점이 있음

- 카나리 업데이트
    - 블루/그린과 비슷하지만 더 진보적인 단계적 접근 방식
    - 카나리는 주로 애플리케이션의 몇몇 새로운 기능을 테스트할 때 사용
    - 두 개의 버전을 모두 배포하지만 새 버전에는 조금씩 트래픽을 증가시켜 새로운 기능들을 테스트함
    - 기능 테스트가 끝나고 새 버전에 문제가 없다고 판단하면 이전 버전은 모두 종료시키고 새 버전으로만 서비스 함

## 디플로이먼트와 서비스 사용하기(실습)

        기존 디플로이먼트 삭제 후 다시 시작
        이미지만 삭제해도 파드들은 종속성이 있기 때문에 그 이미지를 사용한 파드들이 모두 삭제된다.

- 디플로이먼트는 파드 배포에 관한 객체
- 단순히 파드를 배포하는 것 뿐만 아니라 몇 개의 파드를 실행할지 결정하는 것도 디플로이먼트
- 파드에 위치한 서비스를 외부에서 접속하려면 서비스(쿠버네티스의 서비스)를 이용해야 함


디플로이먼트 yaml 파일 생성
```shell
#nano 텍스트 편집기 사용
nano nginx-deploy.yaml
```

- 디플로이먼트 > 파드 > 컨테이너 순서로 이동하면서 상세한 정보들을 정의

yaml 파일 내용

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:               # 디플로이먼트 정보
  name: nginx-deployment    # nginx-deploy라는 이름의 디플로이먼트 생성
  labels:
    app: nginx      # 디플로이먼트의 레이블
spec:       # 디플로이먼트의 spec
  replicas: 2     # 2개의 파드 생성
  selector:     # 디플로이먼트가 관리할 파드를 선택
    matchLabels:
      app: nginx      # 디플로이먼트는 nginx 레이블을 갖는 파드를 선택해 관리
  template:   # template에 정의된 내용에 따라 파드를 생성
    metadata:
      labels:
        app: nginx      # 생성될 파드의 레이블
    spec:     # 컨테이너에 대한 정보, 컨테이너에 대한 spec
      containers:
      - name: nginx   # 컨테이너의 이름
        image: nginx:latest   # 사용할 이미지 (Nginx 웹 서버 이미지)
        ports:
        - containerPort: 80   # 컨테이너가 리스닝하는 포트 (80번 포트로 설정)
```

Metadata:
- name: Deployment의 이름을 "nginx-deployment"로 지정함.
- labels: Deployment에 "app: nginx" 라벨을 할당함. 이 레이블은 파드와 Deployment를 서로 연관시키는 데 사용됨.

Spec (Deployment의 Spec):
- replicas : Deployment가 유지해야 하는 파드의 수를 2개로 지정함.
- selector : Deployment가 관리할 파드를 선택하는 데 사용되는 레이블 셀렉터임. "app: nginx" 레이블을 갖는 파드를 관리함.
- template : Deployment에 의해 생성될 파드의 템플릿을 정의함.
    - metadata : 파드에 "app: nginx" 레이블을 할당함.
    - spec : 파드에서 실행할 컨테이너들에 대한 사양을 정의함.
        - containers: 하나 이상의 컨테이너를 정의할 수 있음. "nginx"라는 이름의 단일 컨테이너를 정의하고 "nginx:latest" image를 사용함. 컨테이너가 80번 포트에서 수신하도록 설정함.

※ Kubernetes의 Deployment에서는 기본적으로 'RollingUpdate' 배포 전략을 사용

만약 배포전략을 바꾼다면 이런식으로 써준다.

```yaml
# Deployment 배포전략 수정
spec:
  strategy:
    type: Recreate #기존파드 삭제 후 새 파드 생성
```

yaml 파일을 사용하여 디플로이먼트 생성

```shell
kubectl apply -f nginx-deploy.yaml
```
- apply: 지정된 리소스 구성을 현재 클러스터에 적용하라는 명령어
- -f: 파일에서 리소스 정의를 읽어들이라는 옵션

배포한 디플로이먼트 목록 확인

```shell
kubectl get deployments
```

파드 목록 확인

```shell
kubectl get pod
```

> ※ 외부에서 접속하게 하기 위해서는 서비스를 생성해야한다.
>
> 서비스는 파드로 분류되지만 논리적으로는 파드로 생각하지 않아도된다.


서비스 yaml 파일 생성

```shell
#nano 텍스트 편집기 사용
nano nginx-svc.yaml
```

서비스  yaml 내용

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-nginx  # 서비스의 이름
  labels:
    run: my-nginx  # 레이블 (run: my-nginx) 설정
spec:
  type: NodePort  # 서비스 유형을 노드포트로 설정, 
  ports:
  - port: 8080  # 서비스가 리스닝하는 포트 (클러스터 내부에서 접근하는 포트)
    targetPort: 80  # 백엔드 파드가 리스닝하는 포트
    protocol: TCP  # 프로토콜 설정 (TCP)
    nodePort: 30000  # 노드포트 번호 지정 (클러스터 외부에서 접근하는 포트)
    name: http  # 포트의 이름
  selector:
    app: nginx  # 파드 선택자 설정 (app: nginx 레이블을 가진 파드와 연결)
```

>- spec: type: -> ①NodePort, ②LoadBalancer(kubectl get services 로 외부 서비스 IP 확인), ③ClusterIP(클러스터 내부에서만 접속, 외부접속 X)
>- port: 8080  # 서비스가 리스닝하는 포트 (클러스터 내부에서 접근하는 포트)
>    - targetPort: 80  # 백엔드 파드가 리스닝하는 포트
>    - nodePort: 30000  # 노드포트 번호 지정 (클러스터 외부에서 접근하는 포트)

*클라우드 그림노트 참고하기

<b class="text-red">※ 기본 서비스 포트 정책</b> 읽어보기
- NodePort 서비스는 클러스터 외부에서 바로 targetPort로 포워딩하는 것이 아니라, nodePort와 port를 거쳐서 포워딩하는 구조를 가짐. 이는 Kubernetes의 네트워킹 모델의 일부로, 클러스터 외부와 내부 간의 통신을 관리하는 방식임.
- NodePort 서비스에서 port 필드를 지정하지 않는 경우, Kubernetes는 기본적으로 targetPort와 동일한 값을 port로 설정함. 즉, 백엔드 파드가 리스닝하는 포트와 클러스터 내부에서 서비스에 접근하기 위한 포트가 같아짐.
- 예를 들어, targetPort가 80으로 설정되어 있고 port를 명시적으로 지정하지 않는다면, Kubernetes는 port도 80으로 설정함. 그러나 port는 클러스터 내부에서만 사용되고, 클러스터 외부에서 접근할 때는 nodePort를 사용함.
- nodePort는 클러스터 외부에서 서비스에 접근할 때 사용되는 포트로, 이 값은 명시적으로 지정하거나 지정하지 않으면 Kubernetes가 자동으로 할당함. 자동 할당된 - nodePort는 일반적으로 30000-32767 범위 내에서 선택됨.
- 따라서, port를 지정하지 않으면 기본적으로 targetPort와 동일한 값이 사용되고, nodePort는 자동 할당되거나 지정된 값에 따라 결정됨.

yaml 파일을 사용하여 서비스 생성

```shell
kubectl apply -f nginx-svc.yaml
```

서비스 상태 정보 확인
```shell
#서비스 목록 확인 명령어
kubectl get svc
```

파드 위치 확인하기
```shell
#파드 상세 정보 확인하는 명령어
kubectl get pods -o wide
```

노드 IP 확인
```shell
#노드 상세 정보 확인하는 명령어
kubectl get nodes -o wide
```

웹 브라우저에서 노드의 IP와 서비스의 포트번호를 이용해서 파드에 접속하기
- http://[worker-IP]:30000/
- [노드IP] : [서비스포트번호]

※ 서비스의 노드벨런싱 정책
- NodePort 서비스를 사용할 때, 클러스터 외부에서 NodePort (여기서는 30000)를 통해 서비스에 접근하면, Kubernetes는 클러스터 내의 어떤 노드로든 요청을 전달할 수 있음. 192.168.47.128:30000 (master 노드의 IP와 NodePort)으로 접근하면, Kubernetes는 이 요청을 my-nginx 서비스로 라우팅함.
- my-nginx 서비스는 app: nginx 레이블을 가진 파드들과 연결되어 있고, 서비스의 spec.selector 필드에 의해 nginx-deployment-7c79c4bf97-9979k와 nginx-deployment-7c79c4bf97-hsxdv 파드들이 선택됨. 따라서, 192.168.47.128:30000으로 요청을 보내면, Kubernetes는 이 요청을 두 개의 Nginx 파드 중 하나로 라우팅함.
- Kubernetes의 로드 밸런싱 메커니즘은 요청을 균등하게 분배하려고 시도하므로, 요청은 두 파드 중 하나로 무작위로 전달됨. 이 과정에서 어떤 워커 노드(worker0 또는 worker1)에 있는 파드로 요청이 전달될지는 예측할 수 없음. 요청이 어느 파드로 라우팅되는지 정확히 알고 싶으면, 각 파드의 로그를 확인하거나, 서비스에 대한 트래픽 로그를 수집하는 것이 필요함.

>selector: app: nginx  -> 파드 선택자 설정 (app: nginx 레이블을 가진 파드와 연결), 이름기반의 선택을 할 수 있게끔 해준다
> 이러한 것을 활용하여 같은 포트번호로 특정 pod로 접속할 수 있게끔 할 수 있음

# 레플리카셋 사용

레플리카셋
- 레플리카셋은 일정한 개수의 동일한 파드가 항상 실행되도록 관리
- 이 기능이 필요한 이유는 서비스의 지속성 때문
- 파드를 사용할 수 없을 때 다른 노드에서 파드를 다시 생성해 사용자에게 중단 없는 서비스를 제공

## 레플리카셋 사용(실습)

레플리카셋 yaml 파일 생성
```shell
#nano 텍스트 편집기 사용
nano replicaset.yaml
```

레플리카셋 yaml 내용
```yaml
apiVersion: apps/v1
kind: ReplicaSet    
metadata:
  name: 3-replicaset    # 레플리카 이름
spec:
  template:
    metadata:
      name: 3-replicaset
      labels:
        app: 3-replicaset
    spec:
      containers:
      - name: 3-replicaset    
        image: nginx
        ports:
        - containerPort: 80
  replicas: 3 # 3개의 파드 생성    
  selector:
    matchLabels:
      app: 3-replicaset
```

apiVersion: 리소스를 정의하는데 사용되는 Kubernetes API의 버전을 나타냄. apps/v1.
kind: 생성하려는 리소스의 종류를 나타냄. ReplicaSet.
metadata: 리소스에 대한 메타데이터를 제공함.
- name: ReplicaSet의 이름을 "3-replicaset"으로 지정함.
spec (첫 번째 spec): ReplicaSet의 사양을 정의함.
- replicas: 유지하려는 파드의 수를 3으로 지정함.
- selector: ReplicaSet이 관리할 파드를 선택하는 데 사용되는 레이블 셀렉터임.
    - matchLabels: ReplicaSet이 관리할 파드는 "app: 3-replicaset" 레이블을 가져야 함.
- template: ReplicaSet에 의해 생성될 파드의 템플릿을 정의함.
    - metadata (템플릿의 metadata): 파드에 대한 메타데이터를 제공함.
        - labels: 생성될 파드에 "app: 3-replicaset" 레이블을 할당함.
    - spec (두 번째 spec): 파드의 사양을 정의함.
        - containers: 실행할 컨테이너에 대한 사양을 정의함.
            - name: 컨테이너의 이름을 "3-replicaset"으로 지정함.
            - image: 사용할 컨테이너 이미지로 "nginx"를 지정함.
            - ports: 컨테이너가 리스닝할 포트를 지정함.
                - containerPort:  80번이 컨테이너가 리스닝 포트.

yaml 파일을 사용하여 레플리카셋 생성
```shell
kubectl apply -f replicaset.yaml
```

레플리카셋과 파드 정보 확인
```shell
#레플리카셋과 파드 목록 확인 명령어
kubectl get replicaset,pods
```

파드 하나 삭제
```shell
#파드 삭제 명령어
kubectl delete pod 3-replicaset-jmv9f
```

레플리카셋과 파드 목록 재확인
```shell
#레플리카셋과 파드 목록 확인 명령어
kubectl get replicaset,pods
```

- 파드가 삭제되어도 지정된 숫자 만큼 파드의 개수를 유지한다

레플리카셋 파드 개수 늘리기
```shell
#레플리카를 늘리는 명령어
kubectl scale replicaset/3-replicaset --replicas=5
```

레플리카셋과 파드 정보 확인
```shell
#레플리카셋과 파드 목록 확인 명령어
kubectl get replicaset,pods
```

파드는 유지하되, 레플리카셋만 삭제
```shell
#파드는 유지하고 레플리카셋만 삭제해주는 명령어
kubectl delete -f replicaset.yaml --cascade=orphan
```

        replicaset.yaml 파일에서 레플리카셋의 옵션을 날려준다.
        * 파일 기반이아니라 그냥 없앨 수도 있음

> - 디플로이먼트는 관리자기 때문에 종속성을 가지고 디플로이먼트를 삭제하면 파드전체가 삭제된다.
> - 레플리카셋은 정책 같은 거라서 종속성을 가지지 않음 그래서 레플리카셋을 지워도 파드들이 지워지지 않고 남아있음
>    - 하지만 기존처럼 파드를 지워도 더이상 재생성되지는 않음 

# 끝

## reference
[교수님 강의 및 블로그, hull.kr](https://hull.kr/cloud/24)
---
title: "[kubernetes] 쿠버네티스 실습 3 - 기본 사용법(2)"
category: Cloud
tags: cloud kubernetes
---

2023-12-06 클라우드 프로그래밍 14주차 강의

-----

> ### 목차
> 1. 데몬셋
> 2. 크론잡
> 3. 컨피그맵
> 4. 시크릿
> 5. 볼륨

-----

# 1. 데몬셋

## 데몬셋(DemonSet)
- 모든 노드 파드 실행시 사용
- **모니터링 용도**로 사용

        ✓ 데몬셋이 어떤 식으로 동작하고 어떤 용도로 사용되는지 알기
        각 노드마다 고정해서 사용하는 파드

## 데몬셋(실습)
### 데몬셋 yaml 파일 생성

```shell
vi daemonsets.yaml
```

        ✓ sudo su -> 루트의 home `~`(/root) 에서 작업해야한다.

### 데몬셋 yaml 파일 생성
```yaml
apiVersion: apps/v1  # Kubernetes API 버전 지정
kind: DaemonSet       # 데몬셋 정의
metadata:  #
  name: prometheus-daemonset  # 데몬셋 이름 지정
spec:  # 데몬셋 스펙 정의
  selector:  # 파드 선택하는 방법 지정
    matchLabels:
      tier: monitoring   
      name: prometheus-exporter
  template:  # 파드 템플릿 정의 데몬셋이 생성하는 파드는 템플릿을 기반으로 생성
    metadata:  # 파드 메타데이터 정의
      labels:  # 파드 라벨 정의
        tier: monitoring
        name: prometheus-exporter
    spec:  # 파드 스펙 정의
      containers:  # 파드 포함 컨테이너 정의
      - name: prometheus  # 컨테이너 이름 지정
        image: prom/node-exporter  #  도커 이미지 지정
        ports:  # 컨테이너 포트 정의
        - containerPort: 80  #  컨테이너 사용 포트
```
        tier: monitoring   
        name: prometheus-exporter
        -> 이것들이 크게 의미있는것이 아니고 그냥 라벨링 해주고 이름 지어준 것 뿐이다.

### 데몬셋 생성 및 정보 확인
```shell
#데몬셋 생성
kubectl apply -f daemonsets.yaml
#데몬셋 상세 정보 확인
kubectl describe daemonset/prometheus-daemonset
```

### 데몬셋 상세 정보 확인(실행 화면)
<img width="700" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/53c225c0-7a37-4f07-a1c8-71104451edc6">

> 상세정보 내용
>- Name: prometheus-daemonset: DaemonSet의 이름
- Selector: name=prometheus-exporter,tier=monitoring: DaemonSet이 어떤 노드에 파드를 배포할지 선택하는 데 사용되는 레이블 셀렉터
- Desired Number of Nodes Scheduled: 예상된 노드 수
- Current Number of Nodes Scheduled: 현재 노드 수
- Number of Nodes Scheduled with Up-to-date Pods: 최신 상태의 파드를 가진 노드 수
- Number of Nodes Scheduled with Available Pods: 사용 가능한 파드를 가진 노드 수
- Number of Nodes Misscheduled: 잘못 배치된 노드의 수
- Pods Status: 파드의 상태(실행 중인 파드 수 / 대기 중인 파드 수 / 성공한 파드 수 / 실패한 파드 수)
- Pod Template: 사용되는 파드 템플릿의 정보
- Containers: 파드에 포함된 컨테이너의 정보
- Events: DaemonSet과 관련된 이벤트 정보
    - Type: 이벤트의 유형 (Normal, Warning 등)
    - Reason: 이벤트의 발생 이유
    - Age: 이벤트가 발생한 시간 경과
    - From: 이벤트가 발생한 소스
    - Message: 이벤트에 대한 메시지

### 데몬셋 파드 리스트 확인 및 데몬셋 삭제

```shell
#데몬셋 확인
kubectl get daemonset
#파드 리스트 확인
kubectl get pods -o wide
```

### 데몬셋 파드 리스트 확인(실행 화면)
<img width="700" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/3a5373c1-5997-44dd-9bc3-0c81766266a5">

> 데몬셋 목록 조회 결과(각 열 의미)
- NAME: DaemonSet의 이름
- DESIRED: DaemonSet이 관리하는 파드의 원하는 수
- CURRENT: 현재 DaemonSet이 관리하는 파드의 실제 수
- READY: 현재 실행 중이고 모든 컨테이너가 정상적으로 시작된 파드 수
- UP-TO-DATE: 최근에 업데이트된 컨트롤러 템플릿을 사용하여 생성된 파드의 수
- AVAILABLE: 사용 가능한 파드 수
- AGE: DaemonSet이 생성된 시간으로 표시된 시간 경과

### 데몬셋 삭제
```shell
#데몬셋 삭제 kubectl delete daemonset [데몬셋 이름]
kubectl delete daemonset prometheus-daemonset
```

> ✓ 만약 모든 파드를 삭제하고 싶을떄
pods 삭제 -all 하면 deployment로 생성할 때 "최소 몇개는 살려둬라" 했던 pod들이 살아있게 된다 따라서 deployment도 삭제 -all 해줘야 한다.

---

# 2. 크론잡
잡(job)
- 하나 이상 파드가 특정한 파드의 정상적인 상태 유지 및 관리
크론잡(CronJob)
- 주기적으로 액션을 발생시키는 잡
- 애플리케이션, 데이터 백업시 사용

## 크론잡(실습)
### 크론잡 yaml 파일 생성
```shell
vi cronjob.yaml
```

### 크론잡 yaml 파일 내용
```yml
apiVersion: batch/v1
kind: CronJob   # CronJob 정의
metadata:
  name: hello  # CronJob의 이름 지정
spec:
  schedule: "*/1 * * * *"  # CronJob 실행 일정 지정
  jobTemplate:  # Job 템플릿 정의
    spec:
      template:
        spec:
          containers:
          - name: hello  # 컨테이너 이름 지정
            image: busybox  # 도커 이미지 지정
            imagePullPolicy: IfNotPresent  # 이미지 풀 정책 설정(이미지가 없을 때만 풀도록 설정)
            command:  # 컨테이너에서 실행할 명령어 정의
            - /bin/sh  # 셸 실행
            - -c  # 명령어 실행시 사용할 인자
            - date; echo Hello from the Kubernetes cluster  # date 명령어와 메시지 출력 명령어
          restartPolicy: OnFailure  # Job이 실패할 때만 다시 시작되도록 재시작 정책 설정
```

          schedule: "*/1 * * * *" -> "*/1"은 시간(분)

<img width="692" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/4e13fe21-6e74-4b2a-b028-81fa3922fc2d">

### 크론잡 생성 및 목록 확인
```shell
#크론잡 생성
kubectl create -f cronjob.yaml
#크론잡 목록 확인
kubectl get cronjob hello
#크론잡 목록 상세정보 확인
kubectl get cronjob -w
```

### 크론잡 목록 확인(실행 화면)
<img width="696" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/e3aae3f8-6993-4ae3-8e0c-3916da413be0">

### 크론잡 목록 상세 정보 확인(실행 화면)
<img width="691" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/be5e1357-2cd2-4216-97d4-0ac0a55a4ad2">

        kubectl get pods -o wide 를 사용해서 확인해보면 completed된 파드들을 확인할 수 있다. 1분에 한개씩 생성되어서 특정행위를 하고 내려가는 것이다.

>CronJob 리소스에 대한 정보(각 열 의미)
- NAME: CronJob의 이름
- SCHEDULE: Cron 표현식에 따라 작업이 예약된 시간
    - */1 * * * *"로 설정 매 분마다 실행 예약
- SUSPEND: 현재 CronJob이 일시 중지되었는지 여부
- ACTIVE: 현재 실행 중인 작업의 수
- LAST SCHEDULE: 가장 최근에 작업이 예약된 시간
- AGE: CronJob이 생성된 이후의 경과 시간

### 파드 변화 확인 및 크론잡 삭제
```shell
#파드 변화 확인
kubectl get pod -w
#크론잡 삭제
kubectl delete cronjob hello
```

#### 파드 변화 확인(실행 화면)
<img width="699" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/2ec4c62b-c7b1-4d53-a02f-e518e969deb5">

#### 크론잡 삭제(실행 확인)
<img width="693" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/97196463-8534-4e86-9581-40725a630ff8">

✓ 데몬셋과 크론잡은 활용할일이 많지 않음, 어떤 식으로 동작하고 용도가 무엇인지 정도 알기

---

# CoreDNS
        
        내부적으로 어떻게 도는지는 알 수 없음
        쿠버네티스가 가지고 있는 도메인에 대해서 IP를 반환해준다.

DNS
- 사람이 이해하는 도메인과 컴퓨터가 이해하는 IP 주소 간 변환 서비스
CoreDNS
- 쿠버네티스 전용 DNS
- kubeadm 설치 시 기본 설치

## CoreDNS(실습)

#### CoreDNS 구성 및 서비스 확인
```shell
#CoreDNS 구성 확인
#두 개의 파드로 구성
kubectl  get po -n kube-system -l k8s-app=kube-dns
#두 개의 파드에 대한 서비스 확인
#kube-dns는 TCP, UDP 모두 53 포트 사용
kubectl get svc -n kube-system -l k8s-app=kube-dns
```

#### CoreDNS 구성 확인(실행 화면)
<img width="691" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/c3d28fdb-55ba-427e-a4e2-bf2c913978ef">

#### 두 개의 파드에 대한 서비스 확인(실행 화면)
<img width="689" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/5e53eca3-63ee-42c3-a69d-e39364b35b3f">

### CoreDNS의 내용을 담고 있는 Corefile 정보 확인
- Corefile은  CoreDNS의 구성 파일
- 어떤 서버가 어떤 포트와 프로토콜을 수신하는지에 대한 내용 기록

```shell
#Corefile 정보 확인
kubectl describe cm -n kube-system coredns
```

#### Corefile 정보 확인(실행 화면)
사진

> Corefile의 출력 내용
- errors: 에러 발생시 stdout에 내용 기록
- health: CoreDNS 상태 확인 부분
- ready: DNS 서비스 준비가 되었다면 200 OK 반환
- kubenetes: CoreDNS가 쿠버네티스의 서비스 및 파드의 IP를 기반으로 DNS 질의에 응답
- prometheus: 지정된 포트(9153)로 프로메테우스의 메트릭 정보 확인
    - 프로메테우스(Prometheus)는 오픈 소스 시스템 모니터링 및 경고 도구
    - 시스템 상태를 모니터링하는 데 사용
    - 메트릭(Metrics)은 시스템의 동작을 측정하고 표현하는 수치 값

### CoreDNS가 어떻게 사용되는지 확인

```shell
#파드 하나 생성후 /erc/resolv.conf 파일 내용 확인
#파드 생성시 kubelet은 /erc/resolv.conf 파일에 CoreDNS를 가리키는 IP 주소를 네임서버로 등록
#resolv.conf 파일에 등록된 네임서버를 이용해 도메인을 IP로 변경 가능
kubectl run -it --rm --image=busybox --restart=Never busybox -- cat /etc/resolv.conf
```

#### 파드 하나 생성후 /erc/resolv.conf 파일 내용 확인(실행 화면)
사진

> resolv.conf에 등록된 결과(의미)
- nameserver: CoreDNS의 IP 주소
- search: DNS에 질의하는 부분으로 도메인 주소 표시
- ndots: 도메인에 포함될 .(점)의 최소 개수(예, www.a.com에서 “.”의 개수)

----

# 3. 컨피그맵

## 컨피그맵(ConfigMap)
- 환경 변수 값을 도커 이미지에 포함시키지 않고 별도로 분리해서 관리하는 방법을 제공
- 기밀이 아닌 데이터를 키-값 쌍으로 저장하는데 사용

        환경변수를 각 파드에 들어가지 않고 밖에서 설정할 수 있다.

## 컨피그맵 생성 명령어
- kubectl create configmap <map-name> <data-source> <arguments>

## 옵션 의미
- map-name: 컨피그맵 이름
- data-source: 컨피그맵을 포함하는 파일 또는 디렉터리의 경로
- arguments: 컨피그맵을 생성하는 방법 지정
    - 파일로 생성: 파일/디렉터리(--frome-file), 환경 파일(--from-env-file)
    - 문자로 생성: 리터럴 값(--from-literal)

## 컨피그맵(실습)

**리터럴 값(--from-literal) 사용 방법**
- kubectl create configmap [컨피그맵 이름] --from-literal=[키]=[값]

### 컨피그맵 JAVA_HOME 환경 변수 정의

```shell
kubectl create configmap my-config --from-literal=JAVA_HOME=/usr/java
```

#### 컨피그맵 JAVA_HOME 환경 변수 정의(실행 화면)
사진넣기

### 컨피크맵 삭제
```shell
kubectl delete configmap my-config
```

### 컨피그맵 변수 추가 정의
```shell
kubectl create configmap my-config --from-literal=JAVA_HOME=/usr/java --from-literal=URL=http://localhost:8000
```

#### 컨피그맵 변수 추가 정의(실행 화면)
사진추가

### 컨피그맵 활용
```shell
vi configmap-test.yaml
```

```yml
apiVersion: apps/v1
kind: Deployment    #유형 = 디플로이먼트
metadata:
  name: java-app    # java-app이라는애들을 관리하겠다.
spec:
  replicas: 1
  selector:
    matchLabels:
      app: java-app
  template:
    metadata:
      labels:
        app: java-app
    spec:
      containers:
      - name: java-container
        image: openjdk:11
        command: ["sh", "-c", "while true; do sleep 3600; done"]
        env:
        - name: JAVA_HOME
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: JAVA_HOME # JAVA_HOME=/usr/java, 위에서 컨피그맵에 정의한 이것을 사용하겠다.
        - name: URL
          valueFrom:
            configMapKeyRef:
              name: my-config 
              key: URL  # URL=http://localhost:8000, 위에서 컨피그맵에 정의한 이것을 사용하겠다.
```

        command: ["sh", "-c", "while true; do sleep 3600; done"] 
        -> 이 옵션으로 java11이 3600초(1분) 동안 돌게끔 한다?
        
        name: my-config
        key: URL
        -> my-config에서 설정한 URL을 사용하겠다.

```shell
kubectl apply -f configmap-test.yaml
kubectl get pods
kubectl exec -it [Pod 이름] -- /bin/bash
echo $JAVA_HOME
```

<img width="686" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/5a322d31-cb14-4226-bb18-22c41ce9559b">

### 파일/디렉터리(--from-file) 사용 방법
- 직접 환경 변수 값을 지정하는 것이 아닌 환경 변수 정의된 파일 이용

### Hello, World! 내용 html 파일 생성
```shell
echo Hello, World! >> configmap_test.html
```

### html 파일 이용하여 컨피그맵 생성
```shell
kubectl create configmap configmap-file --from-file configmap_test.html
```

#### html 파일 이용하여 컨피그맵 생성(실행 화면)
사진넣기

### 컨피그맵 내용 확인
```shell
kubectl describe configmap configmap-file
```

#### 컨피그맵 내용 확인(실행 화면)
사진넣기

### 컨피그맵 파일 활용
```shell
vi configmap-file-test.yaml
```
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 1
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
        image: nginx:latest
        volumeMounts:
        - name: configmap-volume
          mountPath: /usr/share/nginx/html
      volumes:
      - name: configmap-volume
        configMap:
          name: configmap-file  # 이 파일안에있는 
          items:
          - key: configmap_test.html  # 이 html을 
            path: index.html          # 이 index.html로 만들어라
```

### 적용
```shell
kubectl apply -f configmap-file-test.yaml
kubectl get pods -o wide
```
<img width="687" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/9a08f4ef-560c-4378-b75d-598b684af1fd">

```shell
curl 10.244.2.67
```
<img width="457" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/ef0f2b0a-c3c7-498a-a7c4-f3bb2f711e2a">

        congifmap-file안에 적은 configmap_test.html이 index에 적용된 것을 확인 할 수 있다.

----

# 4. 시크릿

## 시크릿
- 비밀번호와 같은 민감한 정보들을 저장하는 용도
- 컨테이너에 저장하지 않고 별도 보관 후 파드 실행 시 시크릿 값 가져와 파드에 제공
        
## 시크릿(실습)
✓ 컨피그맵과 사용법은 거의 동일하다.

### 시크릿 생성
```shell
kubectl create secret generic dbuser --from-literal=username=testuser --from-literal=apssword=1234
```

#### 시크릿 생성(실행 화면)

사진넣기

### 시크릿 활용
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: mycontainer
        image: myimage
        env:
        - name: USERNAME
          valueFrom:
            secretKeyRef:
              name: dbuser
              key: username
        - name: PASSWORD
          valueFrom:
            secretKeyRef:
              name: dbuser
              key: password
```

### 시크릿 삭제
```shell
kubectl delete secret dbuser
```

----

# 5. 볼륨
        - 가장 많이 활용되는 부분

        - 이미지파일을 보통 내부(pod,container) 볼륨에 넣지 않음
        ↳ 왜냐면 생성된 모든 파드들이 동기화되어야하기때문에 하나라도 동기화 되지 않는 오류가 발생하면 그냥 오류가 나버리는 것이기 때문

## 볼륨
- 데이터를 저장하는 저장소
- 볼륨은 파드의 구성 요소 매니페스트로 정의
- 독립적인 쿠버네티스 리소스가 아니므로 자체적으로 생성되거나 삭제 될 수 없음

<img width="695" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/4c77b413-ae2f-4a4c-b4b8-3274bf81dd0f">

## 볼륨 분류
- 파드 내 위치: 파드(컨테이너) 내 데이터 저장 시, 파드 종료 시 데이터도 삭제
    - 임시적으로 데이터를 저장하는 경우, DB에 저장하기전에 임시저장하는 경우
    - 파드가 꺼지면 데이터가 필요없는 경우
    - 휘발성, 영속성X, ex) 교수님의 CPU온도 모니터링 시스템
- 워커 노드 내 위치: 파드가 종료되어도 데이터 유지, 노드 종료시 데이터도 삭제
- 노드 외부 위치: 파드 혹은 노드 종료와 무관하게 데이터 항상 보존
    - 영속성 보장
    - 파일이나 이미지를 저장할 경우 사용

<img width="725" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/f6261a35-f459-467c-b421-6c8c2ea4fe5b">

## 볼륨 유형
- 임시 볼륨: 파드 내 공간 사용, 파드가 삭제(종료)시 데이터 삭제
- 로컬 볼륨: 노드 내 디스크를 저장소로 사용, 노드 종료 시 데이터 삭제
- 외부 볼륨: 노드 외부 저장소를 이용, 파드 및 노드와 무관하게 영구적으로 사용할 수 있음
    - 외부 저장소가 따로 있어야 해서 비용 고려

<img width="528" alt="image" src="https://github.com/junodevv/junodevv.github.io/assets/126752196/d438c0bb-9903-4fcb-b42e-7490bd7186d6">

## emptyDir(실습) 임시 볼륨 마운트 후 데이터 읽고 쓰기

### emptydir yaml 파일 생성 및 실행
```shell
#emptydir yaml 파일 생성
nano emptydir.yaml
#emptydir yaml 파일 실행
kubectl apply -f emptydir.yaml
```

### emptydir yaml 파일 내용
```yml
apiVersion: v1
kind: Pod
metadata:
  name: emptydata   # Pod의 이름을 'emptydata'로 지정
spec:
  containers:
  - name: nginx
    image: nginx   # Nginx 이미지를 사용하는 컨테이너를 정의
    volumeMounts:
    - name: shared-storage
      mountPath: /data/shared   # 'shared-storage' 볼륨을 '/data/shared' 경로에 마운트
  volumes:
  - name : shared-storage
    emptyDir: {}   # 'shared-storage' 볼륨을 EmptyDir 타입으로 정의
```

### emptydata 파드에 접속 및 디렉토리 이동
```shell
#emptydata 파드에 접속
kubectl exec -it emptydata -- /bin/bash
#/data/shared 디렉토리로 이동
cd /data/shared
```

#### emptydata 파드에 접속 및 디렉토리 이동(실행 화면)
사진넣기

### text 파일 생성 및 확인
```shell
#text 파일 생성
echo "hello" > test.txt
#생성 파일 확인
ls –al
#파일 내용 확인
date,cat test.txt
```

#### text 파일 생성 및 확인(실행 화면)
사진넣기

## hostPath(실습) 로컬 볼륨 마운트 후 데이터 생성
- hostPath 사용하는 다수 파드끼리 데이터 공유 가능
- 파드 삭제시 hostaPath내 파일은 삭제되지 않고 같은 hostPath 사용하는 다른 파드는 해당 볼륨 접근 가능

### hostpath yaml 파일 생성
```shell
vi hostpath.yaml
```

### hostpath yaml 파일 내용
```yml
apiVersion: v1
kind: Pod
metadata:
  name: hostpath   # Pod의 이름을 'hostpath'로 지정
spec:
  containers:
  - name: nginx
    image: nginx   # Nginx 이미지를 사용하는 컨테이너를 정의
    volumeMounts:
    - name: localpath
      mountPath: /data/shared   # 'localpath' 볼륨을 '/data/shared' 경로에 마운트
  volumes:
  - name: localpath
    hostPath:
      path: /tmp   # 호스트 경로를 '/tmp'로 설정
      type: Directory   # 호스트 경로의 타입을 디렉토리로 지정
```
        - master의 /data/shared를 worker노드의 /tmp와 연결
        - 어떤 worker노드가 될지는 알수 없음 wide를 통해 확인가능

### hostpath yaml 파일 실행 및 hostpath 파드 접속
```shell
#hostpath yaml 파일 실행
kubectl apply -f hostpath.yaml
#hostpath 파드 접속
kubectl exec -it hostpath -- /bin/bash
#/data/shared 디렉토리 이동
cd /data/shared
```

#### hostpath yaml 파일 실행(실행 화면)
사진넣기

#### hostpath 파드 접속 및 디렉토리 이동(실행 화면)
사진넣기

### text 파일 생성 및 생성 파일 확인
```shell
#text 파일 생성
echo "hello" > test.txt
#생성 파일 확인
ls –al
```

### text 파일 생성 및 생성 파일 확인(실행 화면)
사진넣기

## 외부볼륨

### NFS (Network File System)**
- NFS는 네트워크를 통해 파일 시스템을 공유하는 방식임.
- 쿠버네티스는 NFS 서버에 볼륨을 마운트하여 사용할 수 있음.
- PersistentVolume과 PersistentVolumeClaim을 사용해 NFS 서버의 경로를 쿠버네티스 클러스터에 연결함.
- 다중 읽기/쓰기 작업이 가능하지만, 성능이 중요한 애플리케이션에는 적합하지 않을 수 있음.
    - 일반적인 어플리케이션은 괜찮으나, 지속적으로 저장삭제를 해야하는 센싱 어플리케이션에서는 적합하지 않을 수 있다.

### CephFS

- CephFS는 Ceph 스토리지 시스템을 사용하는 분산 파일 시스템임.
- 고가용성과 확장성을 제공함.
- 쿠버네티스 **cephfs 볼륨 플러그인**을 통해 CephFS 볼륨을 사용할 수 있음.
- 병렬 접근이 가능하여, 고성능을 요구하는 작업에 적합함.

### GlusterFS

- GlusterFS는 스케일 아웃 방식의 분산 파일 시스템임.
- 여러 서버에 걸쳐 데이터를 저장하며, 확장성이 뛰어남.
- 쿠버네티스 내에서 **glusterfs 볼륨 플러그인**을 사용하여 볼륨을 연결할 수 있음.
- 높은 가용성과 확장성을 제공하지만, 구성이 복잡할 수 있음.

### AWS EBS (Elastic Block Store):

- AWS EBS는 AWS 클라우드 서비스에서 제공하는 블록 스토리지임.
- 쿠버네티스 클러스터가 AWS에 위치한 경우, EBS 볼륨을 쉽게 연결하여 사용할 수 있음.
- awsElasticBlockStore 볼륨 타입을 사용하여 EBS 볼륨을 쿠버네티스에 연결함.
- 높은 내구성과 성능을 제공하지만, AWS 클라우드에 종속적임.

각각의 볼륨 타입은 특정 사용 사례와 요구 사항에 따라 선택되어야 함. 예를 들어, AWS EBS는 AWS 환경에서 운영되는 서비스에 적합하고, CephFS나 GlusterFS는 고가용성과 확장성이 중요한 경우에 사용됨. NFS는 설정이 간단하고 범용적이지만, 고성능을 요구하는 애플리케이션에는 적합하지 않을 수 있음.

## NFS (Network File System) 실습

### NFS 서버 설치(marst)
```shell
#NFS 서버 패키지 설치
sudo apt-get update
sudo apt-get install nfs-kernel-server
#NFS 서버 재시작
sudo systemctl restart nfs-kernel-server
```
### NFS 유틸리티 설치(worker0, worker1)
```shell
sudo apt-get updat
sudo apt-get install --reinstall nfs-common
```
### 공유 디렉토리 생성
```shell
sudo mkdir -p /nfs_share
#디렉토리 권한 설정
sudo chown nobody:nogroup /nfs_share
sudo chmod 777 /nfs_share
```

사진넣기

### NFS 공유설정
```shell
sudo vi /etc/exports
```

```
# /etc/exports 공유 설정
/nfs_share 192.168.47.0/24(rw,sync,no_root_squash,no_subtree_check)
```

사진추가하기

### 변경사항 적용
```shell
sudo systemctl restart nfs-kernel-server
```

### 공유폴더 확인
```shell
showmount -e 192.168.47.128
```

실습사진넣기

### 워크노드에서 마운트 테스트
```shell
#에러 로그 미출력시 성공
sudo mount -t nfs 192.168.47.128:/nfs_share /mnt
#마운트 해제
sudo umount /mnt
```

실습사진넣기

### PersistentVolume 설정
```shell
cd ~
sudo vi nfs-pv.yaml
```
```yml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs-pv
spec:
  capacity:
    storage: 5Gi
  accessModes:
    - ReadWriteMany
  nfs:
    server: 192.168.47.128
    path: "/nfs_share"
```

### 적용
```shell
kubectl apply -f nfs-pv.yaml
```

### PersistentVolumeClaim(PVC) 설정
```shell
sudo nano nfs-pvc.yaml
```
```yml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nfs-pvc
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 5Gi
```

### 적용
```shell
kubectl apply -f nfs-pvc.yaml
```

실습사진추가

### PVC 마운트 pod 생성
```shell
vi nfs-pod.yaml
```
```yml
apiVersion: v1
kind: Pod
metadata:
  name: nfs-pod
spec:
  containers:
  - name: nfs-container
    image: nginx
    volumeMounts:
    - name: nfs-storage
      mountPath: "/usr/share/nginx/html"
  volumes:
  - name: nfs-storage
    persistentVolumeClaim:
      claimName: nfs-pvc
```

```shell
#파드생성
kubectl apply -f nfs-pod.yaml
#공유폴더에 파일 생성
echo hello nginx >> /nfs_share/index.html
#파드(컨테이너) 내부 접근
kubectl exec -it nfs-pod -- /bin/bash 
#root@nfs-pod:/ 컨테이너
cd /usr/share/nginx/html/
cat index.html
```

사진넣기

----

# 끝

----

## reference
[교수님 강의 및 블로그, hull.kr](https://hull.kr/cloud/28)
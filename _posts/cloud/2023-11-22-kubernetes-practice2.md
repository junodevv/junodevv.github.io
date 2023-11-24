---
title: "[Kubernetes] 쿠버네티스 실습 2 - 기본 사용법(1)"
category: Cloud
tags: cloud kubernetes
---

2023-11-22 클라우드 프로그래밍 12주차 강의2

-----

## 목차
- 매니페스트
- 파드 생성 및 관리
- 디플로이먼트와 서비스 사용
- 레플리카셋 사용

# 매니페스트

매니페스트(manifest)

- 쿠버네티스의 오브젝트를 생성하기 위한 메타 정보를 yaml이나 json으로 기술한 파일
- yaml이나 json은 데이터 표현의 한 양식
- yaml 파일은 인간이 보고 이해하기 쉬운 형태로 최근에 더 많이 사용
- yaml 파일에는 생성할 오브젝트나 컴포넌트 유형, 이름 등이 포함

디플로이먼트 YAML 파일 예시

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
spec:
  replicas: 4
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myfront
        image: myphpimage
        volumeMounts:
        - name: html-volume
          mountPath: /var/www/html
        ports:
        - containerPort: 80
      - name: mytaskapi
        image: myphpimage
        volumeMounts:
        - name: task-volume
          mountPath: /var/www/html
        ports:
        - containerPort: 80
      - name: myuserapi
        image: myphpimage
        volumeMounts:
        - name: user-volume
          mountPath: /var/www/html
        ports:
        - containerPort: 80
      volumes:
      - name: html-volume
        hostPath:
          path: /path/to/your/html
      - name: task-volume
        hostPath:
          path: /path/to/your/php/task
      - name: user-volume
        hostPath:
          path: /path/to/your/php/user
```

        replicas -> 파드를 몇개까지 만들것인가, 그리고 worker0, worker1의 자원 상태에따라 그 파드들을 적절히 분배한다.

서비스 YAML 예시
- nodePort를 설정하지 않으면, 30000-32767 범위 내에서 사용 가능한 포트를 자동으로 할당

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort #NodePort, LoadBalancer(kubectl get services 로 외부 서비스 IP 확인), ClusterIP(클러스터 내부에서만 접속, 외부접속 X)
  selector:
    app: myapp
  ports:
  - name: myfront
    port: 8011
    targetPort: 80
  - name: mytaskapi
    port: 8012
    targetPort: 80
  - name: myuserapi
    port: 8013
    targetPort: 80
```

        pod들은 master에는 없고 worker0, worker1에만 있음

# 파드 생성 및 관리

## 파드생성(실습)

- 파드 생성 명령어
    - kubectl create: 클러스터에 새로운 리소스를 생성
    - kubectl apply: create와 replace(생성된 오브젝트를 삭제하고 새로운 오브젝트를 생성)의 혼합

```shell
#create로 파드 생성
kubectl create deployment my-httpd --image=httpd --replicas=1 --port=80
```

- 명령어 내용
    - deployment: 스테이트리스(Stateless) 형태의 파드 생성
    - my-httpd: 디플로이먼트 이름
    - --image=httpd: 파드를 생성하는데 사용되는 이미지
    - --replicas=1: running 상태를 유지할 파드 개수
    - --port=80: 파드에 접근할 때 사용할 포트 번호

            - 스테이트리스: 파일을 내부에 저장하지 않고 외부 서버(외부의 파일시스템)에 저장한다. 왜냐면 여러개의 파드들이 운영되고 있고 그 파드들은 모두 같은 상태여야 하는데 내부에 저장하게 되면 동기화 문제가 발생한다.
            - 플라넬 = 오버레이 네트워크 : 내부적으로 접속할 수 있도록 해준다.

## 스테이트리스와 스테이트풀

- 스테이트리스(stateless, 상태가 없는)
    - 사용자가 애플리케이션을 사용할 때 상태나 세션을 저장해 둘 필요가 없는 애플리케이션에서 사용
- 스테이트풀(stateful)
    - 사용자가 애플리케이션을 사용할 때 상태나 세션을 별도의 데이터베이스에 저장해야 하는 애플리케이션에서 사용
    - 예시: 상품을 장바구니에 담고 구매하는 쇼핑몰, 금융 거래 사이트 등

## 디플로이먼트 관리(실습)

디플로이먼트 목록 확인

```shell
kubectl get deployment
```

- 목록 항목
    - READY: 레플리카의 개수
    - UP-TO-DATE: 최신 상태로 업데이트된 레플리카의 개수
    - AVAILBLE: 사용 가능한 레플리카의 개수
    - AGE: 파드가 실행되고 있는 지속 시간

## 디플로이먼트의 추가 정보를 포함하여 목록 확인

```shell
kubectl get deployment -o wide
```
- 목록항목
    - CONTANERS: 파드에 포함된 컨테이너의 개수
    - IMAGES: 파드 생성에 사용된 이미지
    - SELECTOR: yaml 파일의 셀렉터를 의미(yaml 파일의 셀렉터는 라벨이 app=my-httpd인 파드만을 선택해서 서비스를 하겠다는 의미)

## 파드 관리(실습)

파드 목록 확인

```shell
kubectl get pod
```

파드의 추가 정보를 포함하여 목록 확인

```shell
kubectl get pod -o wide
```

- 목록 항목
    - NODE: 파드가 실행되고 있는 노드
    - NOMINATED NODE: 예약된 노드의 이름(예약된 노드가 없으므로 none)
    - READINESS GATES: 파드 상태 정보로 사용자가 수정할 수 있음. 예를 들어 running, pending 상태가 기본이지만 creating 같은 좀 더 상세한 정보를 원한다면 READINESS GATES를 다음과 같이 설정하면 됨

curl 명령어로 httpd 웹 서버에 IP로 접속
```shell
#‘It works!’라는 결과가 출력되었다면 웹 서비스가 정상적으로 동작하고 있다는 의미
curl 10.36.0.1
```

## 파드 삭제(실습)

디플로이먼트 삭제

```shell
#kubectl delete deployment [ 디플로이먼트 이름 ]
kubectl delete deployment my-httpd
```

- 디플로이먼트를 삭제하면 파드도 삭제된다.

## 파드 관리(실습)

생성된 컨테이너나 파드에 접속
```shell
kubectl exec -it my-httpd-7547bdb59f-cdkd9 -- /bin/bash
```
- 명령어 설명
    - kubectl exec: 실행 중인 파드에 접속
    - i: 컨테이너에 대화형 셸(shell)을 생성
    - t: 대화형 터미널을 도커 컨테이너에 연결
    - my-httpd-7547bdb59f-cdk9: 접속할 파드의 이름
    - -- /bin/bash: 셸의 절대 경로
    - 이후 exit 명령어를 사용해 파드를 빠져나옴

파드의 로그 확인

```shell
kubectl logs my-httpd-7547bdb59f-cdkd9
```


끝
# Part1. Docker 기초

## 01. Docker 기초 파트 개요

컨테이너 기술의 발전 - 어떻게 서비스를 효율적으로 운영할 것인가?

비용 효율성, 컴퓨팅 자원의 최적화와 효과적

- Traditional Deployment - 하드웨어
- Virtualized Deployment - 가상화 도구, 낮은 성능 효율성, 자원 오버헤드 발생
- Container Deployment - 컨테이너 엔진, 격리 기술 적용, 높은 성능 효율성, 높은 자원 효율성
- K8S Deployment - 컨테이터 오케스트레이션 시스템

### kustomize 소개

- 쿠버네티스 매니페스트 파일 관리 도구

### minikube 소개

가상환경을 사용하여 쿠버네티스 클러스터를 구현, 드라이버를 선택하여 원하는 도커, 버츄얼박스 등 가상환경에서 구성이 가능하다.

```bash
minikube start --driver docker
```

- minikube 설정 확인

```bash
cat ~/.kube/config
```

- node 정보 확인

```bash
kubectl get nodes
```

- 클러스터 정보 확인

```bash
kubectl cluster-info
```

- minikube 기본 사용법
    - 쿠버네티스 클러스터 상태 확인

        ```bash
        minikube status
        ```

    - 쿠버네티스 클러스터 중지

        ```bash
        minikube stop
        ```

    - 쿠버네티스 클러스터 삭제

        ```bash
        minikube delete
        ```

    - 쿠버네티스 클러스터 일시 정지

        ```bash
        minikube pause
        ```

    - 쿠버네티스 클러스터 재시작

        ```bash
        minikube unpause
        ```

    - 추가 기능 확장 리스트

        ```bash
        minikube addons list
        ```

    - 클러스터 접속

        ```bash
        minikube ssh
        ```

    - kubectl 명령어 실행

        ```bash
        minikube kubectl
        ```

---

## 02. 도커를 이용한 컨테이너 관리

### 01. 도커 이미지와 컨테이너

- 이미지와 컨터이너는 도커 기본 단위
    - 이미지
        - 컨테이너 생성 필요 요소
        - 바이너리와 의존성 설치됨
        - 읽기 전용
    - 컨테이너
        - 호스트와 다른 컨테이너로부터 격리된 시스템 자원과 네트워크를 사용하는 프로세스
- 도커 이미지 이름 구성
    - 저장소 이름
    - 이미지 이름
    - 이미지 태그 - 생략시 lastest 최신 버전으로 인식
    - [저장소 이름]/[이미지 이름]/[이미지 태그]
- 도커 이미지 저장소
    - 도커 이미지 관리 및 공유 서버 어플리케이션

### 02. 도커 컨테이너 다루기: 컨테이너 라이프 사이클

- 컨테이너 라이프 사이클

![img.png](img/img.png)
![img_1.png](img/img_1.png)

- 컨테이너 시작

  create, run 명령오 모두 이미지가 없는 경우 자동으로 이미지를 pull 하여 실행

    - 컨테이너 생성 / 시작 / 생성 및 시작
        - docker create [image]
        - docker start [container]
        - docker run [image]
- 주요 옵션
    - -i : 호스트의 표준 입력을 컨테이너와 연결 interactive
    - -t : TTY 할당
    - -it 로 함께 주로 사용됨 : ctrl + p + q 로 실행하면서 나오기
    - -d : 백그라운드 실행
    - —rm : 컨테이너 실행 종료 후 자동 삭제
    - —name : 컨테이너 이름 지정
    - -p 80:8000 : 호스트 - 컨테이너 간 포트 바인딩
    - -v /opt/example:/example \ : 호스트 - 컨테이너 간 볼륨 바인딩

- 컨테이너 일시 중지 및 재개
    - 컨테이너 일시 중지
        - docker pause [container]
    - 컨테이너 재개
        - docker unpause [container]
- 컨테이너 종료
    - 컨테이너 종료 SIGTERM 시그널 전달
        - docker stop [container]
    - 모든 컨테이너 종료
        - docker stop $(docker ps -a -q)
    - 컨테이너 강제 종료 SIGKILL 시그널 전달
        - docker kill [container]

### 03. 도커 컨테이너 다루기: 엔트리포인트와 커맨드

- 엔트리포인트
    - 도커 컨테이너가 실행할 때 고정적으로 실행되는 스크립트, 명령어
    - 생략시 지정된 명령어 수행
- 커맨드
    - 도커 컨테이너가 실행할 때 수행할 명령오 혹은 엔트리포인트에 지정된 명령어에 대한 인자 값

실제 수행되는 컨테이너 명령어 [Entrypoint] [Command]

Entrypoint 가 프리픽스로 지정되어 있음

- Dockerfile 의 엔트리포인트와 커맨드
    - ENTRYPOINT [”docker-entrypoint.sh”]
    - CMD [”node”]
    - 인 경우 실제 명령어는 아래와 같다

        ```bash
        [docker-entrypoint.sh](http://docker-entrypoint.sh) node
        ```


- 도커 명령어의 엔트리포인트와 커맨드

    ```bash
    docker run --entrypoint sh ubuntu:focal
    docker run --entrypoint echo ubuntu:focal hello world 
    ```

도커 컨테이너 실행시에 엔트리포인트와 커맨드 모두 변경이 가능하다

### 04. 도커 컨테이너 다루기: 환경 변수

- -e run 옵션을 사용하여 환경변수 주입
- 파일로 환경 변수 주입
    - docker run -it —env-file ./환경변수파일.env

### 05. 도커 컨테이너 다루기: 명령어 실행

```bash
docker exec [container] [command]
```

- 실행 중인 컨테이너에 접속 가능

    ```bash
    docker exec -it my-nginx bash
    ```

- 환경 변수 확인하기

    ```bash
    docker exec my-nginx env
    ```

### 06. 도커 컨테이너 다루기: 네트워크

veth : vertual eth

- docker 0: 도커 엔진에 의해 기본 생성되는 브릿지 네트워크 ⇒ veth 와 eth 간 다리 역할

![img.png](img.png)

- 컨테이너 포트를 호스트의 ip:post 와 연결하여 서비스 노출

```bash
docker run -p [Host IP:PORT]:[Container port] [container]

docker run -d -p 127.0.0.1:80:80 nginx
# ip 지정 

docker run -d -p 80 nginx
# 컨테이너의 80번 포트를 호스트의 사용 가능한 포트와 연결 후 실행 
```

- —expose : 문서화하는 용도로만 사용

#### [도커 네트워크 드라이버](https://docs.docker.com/network/drivers/)

- Single Host Networking
    - bridge
    - host
    - none
- Multi Host Networking
    - overlay: 가상 네트워크
# Ch.2 Docker 빌드를 위한 Jenkins CI 활용

## 01. Docker 빌드를 위한 Jenkins 활용 소개

### 빌드란 무엇인가?

- 소프트웨어 빌드는 소스 코드 파일을 실행 가능한 독립 소프트웨어 아티팩트로 변환하는 과정을 말하거나 그에 대한 결과물을 말한다.

### 빌드 방식

- 전체 빌드
    - 매 빌드마다 전체 코드 포함해서 빌드
    - 처음부터 전체 프로젝트의 종속성을 확인
    - 모든 소스 파일을 컴파일 및 빌드하고 최종적으로 빌드된 아티팩트에 합침
- 증분 빌드
    - 변경된 코드 대상만 분리해 빌드
    - 프로젝트 내 모든 소스 파일 및 종속성을 확인해 변경된 경우 해당 대상만 다시 빌드
    - 전체 빌드 방식보다 빠르고 리소스 덜 사용
- 도커 빌드가 전체 빈드보다는 증분 빌드 형태로 변경된 코드 대사만 분리해 사용

### Dockerfile 설정 명령어

| 명령어        | 기능                    |
|------------|-----------------------|
| FROM       | 컨테이너 Base Image 지정    |
| RUN        | 명령어 실행                |
| CMD        | 컨테이너 내에서 실행 명령어       |
| LABEL      | Label 설정              |
| EXPOSE     | 컨테이너 포트 노출            |
| ENV        | 환경 변수                 |
| ADD        | 파일/디렉터리 경로 추가         |
| COPY       | 파일 복사                 |
| ENTRYPOINT | 컨테이너 실행시 실행되는 명령어     |
| VOLUME     | 특정 경로를 컨테이너 볼륨으로 마운트  |
| USER       | 사용자 지정                |
| WORKDIR    | 작업 디렉터리               |
| ARG        | Dockerfile 에서 사용되는 변수 |

- RUN
    - Dockerfile 로부터 도커 이미지를 빌드하는 순간에 실행 되므로 RUN 명령어는 라이브러리 설치를 하는 부분에서 주로 활용 됨
- CMD
    - RUN 명령어가 이미지를 빌드할 때 실행되는 것과 다르게 이미지로부터 컨테이너를 생성하여 실행 중 사용이 가능한 명령어
- ENTRYPOINT
    - 컨테이너 실행 후 최초로 실행되는 명령어가 있을 때만 사용

### Docker 빌드 명령어

- 기본 빌드 명령어
    - docker build -t [이미지명] [빌드할 코드 경로]
- dockerfile 지정을 위한 -f 옵션 적용
    - docker build [생성할 이미지명] -f [dockerfile 파일명] [빌드할 코드 경로]

### Docker in Ubuntu

- ubuntu 계정에 docker 실행 권한을 주기

    ```bash
    sudo chmod 666 /var/run/docker.sock
    ```

- 도커 설치 후 데몬 기동 현황 확인

    ```bash
    sudo systemctl status docker
    ```

## 02. Gradle을 활용한 빌드 준비

Gradle 은 Groovy 를 이용한 빌드 자동화 시스템이다.

### Gradle 수행 순서

1. 초기화 단계
    - Gradle에 빌드할 프로젝트를 설정하고 생성
2. 구성 단계
    - 프로젝트 객체가 구성함. 빌드에 포함할 프로젝트의 빌드 스크립트 및 Task 생성
3. 실행 단계
    - Gradle 의 모든 Task 를 통합하고 빌드를 실행 


## 03. Jenkins 소개와 설치 

젠킨스는 소프트웨어 개발시 지속적 통합 소비스를 제공하는 툴이다. 버전 충돌 방지를 위해 작업한 내용을 공유 영역에 
있는 Git 등의 저장소에 빈번히 업로드함으로써 지속적 통합이 가능하도록 해준다.

## 04. Jenkins를 활용한 Docker 빌드 

```bash
docker run --name jenkins -d -p 8080:8080 -v ~/jenkins:/var/jenkins_home -u root jenkins/jenkins
```
# Docker Basics

1. [Docker?](#1-docker) <br />
    1-1. [개념](#1-1-개념) <br />
    1-2. [이미지와 컨테이너](#1-2-이미지와-컨테이너) <br />
2. [라이프사이클](#2-라이프사이클) <br />
    2-1. [created](#2-1-created) <br />
    2-2. [running](#2-2-running) <br />
    2-3. [paused](#2-3-paused) <br />
    2-4. [stopped](#2-4-stopped) <br />
    2-5. [deleted](#2-5-deleted) <br />
3. [엔트리포인트와 커맨드](#3-엔트리포인트와-커맨드) <br />
    3-1. [엔트리포인트](#3-1-엔트리포인트) <br />
    3-2. [커맨드](#3-2-커맨드) <br />
4. [환경변수 설정](#4-환경변수-설정) <br />
5. [도커 컨테이너 명령어 실행](#5-도커-컨테이너-명령어-실행) <br />
6. [도커 네트워크](#6-도커-네트워크) <br />
    6-1. [도커 네트워크 확인 및 생성](#6-1-도커-네트워크-확인-및-생성) <br />
    6-2. [도커 컨테이너 포트포워딩](#6-2-도커-컨테이너-포트포워딩) <br />
7. [도커 볼륨](#7-도커-볼륨) <br />
    7-1. [호스트 볼륨 공유](#7-1-호스트-볼륨-공유) <br />
    7-2. [컨테이너 볼륨을 다른 컨테이너에 공유](#7-2-컨테이너-볼륨을-다른-컨테이너에-공유) <br />
    7-3. [도커가 제공하는 볼륨](#7-3-도커가-제공하는-볼륨) <br />
8. [도커 로그](#8-도커-로그) <br />
9. [도커 이미지](#9-도커-이미지) <br />
    9-1. [도커 commit 활용](#9-1-도커-commit-활용) <br />
    9-2. [dockerfile 활용](#9-2-dockerfile-활용) <br />
    9-3. [docker hub 활용](#9-3-docker-hub-활용) <br />

## 1. Docker?

<p align="center">
    <img width="500" alt="docker image" src="https://github.com/jongeunShin95/TIL/assets/20867824/c5872d13-ae07-4f0c-9f22-e3af0c97587e">
    <p align="center"><I>Docker Image</I></p>
</p>

### 1-1. 개념

Docker는 **컨테이너** 기반으로 빌드, 배포, 실행, 업데이트 등 관리할 수 있는 **오픈소스 가상화 플랫폼**이다. 프로그램, 실행환경 등 필요한 모든것들을 하나의 컨테이너로 패키징하여 배포 및 실행할 수 있도록 하며, 같은 도커 환경에서는 실행하는 OS 등의 **환경에 상관없이 모든 환경에서 컨테이너를 실행할 수 있다.**

<br />

### 1-2. 이미지와 컨테이너

먼저 도커 이미지는 컨테이너를 생성할 때 필요한 요소로 프로그램, 라이브러리 등 **바이너리와 의존성을 묶은 바이너리 파일**을 말한다.

도커 컨테이너는 도커 이미지를 실행한 형태로 **다른 컨테이너와 격리된 자원 및 네트워크를 사용하는 프로세스**를 말한다. 컨테이너에서 하는 행위들을 도커 이미지에 영향을 주지 않는다.

완전히 동일하지는 않겠지만 도커 이미지는 프로그램, 도커 컨테이너는 프로세스로 생각해도 될 것 같다.

## 2. 라이프사이클

<p align="center">
    <img width="500" alt="docker life cycle" src="https://github.com/jongeunShin95/TIL/assets/20867824/c6ea1c96-9ff5-4b5d-860c-0c73b8cd1f74">
    <p align="center"><I>Life Cycle</I></p>
</p>

위 그림과 같이 컨테이너 라이프사이클은 복잡하며 **created, running, paused, stopped, deleted** 에 대한 상태를 명령어를 통해 실행해보려고 한다.

### 2-1. created

```javascript
$ docker create nginx // nginx 컨테이너 생성
// docker ps -a 를 통해 Created 된 상태를 확인할 수 있다.
```

### 2-2. running

```javascript
$ docker start 4fb // 4fb는 create 통해 생성된 컨테이너 ID의 앞 3자리이다.
// docker ps 를 통해 실행된 컨테이너를 확인할 수 있다.
```

### 2-3. paused

```javascript
$ docker pause 4fb // pause는 도커를 일시중지 한 상태이다.
// docker ps 를 통해 pause 상태를 확인할 수 있으며 unpause 를 통해 다시 실행할 수 있다.
```

### 2-4. stopped

```javascript
$ docker stop 4fb // 도커 컨테이너를 종료시킨다.
// docker ps -a 를 통해 종료된 상태를 확인할 수 있다.
```

### 2-5. deleted

```javascript
$ docker rm 4fb // 도커 컨테이너를 삭제시킨다.
// docker ps -a 를 통해 확인해보면 아무것도 안나오는것을 확인할 수 있다.
```

## 3. 엔트리포인트와 커맨드

엔트리포인트와 커맨드 둘 다 **도커 컨테이너가 실행될 때 수행하게 되는 명령어를 선언**하는 기능이다. 둘 다 지정된 명령어를 수행하는 선언문이지만 약간의 차이점이 있으며 차이점은 다음과 같다.

<br />

> [엔트리포인트][커맨드] 순으로 실행됨

### 3-1. 엔트리포인트
도커 컨테이너가 실행될 때 반드시 실행되는 명령으로 docker run 을 통해 실행될 때 추가 명령 인자로 덮어쓸 수 없다.

### 3-2. 커맨드
도커 컨테이너가 실행될 때 수행되는 기본 인자값을 말하며 docker run 을 통해 명령어를 전달할 시 기존 명령어를 덮어쓴다.

## 4. 환경변수 설정

-e 옵션을 통해 실행될 컨테이너의 환경변수를 미리 선언하여 셋팅할 수 있다.

<p align="center">
    <img width="500" alt="docker evn" src="https://github.com/jongeunShin95/TIL/assets/20867824/1ec4feb2-9872-4948-ae35-86b838aa409e">
    <p align="center"><I>docker env 설정</I></p>
</p>

## 5. 도커 컨테이너 명령어 실행

docker exec 명령어를 통해 컨테이너에 명령어를 실행할 수 있다.

<p align="center">
    <img width="500" alt="docker exec env" src="https://github.com/jongeunShin95/TIL/assets/20867824/ebe5c2a5-80f3-4ef5-8b4f-362607d7f458">
    <p align="center"><I>컨테이너의 환경변수 확인</I></p>
</p>

<p align="center">
    <img width="500" alt="docker exec -it bash" src="https://github.com/jongeunShin95/TIL/assets/20867824/9829a4f5-4229-4271-986c-3f4a779201f1">
    <p align="center"><I>컨테이너의 쉘 접속</I></p>
</p>

## 6. 도커 네트워크

<p align="center">
    <img width="400" alt="docker network" src="https://github.com/jongeunShin95/TIL/assets/20867824/915ade2c-6e91-4063-87b4-59113c9afe4a">
    <p align="center"><I>도커 네트워크</I></p>
</p>

먼저 veth를 살펴보면 컨테이너가 생성될 때 자동으로 생성되며 Host의 eth0와 연결하기 위한 가상 네트워크 인터페이스이다. 그리고 이 host의 eth0와 veth를 연결해주는, 즉 브릿지 역할을 해주는 것이 docker0이다.

### 6-1. 도커 네트워크 확인 및 생성

docker network ls 통해 네트워크 목록을 확인할 수 있다.

<p align="center">
    <img width="400" alt="docker network ls" src="https://github.com/jongeunShin95/TIL/assets/20867824/ea61a974-302d-430d-8965-2d1dbe06e15e">
    <p align="center"><I>네트워크 확인</I></p>
</p>

해당 도커 네트워크에서 none 네트워크를 사용하는 도커 컨테이너를 생성해본다.

```javascript
$ docker run -d --net none nginx // net 옵션을 통해 네트워크 이름을 준다.
```

해당 네트워크를 inspect 명령어를 통해 network 부분을 보면 none으로 설정된 것을 확인할 수 있다.

<p align="center">
    <img width="400" alt="docker inspect" src="https://github.com/jongeunShin95/TIL/assets/20867824/27176a6b-4272-4fc9-a681-9aed21917ccd">
    <p align="center"><I>생성한 컨테이너의 네트워크 확인</I></p>
</p>

network create 명령어를 통하면 도커 네트워크를 새로 생성할 수 있다.

<p align="center">
    <img width="400" alt="docker network create" src="https://github.com/jongeunShin95/TIL/assets/20867824/7dcf3b1d-c755-4c88-9d18-cbeb7dec0f9f">
    <p align="center"><I>네트워크 생성</I></p>
</p>

### 6-2. 도커 컨테이너 포트포워딩

> $ docker run -d **-p [host_port]:[container_port]** [container]

위와 같이 -p 옵션을 통해 HOST PORT:CONTAINER PORT를 줄 수 있다.

<p align="center">
    <img width="400" alt="docker port forwarding" src="https://github.com/jongeunShin95/TIL/assets/20867824/51c1cc79-808d-4ab8-b57e-e06411650f08">
    <p align="center"><I>도커 포트 포워딩</I></p>
</p>

80번 포트를 이용해 컨테이너의 nginx를 localhost에서 접속할 수 있는 것을 확인할 수 있다.

## 7. 도커 볼륨

### 7-1. 호스트 볼륨 공유

```python
# 호스트의 /opt/html 디렉토리를 Nginx의 웹 루트 디렉토리로 마운트
$ docker run -d \
  --name nginx \
  -v /opt/html:/usr/share/nginx/html \
  nginx
```

위와 같이 호스트의 디렉토리를 컨테이너의 디렉토리로 마운트가 가능하다. 아래는 /Users/jongeun/development/html 디렉토리를 nginx html에 마운트해보고 제대로 되었는지 확인을 위해 localhost 접속을 통해 확인해본다.

<p align="center">
    <img width="500" alt="docker host mount" src="https://github.com/jongeunShin95/TIL/assets/20867824/6b15682a-9ca4-4666-baa9-bf3c01d1911e">
    <p align="center"><I>호스트 디렉토리 마운트하여 컨테이너 실행</I></p>
</p>

<br />

<p align="center">
    <img width="300" alt="connect" src="https://github.com/jongeunShin95/TIL/assets/20867824/2d2a5b9e-50db-449c-947d-8af5abf9b4ba">
    <p align="center"><I>정상적으로 호스트 html 표시 확인</I></p>
</p>

<br />

만약 호스트 /Users/jongeun/development/html 디렉토리의 index.html 을 변경하면 변경된 화면을 바로 볼 수 있다.

<p align="center">
    <img width="300" alt="connect" src="https://github.com/jongeunShin95/TIL/assets/20867824/2daaa0e6-4234-4d67-9447-afe19d313226">
    <p align="center"><I>정상적으로 변경</I></p>
</p>

### 7-2. 컨테이너 볼륨을 다른 컨테이너에 공유

호스트의 디렉토리를 마운트 한 컨테이너의 볼륨을 다른 컨테이너로 공유하여 사용할 수 있다.
아래는 my-volume 이라는 호스트 디렉토리를 마운트 한 컨테이너를 만들고 이를 nginx 컨테이너에 공유하는 것을 확인해본다.

<p align="center">
    <img width="400" alt="create my-volume" src="https://github.com/jongeunShin95/TIL/assets/20867824/145a3617-0247-4f6b-bde5-6ba6794089be">
    <p align="center"><I>my-volume 컨테이너 생성</I></p>
</p>

<br />

<p align="center">
    <img width="400" alt="create nginx" src="https://github.com/jongeunShin95/TIL/assets/20867824/0825986d-aca5-4064-a7b2-b4ea7c9ac43a">
    <p align="center"><I>my-volume 볼륨을 사용하는 nginx 컨테이너 생성</I></p>
</p>

<br />

위와 같이 nginx 컨테이너를 생성하여 localhost로 들어가보면 정상적으로 출력이 되는 것을 확인할 수 있다.

### 7-3. 도커가 제공하는 볼륨

도커에서 기본적으로 제공하는 볼륨 관리 기능을 활용하여 데이터 관리를 할 수 있다. 해당 부분은 간단하게 도커 볼륨 명령어를 통해 볼륨을 생성하는 방법을 확인해본다.

```python
# my-volume 이라는 볼륨을 생성
$ docker volume create --name my-volume

# 마운트 할 때는 해당 -v 옵션에 볼륨 이름을 매핑
-v my-volume:/usr/share/nginx/html

# ro 옵션을 통해 읽기 전용으로 볼륨을 마운트
-v my-volume:/usr/share/nginx/html:ro
```

## 8. 도커 로그

도커 컨테이너의 로그를 logs 명령어를 통해 확인할 수 있다.

```python
# 전체 로그 확인
$ docker logs [container]

# 마지막 로그 10줄 확인
$ docker logs --tail 10 [container]

# 실시간 로그 스트림 확인
$ docker logs -f [container]

# 로그마다 타임스탬프 표시
$ docker logs -f -t [container]
```

<br />

<p align="center">
    <img width="400" alt="docker logs" src="https://github.com/jongeunShin95/TIL/assets/20867824/2c9eaefb-0dce-4e08-b6e5-c6aa1a6c5787">
    <p align="center"><I>도커 로그 확인</I></p>
</p>

## 9. 도커 이미지

도커 이미지는 컨테이너를 생성 및 실행할 때 필요한 요소이다. 아래 그림과 같이 하나의 도커 이미지에 여러 개의 layer가 있고 이 layer를 쌓아 하나의 이미지를 만들어 컨테이너를 실행한다. 실행된 컨테이너에서는 이미지에 대한 layer는 읽기 전용으로만 가능하며 실행하면서 생기는 layer의 경우에는 컨테이너 layer에 쌓기게 된다.

<p align="center">
    <img width="400" alt="docker image" src="https://github.com/jongeunShin95/TIL/assets/20867824/c858dc4d-b54c-41c1-8afd-c9dab0ad15d9">
    <p align="center"><I>도커 이미지</I></p>
</p>

### 9-1. 도커 commit 활용

기존 컨테이너를 기반으로 새로운 이미지를 만들 수 있다.

```python
docker commit [OPTIONS] CONTAINER [CONTAINER_NAME:TAG]
```

<p align="center">
    <img width="400" alt="docker commit" src="https://github.com/jongeunShin95/TIL/assets/20867824/848f04cf-3fd2-4497-baae-514302deaac3">
    <p align="center"><I>도커 커밋</I></p>
</p>

위 예시는 실행중인 nginx 컨테이너를 기반으로 jong-nginx:1.0 이미지를 생성한 것이다.

### 9-2. Dockerfile 활용

Dockerfile 을 활용하여 build를 사용할 수 있다. 아래 예제는 create-react-app 을 통해 만들어낸 프로젝트를 Dockerfile을 통해 build 하고 실행하는 예제이다. <br />

먼저 create-react-app을 통해 프로젝트를 생성하고 Dockerfile을 생성한다.

<p align="center">
    <img width="400" alt="init" src="https://github.com/jongeunShin95/TIL/assets/20867824/e8de5f26-8b9e-4cf1-9881-748fe7d62e6c">
    <p align="center"><I>react, dockerfile</I></p>
</p>

<br />

그리고 아래 사진과 같이 reactapp 을 실행시킬 수 있도록 Dockerfile 을 작성한다.

<p align="center">
    <img width="300" alt="Dockerfile" src="https://github.com/jongeunShin95/TIL/assets/20867824/935c9ef9-2587-442f-bdab-a9f1fab7d7e3">
    <p align="center"><I>Dockerfile</I></p>
</p>

<br />

작성한 Dockerfile 을 가지고 build 명령어를 통해 현재 폴더 기준으로 도커 이미지를 생성한다.

<p align="center">
    <img width="300" alt="docker build" src="https://github.com/jongeunShin95/TIL/assets/20867824/3bc8f78f-11fb-4a6b-9f64-5995e0156bcf">
    <p align="center"><I>dokcer build</I></p>
</p>

<br />

<p align="center">
    <img width="300" alt="docker image" src="https://github.com/jongeunShin95/TIL/assets/20867824/19fd045a-560a-450a-b0f8-260924dacbf4">
    <p align="center"><I>dokcer image</I></p>
</p>

<br />

생성한 이미지를 기반으로 docker run 을 통해 컨테이너를 실행하면 아래와 같이 정상적으로 react app 이 실행된 것을 볼 수 있다.

<p align="center">
    <img width="300" alt="docker run" src="https://github.com/jongeunShin95/TIL/assets/20867824/4945df7a-b520-4889-966c-d561f384770d">
    <p align="center"><I>dokcer run</I></p>
</p>

<br />

### 9-3. Docker hub 활용

다음으로는 로컬의 도커 이미지를 [DockerHub](https://hub.docker.com/) 에 저장하고 저장된 이미지를 다시 로컬에 가져오는 예제를 진행해본다. <br />

먼저 dockerhub 에서 레파지토리를 하나 만든다.

<p align="center">
    <img width="300" alt="dockerhub" src="https://github.com/jongeunShin95/TIL/assets/20867824/c108d10f-1ef3-40c0-bf37-e5278378af55">
    <p align="center"><I>dockerhub</I></p>
</p>

<br />

dockerhub 에 push 하기 위해 도커아이디를 포함하여 tag 를 이용해 이미지를 만든다.

<p align="center">
    <img width="300" alt="tag docker image" src="https://github.com/jongeunShin95/TIL/assets/20867824/8df63090-78b2-4bf1-8520-7875e41fefec">
    <p align="center"><I>tag docker image</I></p>
</p>

<br />

dockerhub 에 이미지를 push 하면 dockerhub 에서 저장된 이미지와 태그를 확인할 수 있다.

<p align="center">
    <img width="300" alt="image push" src="https://github.com/jongeunShin95/TIL/assets/20867824/6911984e-f944-4cb3-85a1-6002c583393c">
    <p align="center"><I>docker image push</I></p>
</p>

<br />

dockerhub 에서 이미지를 받아올 수 있는지 확인하기 위해 로컬에 만들어진 이미지를 삭제한다.

<p align="center">
    <img width="300" alt="image rm" src="https://github.com/jongeunShin95/TIL/assets/20867824/a88466be-ea0b-444e-bb39-f67e4f424597">
    <p align="center"><I>docker image rm</I></p>
</p>

<br />

그리고 pull 을 통해 dockerhub 에서 이미지를 받아오고 로컬에서 이미지를 확인해보면 잘 받아오는 것을 확인할 수 있다.

<p align="center">
    <img width="300" alt="image pull" src="https://github.com/jongeunShin95/TIL/assets/20867824/d11682c1-90b7-4921-8ea5-16ca9f66ae11">
    <p align="center"><I>docker image pull</I></p>
</p>

<br />
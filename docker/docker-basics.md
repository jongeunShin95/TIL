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
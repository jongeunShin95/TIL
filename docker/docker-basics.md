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
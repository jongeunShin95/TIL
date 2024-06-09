# Kubernetes Basics

1. [Kubernetes?](#1-Kubernetes) <br />
    1-1. [컨테이너 오케스트레이션 시스템](#1-1-컨테이너-오케스트레이션-시스템) <br />
    1-2. [쿠버네티스인 이유](#1-2-쿠버네티스인-이유) <br />
2. [클러스터 구성](#2-클러스터-구성) <br />
    2-1. [Ctrl Plane (Master Node)](#2-1-ctrl-plane-master-node) <br />
    2-2. [Node (Worker Node)](#2-2-node-worker-node) <br />
3. [API 리소스와 오브젝트](#3-api-리소스와-오브젝트) <br />

## 1. Kubernetes?

<p align="center">
    <img width="500" alt="Kubernetes image" src="https://github.com/jongeunShin95/TIL/assets/20867824/d7f19d53-e295-4dde-9889-0e79bd66f932">
    <p align="center"><I>Kubernetes Image</I></p>
</p>

### 1-1. 컨테이너 오케스트레이션 시스템

컨테이너의 **배포, 관리, 확장, 네트워킹을 자동화하는 기술**로 로드 밸런싱, 롤백&롤아웃, 오토 스케일링 등 기능을 제공한다. 그리고 이 컨테이너 오케스트레이션 시스템은 **여러 머신으로 구성된 클러스터 상에서 컨테이너를 효율적으로 관리하기 위한 시스템**이다. 이 오케스트레이션 시스템을 제공하는 툴로는 kubernetes, docker swarm, mesos, rancher, nomad 등 소프트웨어들이 존재하고 여기서는 kubernetes 에 대해 정리를 할 것이다.

### 1-2. 쿠버네티스인 이유

그렇다면 저 많은 도구들 중 왜 쿠버네티스인가를 간단히 살펴보면 우선 쿠버네티스는 수 십억 개의 컨테이너를 운영하는 구글에서 설계된 도구이다. 이에 따라,  수 십억 개의 컨테이너를 운영할 수 있게 한 원칙이 있기에 소규모부터 대규모까지 규모를 확장할 수 있다. 그리고 필요한 기능이 없을 경우 CRD(Custom Resource Definitions) 을 통해 기능을 확장할 수 있으며 On-premise/Public Cloud/Hybrid 등 어느 환경에서나 동작이 가능하고 이러한 장점들이 많아 쿠버네티스를 거의 컨테이너 오케스트레이션 시스템의 표준으로 삼고 있기도 하다.


## 2. 클러스터 구성

<p align="center">
    <img width="500" alt="Kubernetes Cluster" src="https://github.com/jongeunShin95/TIL/assets/20867824/29034f94-74cb-436d-a7c3-3839f7567b85">
    <p align="center"><I>Kubernetes Cluster</I></p>
</p>

<br />

쿠버네티스를 실행 중이라면 클러스터를 실행하고 있는 것으로 최소 컨트롤 플레인 및 하나 이상의 워크 노드를 포함하고 있다. 컨트롤 플레인은 클러스터의 관리하는 역할을 담당으로 클러스터를 원하는 상태로 유지 및 관리한다. 노드는 어플리케이션 컨테이너를 실행하는 역할을 담당한다.

### 2-1. Ctrl Plane (Master Node)

<p align="center">
    <img width="500" alt="Ctrl Plane" src="https://github.com/jongeunShin95/TIL/assets/20867824/deafacec-7993-4d0a-bbaf-7ca927e0e37b">
    <p align="center"><I>Ctrl Plane</I></p>
</p>

<br />

**API Server**

* 쿠버네티스 리소스와 클러스터 관리를 위한 API 제공
* etcd 를 데이터 저장소로 사용

**Scheduler**

* 워크노드의 자원 사용 상태를 관리하며 새로운 워크로드(쿠버네티스에서 구동되는 어플리케이션)의 배포 관리

**Controller Manager**

* 여러 컨트롤러 프로세스를 관리
* API Server 를 주기적으로 체크하면서 현재 클러스터 상태와 etcd 에 저장된 리소스 상태를 비교하여 변화가 있다면 현재 클러스터에 반영하는 reconcile 과정을 반복 수행

**etcd**

* 분산 key - value 저장소로 클러스터의 상태를 저장

### 2-2. Node (Worker Node)

<p align="center">
    <img width="500" alt="Worker Node" src="https://github.com/jongeunShin95/TIL/assets/20867824/097f7404-869d-4c5c-9c54-73188b1b5b1a">
    <p align="center"><I>Worker Node</I></p>
</p>

<br />

**kubelet**

* Master Node 의 API Server 와 통신하며 노드의 리소스 상태를 관리
* Container Runtime 과 통신하며 컨테이너의 라이프사이클 관리

**CRI (Container Runtime Interface)**

* kubelet 이 Container Runtime 과 통신할 때 사용되는 인터페이스
* docker runtime 말고도 containerd, cri-o 등 runtime 을 지원하며 이를 CRI 통해 kubelet 과 통신가능하게 함

**kube-proxy**

* 오버레이 네트워크 구성
* 네트워크 프록시 및 내부 로드밸런서 역할 수행

## 3. API 리소스와 오브젝트

* 쿠버네티스가 관리할 수 있는 오브젝트 종류로 YAML 기반 매니페스트 파일로 오브젝트를 만들고 수정하고 삭제하는 등 오브젝트를 관리
* Pod, Service, ConfigMap, Secret, Node, NameSpace 등 오브젝트 종류 존재

<p align="center">
    <img width="500" alt="api-resources" src="https://github.com/jongeunShin95/TIL/assets/20867824/9c0c186f-35b5-427b-90d7-270f64c83903">
    <p align="center"><I>api-resources</I></p>
</p>

<br />
# Kubernetes Basics

1. [Kubernetes?](#1-Kubernetes) <br />
    1-1. [컨테이너 오케스트레이션 시스템](#1-1-컨테이너-오케스트레이션-시스템) <br />
    1-2. [쿠버네티스인 이유](#1-2-쿠버네티스인-이유) <br />

## 1. Kubernetes?

<p align="center">
    <img width="500" alt="Kubernetes image" src="https://github.com/jongeunShin95/TIL/assets/20867824/d7f19d53-e295-4dde-9889-0e79bd66f932">
    <p align="center"><I>Kubernetes Image</I></p>
</p>

### 1-1. 컨테이너 오케스트레이션 시스템

컨테이너의 **배포, 관리, 확장, 네트워킹을 자동화하는 기술**로 로드 밸런싱, 롤백&롤아웃, 오토 스케일링 등 기능을 제공한다. 그리고 이 컨테이너 오케스트레이션 시스템은 **여러 머신으로 구성된 클러스터 상에서 컨테이너를 효율적으로 관리하기 위한 시스템**이다. 이 오케스트레이션 시스템을 제공하는 툴로는 kubernetes, docker swarm, mesos, rancher, nomad 등 소프트웨어들이 존재하고 여기서는 kubernetes 에 대해 정리를 할 것이다.

### 1-2. 쿠버네티스인 이유

그렇다면 저 많은 도구들 중 왜 쿠버네티스인가를 간단히 살펴보면 우선 쿠버네티스는 수 십억 개의 컨테이너를 운영하는 구글에서 설계된 도구이다. 이에 따라,  수 십억 개의 컨테이너를 운영할 수 있게 한 원칙이 있기에 소규모부터 대규모까지 규모를 확장할 수 있다. 그리고 필요한 기능이 없을 경우 CRD(Custom Resource Definitions) 을 통해 기능을 확장할 수 있으며 On-premise/Public Cloud/Hybrid 등 어느 환경에서나 동작이 가능하고 이러한 장점들이 많아 쿠버네티스를 거의 컨테이너 오케스트레이션 시스템의 표준으로 삼고 있기도 하다.
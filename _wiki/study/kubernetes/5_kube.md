---
layout  : wiki
title   : Kubernetes 오브젝트 / 명세(spec)
summary : 쿠버네티스가 리소스를 관리하는 방법
date    : 2021-10-11 09:00:00 +0900
updated : 2021-10-11 09:00:01 +0900
tag     : kubernetes
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}


![basic_object](https://user-images.githubusercontent.com/65143458/137586050-3a70b7ba-2aae-4ef1-bcfe-d9c0f6ec03a6.png)


![kube_state_metrics](https://user-images.githubusercontent.com/65143458/137589826-2793e785-4ec8-4f69-82ec-2397e8151be2.jpeg)


## 개 요

* 쿠버네티스는 리소스를 관리하기 위해 여러 오브젝트를 도입해 상태값을 관리함

<br/>

## 기본 오브젝트

* 노드 (Node)

    * 리소스 중에서 가장 큰 개념으로, 도커 호스트가 설치된 서버를 가리킴

    * 도커 컨테이너가 배치되는 대상으로 쿠버네티스 클러스터를 구성하는 최소 단위임

* 파드 (Pod)

    * 컨테이너가 모인 집합체

    * 서비스가 구동되는 최소 단위

* 네임스페이스
    
    * 쿠버네티스 클러스터 내의 가상 클러스터

    * 특정 이름으로 클러스터의 영역을 구분하기 위한 논리적 개념


* 볼륨

    * 기본적으로 볼륨은 디렉터리로 파드 내 컨테이너에서 접근 가능함

* 서비스

    * 클러스터 안에서 파드의 집합에 대한 경로

    * 내부에 있는 파드에 안정적으로 접속할 수 있도록 내/외부를 연결하는 레이블

<br/>

## 고가용성을 위한 쿠버네티스 오브젝트

* 레플리카셋

    * 가용성을 확보하기 위해 같은 정의를 가지는 파드를 여러 개 생성하고 관리하기 위한 리소스


* 디플로이먼트
    
    * 레필리카셋 보다 상위에 있는 리소스로, 애플리케이션 배포의 기본 단위가 됨

    * 파드와 레플리카셋에 대해 선언적인 업데이트를 가능하게 하는 오브젝트

<br/>

## 쿠버네티스 오브젝트 명세(spec)과 상태(status)

* 쿠버네티스 오브젝트는 영속성을 가지는 엔터티(객체)로 쿠버네티는 클러스트의 상태값을 나타냄

* 쿠버네티스 오브젝트의 명세(spec)와 상태(status)를 선언하면, 시스템은 이러한 명세와 상태를 유지하기 위한 동작을 수행함


    ```shell
    apiVersion: apps/v1

    kind: Deployment

    # metadata
    metadata:
      name: echo-hname
      labels:
        app: nginx

    # spec
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: nginx
      template:

        # metadata
        metadata:
          labels:
            app: nginx

        # spec
        spec:
          containers:
          - name: echo-hname
            image: sysnet4admin/echo-hname%
    ```

<br/>


## Links

[kubernetes objects](https://v1-18.docs.kubernetes.io/ko/docs/concepts/overview/working-with-objects/kubernetes-objects/): 쿠버네티스 오브젝트 이해하기


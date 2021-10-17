---
layout  : wiki
title   : Kubernetes 파드의 C/U/D (디플로이먼트 생성/변경/삭제)
summary : 디플로이먼트로 쿠버네티스 파드 관리하기
date    : 2021-10-10 09:00:00 +0900
updated : 2021-10-10 09:00:01 +0900
tag     : kubernetes
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}


<img width="386" alt="deployment" src="https://user-images.githubusercontent.com/65143458/137607955-e96ae120-1a3d-462d-8574-eae940376aac.png">


<br/>

## 개 요

* 쿠버네티스 오브젝트 중 하나인 디플로이먼트는 파드와 레플리카셋에 대한 선언적 업데이트를 통해, 파드의 생성과 파드의 개수를 관리함

* 디플로이먼트 오브젝트로 선언된 상태(파드의 개수 등)를 유지하기 위해 필요한 작업이 back-ground에서 수행됨

* 선언된 상태는 one-liner cli나 yaml spec 파일 적용을 통해 업데이트할 수 있음

* 노드에 문제가 발생하여 관리가 필요한 경우 cordon, drain 등의 명령어를 통해 노드의 상태를 변경 불가 상태로 만들거나, 파드를 교체할 수 있음


<br/>

## [파드 / 디플로이먼트] 생성(Create) 방법

* 파드

    * run 명령어

        ```shell
        $ kubectl run nginx-pod --image=nginx
        ```

* 디플로이먼트

    * create deployment 명령어

        ```shell
        $ kubectl create deployment nginx-deployment --image=nginx
        ```

    * apply 명령어

        ```shell
        $ kubectl apply -f ./nginx-deployment.yaml
        ```

## [~~파드~~ / 디플로이먼트] 변경(Update) 방법 

* 파드 개수 변경하기

    * scale 명령어

    ```shell
    $ kubectl scale deployment <이름> --replicas=<숫자>
    ```


    * apply 명령어

    ```shell
    $ kubectl apply -f <이름> ./<파일명>.yaml
    ```


## [파드 / 디플로이먼트] 삭제(Delete) 방법

* 파드

    * run 명령어로 생성된 파드 삭제

        ```shell
        $ kubectl delete pod <이름>

        ```

* 디플로이먼트

    * create deployment / apply 명령어로 생성된 디플로이먼트 삭제

        ```shell
        $ kubectl delete deployment <이름>

        ```

## 쿠버네티스의 동작보증(self-healing)

* 동작보증 적용 대상 레이어

    * Application Layer
        
        * 쿠버네티스가 동작 보증을 제공하는 레이어

        * 애플리케이션이 적절하게 컨테이너화 되어 파드에 배포되었을 경우 쿠버네티스가 crash에 대응하여 reschedule을 수행함

    * Kubernetes Component Layer

        * 쿠버네티스 컴포넌트의 정상 동작을 확인하는 별도의 Agent 필요 (Kublr)

    * Infrastructure Layer

        * 쿠버네티스 클러스터가 배포된 환경(인프라스트럭처)에 장애가 발생하여 노드가 정상작동 하지 않을 시, 이에 대응할 수 있는 외부 모니터링 시스템(Kublr)이 필요함


## 동작보증 작동방식 제어


* 오토스케일링 대상에서 제외하기(cordon)

    ```shell
    $ kubectl cordon <노드>
    ```

* 노드에 할당된 파드를 제거하기(drain)

    ```shell
    $ kubectl drain <노드> --ignore-daemonsets
    ```

<br/>

## Links

[self-healing](https://kublr.com/blog/reliable-self-healing-kubernetes-explained/): Reliable, Self-Healing Kubernetes Explained




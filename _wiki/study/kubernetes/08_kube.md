---
layout  : wiki
title   : 부하를 분산하여 라우팅하는 로드밸런서와 부하량에 파드를 조절하는 HPA (수평 스케일러)
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


![kubernetes service](https://user-images.githubusercontent.com/65143458/138579431-94a0dbfd-6184-4e7c-ab39-2f5ecb056089.png)



<br/>

## 개 요

* 클러스터 내부에 접근하기 위한 


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




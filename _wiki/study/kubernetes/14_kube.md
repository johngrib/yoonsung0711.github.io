---
layout  : wiki
title   : 지속적인 통합 / 배포 
summary : 쿠버네티스 볼륨 오브젝트
date    : 2021-11-21 09:00:00 +0900
updated : 2021-11-21 09:00:01 +0900
tag     : kubernetes
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}

<br/>

## 개 요

* 파드(컨테이너)는 라이프사이클이 일시적(ephermeral)이므로 파드에서 실행되는 상태 비저장(stateless) 애플리케이션의 상태를 영속적으로 저장할 수 있는 방법이 필요함

* 쿠버네티스는 컨테이너가 아닌 파드에 종속되는 데이터 저장소를 생성(PV)하고 이를 필요한 만큼 파드(여러 컨테이너)에 할당(PVC)하는 객체를 이용해 클러스터의 영속성을 유지함

<br/>

## CI / CD 도구 비교

* TeamCity

    * 젯브레인즈에서 만든 CI / CD 도구
        
    * Kotlin DSL 사용

    * 에이전트 3개, 빌드 작업 100개 무료 사용 가능

* Github Action 

    * Github에서 만든 Workflow 기반의 CI / CD 도구

    * yaml 사용

    * 깃헙 저장소에 소스 코드 공개시 무료 사용 가능

* Bamboo 
    
    * Atlassian에서 만든 CI / CD 도구

    * Jira, Bitbucket과 연계성이 좋음

* Jenkins

    * 오픈 소스 CI / CD 도구

    * 다양한 사용 환경, 언어 및 빌드 도구와 연계 가능

* Jenkins X

    * 장애가 나서 서비스가 내려갔을 경우 그동안의 git webhook 이벤트를 받을 수 없음 => 개선

    * Disk 공간을 사람이 직접 비워주고 관리를 해주어야 함 => 개선

    * Plugin 버전이 맞지 않는 경우 장애가 발생 => 개선

    * Branch 가 여러개일 경우 속도가 느려짐 => 개선

    * JVM으로 인해 사용중이지 않을 경우에도 메모리를 잡아먹으며 클라우드 환경에서는 불필요한 비용이 발생 => 개선


## Links


[Medium](https://ystatit.medium.com/k8s-pv-pvc-and-configmap-for-prometheus-and-grafana-caa044b0d82b): K8S — PV, PVC and ConfigMap for Prometheus and Grafana




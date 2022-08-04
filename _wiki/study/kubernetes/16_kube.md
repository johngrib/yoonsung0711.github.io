---
layout  : wiki
title   : 지속적인 통합 / 배포 
summary : 쿠버네티스 볼륨 오브젝트
date    : 2021-11-28 09:00:00 +0900
updated : 2021-11-28 09:00:01 +0900
tag     : kubernetes
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}


![helm_vs_kustomize](https://user-images.githubusercontent.com/65143458/145711040-4d5ac070-6d98-441c-8478-da853ad88a4d.png)

<br/>

## 개 요

* 쿠버네티스 클러스터에 컨테이너를 배포하는 것을 돕는 도구들

<br/>

## 배포 간편화 도구

* Kubectl

    * 쿠버네티스에 기본으로 포함되어 있음

    * 개별 오브젝트를 관리하거나 배포할 때 사용하면 좋음

* Kustomize

    * 오브젝트를 사용자의 의도에 따라 변경하여 배포 가능함

    * kubectl로도 배포할 수 있는 옵션을 제공함

* Helm 
    
    * 쿠버네티스 사용자의 70% 이상이 사용하고 있을 정도로 널리 알려진 도구임

    * 오브젝트 배포에 필요한 사용이 이미 정의된 차트라는 패키지를 활용함

    * 헬름 차트는 자체적인 템플릿 문법을 사용하므로 가변적인 인자를 손쉽게 적용할 수 있음








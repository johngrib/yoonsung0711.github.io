---
layout  : wiki
title   : Ansible로 Kubernetes 배포 환경 조성하기
summary : remote server에 provisioning 가능한 IaC 툴
date    : 2021-10-3 22:56:28 +0900
updated : 2021-10-3 22:56:29 +0900
tag     : kubernetes
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}

![ansible](https://user-images.githubusercontent.com/65143458/135781370-3b03601b-08c8-452e-8e6e-f35f6ad5977e.png)


## 개 요

* Ansible는 선언형 DSL로 코드로 remote server의 시스템 구성을 자동화하여 관리하는(Infrastructure as Code) 프로비저닝 소프트웨어임
    
## 구성 방법

* Ansible 설치시에는 python이 사전에 구비되어 있어야 하며, 현재는 하위 호환성 유지를 위해 구동시 python을 사용하나 python3로 대체될 예정임

    ```shell
    $ pip3 install ansible
    ```

* Ansible 명령어



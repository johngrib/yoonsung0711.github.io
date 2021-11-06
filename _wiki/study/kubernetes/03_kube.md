---
layout  : wiki
title   : Ansible과 Terraform으로 Kubernetes 배포 환경 조성하기
summary : remote server에 provisioning 가능한 IaC 툴
date    : 2021-10-03 09:00:00 +0900
updated : 2021-10-03 09:00:01 +0900
tag     : kubernetes
toc     : true
public  : true
parent  : [[/index]]
latex   : true
---
* TOC
{:toc}

![ansible](https://user-images.githubusercontent.com/65143458/135781370-3b03601b-08c8-452e-8e6e-f35f6ad5977e.png)

![Terraform](https://user-images.githubusercontent.com/24386223/136678794-1969002a-be10-473b-8078-dc3319565b0c.jpg)


## 개 요

* Ansible는 선언형 DSL로 코드로 remote server의 시스템 구성을 자동화하여 관리하는(Infrastructure as Code) 프로비저닝 소프트웨어임

* Terraform 역시 선언형 DSL로 코드로 remote server의 시스템 구성을 관리하지만 Ansible과는 달리 컨테이너 배포 관리에 특화된 orchestration tool로써 동작함
    
## 구성 방법

* Ansible 설치시에는 python이 사전에 구비되어 있어야 하며, 현재는 하위 호환성 유지를 위해 구동시 python을 사용하나 python3로 대체될 예정임

    ```shell
    # python

    $ pip3 install ansible --user

    # ansible을 찾지 못한다면 아래와 같이 경로를 추가함 
    $ echo 'export PATH="/Users/<username>/Library/Python/3.8/bin:$PATH"' >> ~/.zshrc
    ```

    * 글을 쓰는 현재 2021. 10의 Ansible의 버전은 4.6.0 (core 2.11.5)임

* Terraform은 아래와 같이 hashicorp의 저장소를 추가한 뒤 brew로 간단하게 설치가 가능함

    ```shell
    $ brew tap hashicorp/tap

    $ brew install hashicorp/tap/terraform
    ```

    * 글을 쓰는 현재 2021. 10의 Terraform 버전은 v1.0.8임

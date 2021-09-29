---
layout  : wiki
title   : 컨테이너 인프라를 사용하는 이유
summary : 마이크로서비스와 컨테이너 인프라 환경
date    : 2021-09-25 22:56:28 +0900
updated : 2021-09-25 22:56:29 +0900
tag     : kubernetes
toc     : true
public  : true
parent  : [[/study/kubernetes/index]]
latex   : true
---
* TOC
{:toc}

## 개 요

* 마이크로서비스 구축 시 컨테이너 인프라 환경을 선택하게 되는 현실적인 이유
    
    * 아래와 스크립트 코드와 컨테이너 코드는 유사한 기능을 수행하지만, 코드량에서 비교하기 어려울 정도로 큰 차이가 남

        * [스크립트 코드](https://github.com/yoonsung0711/automated_ci_cd/tree/main/subprojects) 

        * [컨테이너 코드](https://github.com/yoonsung0711/automated_ci_cd/blob/main/docker-compose.yml)


    * 스크립트로 배포/테스트/구동을 제어할 경우 각각의 서비스별로 필요한 준비 과정이 조금씩 달라 스크립트 모듈화에 어려움이 있음

    * 이에 따라 스크립트 작업시에는 서비스 종류만큼 스크립트를 작성해야 하고, 인스턴스 간 실행 순서 등을 제어하기 위해 healthcheck, wait-on 과 같은 기능을 직접 구현해야 하므로 복잡성이 증가하고, 유지 보수에 어려움이 있을 수 있음

    * domain, port 등의 설정 외부화가 가능하나, 파일을 분할하여 관리해야 하므로 docker-compose 처럼 설정이 한 곳에서 중앙 관리되지 않음 


* 개발 과정에서도 컨테이너 인프라 환경을 선호하게 되는 현실적인 이유

    * 피쳐 중심의 개발을 할 때에도 여러 서비스가 협력하여 하나의 기능으로 동작하기 때문에, 작업 대상이 아닌 서비스들도 직접 구동해야 함

        ```shell
        #!/usr/bin/env bash

        (cd subprojects/service-registry && yarn start &)
        sleep 5
        (cd subprojects/service-api && yarn start &)
        (cd subprojects/service-front && yarn start &)
        (cd subprojects/service-realtime && yarn start &)
        (cd subprojects/service-gateway && yarn start &)
        ```

    * docker-compose는 단일 서비스 개발 시에도 아래와 같은 간단한 명령어로 여러 서비스들을 구동/중단할 수 있음

        ```shell
        $ docker-compose -f docker-compose.dev.yml up
        $ docker-compose -f docker-compose.dev.yml down
        ```

## 요약

* 모노리스 vs 마이크로서비스는 전략적 선택의 결과로 정해짐

* 하지만 마이크로서비스를 선택한 시점부터는, 컨테이너 인프라 환경 구축은 선택이 아니라 필연적인 결과일 수 밖에 없음


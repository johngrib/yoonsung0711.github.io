---
layout  : wiki
title   : 도커 레지스트리 구축하기
summary : 쿠버네티스 클러스터에 이미지 공유하기
date    : 2021-11-21 11:00:00 +0900
updated : 2021-11-21 11:00:01 +0900
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

* 직접 생성한 이미지를 쿠버네티스 노드에 배포하기 위해 모든 노드에서 공통으로 접근 가능한 사설 레지스트리를 구축함

<br/>

## 레지스트리 비교

* Quay

    * Red Hat에서 개발중인 오픈소스 프로젝트

    * 서비스형, 설치형 제공
        
    * 유료

* Harbor

    * 클라우드 네이티브 컴퓨팅 재단에서 지원하는 Project Harbor가 개발중인 오픈소스 프로젝트

    * 도커 이미지 외 헬름 차트 저장 가능

    * 무료

* Nexus Repository 
    
    * Sonartype에서 개발중인 오픈소스 프로젝트

    * 도커 이미지 외 리눅스 설치 패키지, 자바 라이브러리, 파이썬 라이브러리 등의 파일 저장소 역할 수행

    * 무료 / 유료 (기술 지원 + 기능 추가)

* Docker Registry

    * Docker에서 개발중인 오픈소스 프로젝트

    * 도커 이미지만 저장 가능

    * 무료

## 인증 서명 요청과 인증서

* CSR (Certificate Signing Request)

    * 공개키 생성을 위해 기재하는 항목들

    * 항목: 국가명, 주소, 회사명, 부서명, 도메인명, 이메일 등

    ```shell
    # Common Name (CN): The fully qualified domain name (FQDN) of your server.

    # Organization (O): The legal name of your organization. Do not abbreviate and include any suffixes, such as Inc., Corp., or LLC.

    # Organizational Unit (OU):  The division of your organization handling the certificate.

    # City/Locality (L): The city where your organization is located. This shouldn’t be abbreviated.

    # State/County/Region (S): The state/region where your organization is located. This shouldn't be abbreviated.

    # Country (C): The two-letter code for the country where your organization is located.

    # Email Address: An email address used to contact your organization.
    ```

* CRT (Certificate)

    * 안전한 통신을 위해 전송계층과 응용계층 사이에 별도의 계층인 SSL(Secure Socket Layer)을 만들어 데이터를 암호화/복호화 함

    * 웹 서버와 클라이언트 간의 안전한 연결을 위해 SSL Handshake 프로세스를 이용하는데, 이때 사용되는 인증서를 의미함

    * 서드파티 공급업체가 제공하는 CA 인증서(공인인증서)와 openssl를 이용한 사설 인증서가 있음

## 사설 레지스트리 운영 방법

* 레지스트리 생성하기

    ```shell
    $ docker run -d \ 
        --restart=always \
        --name registry \
        -v /etc/docker/certs:/docker-in-certs:ro \
        -v /registry-image:/var/lib/registry \
        -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
        -e REGISTRY_HTTP_TLS_CERTIFICATE=/docker-in-certs/tls.crt \
        -e REGISTRY_HTTP_TLS_KEY=/docker-in-certs/tls.key \
        -p 8443:443 \
        registry:2
    ```

* 레지스트리에 이미지 등록하기

    ```shell
    $ docker push <사설 레지스트리 ip>:8443/<이미지명>
    ```

* 레지스트리에 등록된 이미지 사용하기

    ```shell
    $ docker pull <사설 레지스트리 ip>:8443/<이미지명>
    ```

* 쿠버네티스 spec을 이용해 사설 레지스트리에 등록된 이미지 배포하기

    ```yaml
    spec:
        containers: 
        - image: <사설 레지스트리 ip>:8443/<이미지명>
          name: <컨테이너명>
    ```


## Links


[Medium](https://ystatit.medium.com/k8s-pv-pvc-and-configmap-for-prometheus-and-grafana-caa044b0d82b): K8S — PV, PVC and ConfigMap for Prometheus and Grafana



